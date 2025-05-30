---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

## tl;dr

C/B 성공, 실패 횟수를 수동으로 집계하지 말자. \
수동 집계가 필요하다면 의도한대로 집계되고 있는지 꼭 확인해보자.

## 🔖 배경

WebClient 에 적용한 C/B 의 성공, 실패 집계가 예상했던 결과와 다르게 집계되는 문제가 발견됐다.

예를 들어,

* 애플리케이션의 기준에 따라 요청이 성공한 경우 성공 횟수는 1회 증가해야 한다.
* 반대로, 요청이 실패한 경우 실패 횟수는 1회 증가해야 한다.

발견한 문제는, 요청이 성공한 경우 성공 횟수는 2회 증가하고 요청이 실패한 경우 성공, 실패 횟수가 각각 1회 증가하는 현상이다. 단, 모든 상황에서 이상 집계가 발생한 것은 아니다. 요청 실패의 종류에 따라 정상 집계되기도, 이상 집계되기도 한다.

예를 들어,

* 요청 과정에서 어느 이유로건 Exception 이 발생한 경우 (Mono.error 로 처리되어)실패 횟수만 1회 증가한다. — (1)
* 요청은 성공했으나 응답의 HTTP 상태코드가 4xx, 5xx 인 경우 성공 횟수, 실패 횟수가 각각 1회씩 증가한다. 의도한 처리는 실패 횟수만 1회 증가이다. — (2)

> NOTE: 애플리케이션 정책으로 HTTP 상태코드 4xx, 5xx 는 에러로 취급하고 에러 횟수를 증가한다.

조치해야 할 부분은 (2) 의 내용이다.


## 💭 원인

아래는 WebClient 에 C/B 설정 추가를 위해 사용한 Customizer 코드이다.

```kotlin
class CircuitBreakerFilterWebClientCustomizer : WebClientCustomizer {
    override fun customize(webClientBuilder: WebClient.Builder) {
        webClientBuilder
            .filter { request, next ->
                val cb = CircuitBreaker Object
                        next.exchange(request)
                            .elapsed()
                            .flatMap {
                                ...
                                if (statusCode.isError) {
                                    cb.onError(...)
                                } else {
                                    cb.onSuccess(...)
                                }
                                Mono.just(...)
                            }
                            .transformDeferred(CircuitBreakerOperator.of(cb))
            }
    }
}
```

애플리케이션 실패 기준에 따라 HTTP 상태코드가 에러(4xx, 5xx)인 경우 ‘실패’로 취급하고 있고 수동으로 C/B의 성공, 실패 횟수를 집계 처리해주고 있다. **문제는 이 부분이다.**

`CircuitBreakerOperator` 를 적용하면 조건에 따라 C/B 의 성공, 실패 횟수를 집계해주게 되는데, 수동으로 C/B 횟수 집계 로직을 추가하면서 2번의 집계 처리가 발생하게 된다. 🚨

> `CircuitBreakerOperator` 로직을 따라가보면 나오는 `CircuitBreakerStateMachine.onResult(...)` 메서드에서 내용을 확인할 수 있다.

```kotlin
// CircuitBreakerStateMachine.java
@Override
public void onResult(long duration, TimeUnit durationUnit, @Nullable Object result) {
    if (result != null && circuitBreakerConfig.getRecordResultPredicate().test(result)) {
        ...
        stateReference.get().onError(duration, durationUnit, failure);
    } else {
        onSuccess(duration, durationUnit);
    }
}
```

## ✅ 조치

수동으로 집계 처리하지 말자.

구체적으로,

1. 조건에 따라 Mono.just 를, Mono.error 를 반환해주면 `CircuitBreakerOperator` 가 집계해준다.
2. 혹은 C/B 설정
   중 [recordResult](https://github.com/resilience4j/resilience4j/blob/master/resilience4j-circuitbreaker/src/main/java/io/github/resilience4j/circuitbreaker/CircuitBreakerConfig.java#L851)
   로 실패로 판단할 조건을 추가해줄 수 있다.
    1. 우리 애플리케이션에서는 Mono.error 처리 시 기작성된 `WebClient` 의 객체 변환 코드(onStatus)를 함께 수정해줘야 하는 번거로움이 있었기에 이 방안을 고려해볼 수 있다.
