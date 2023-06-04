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

**1. getter 대신에 객체에 물어보는 형태로 작성해보자.**

\* 적절한 예시인지 모르겠다. 아무튼 getter 보다 최대한 객체의 method 를 활용해보는 것? 이 포인트 인 것 같다.
```
Man man = new Man();

...

// A
if(man.getAge() > 10) {
    ...
}

// B
if(man.isOverTenAge()) {
    ...
}
```

<br>

**2. 될 수 있으면 숫자, 문자열 값에 대한 상수(변수) 처리를 하자.<br>혹은 Enum 클래스를 사용하도록 하자.**

<br>

**3. 한 테스트 함수(코드)에서 여러 개를 테스트하지 말자.<br>
n 개를 테스트하고자 한다면, n 개의 테스트 함수를 만들자.**

<br>

**4. 리뷰 사항 중 SCP 에 대한 코멘트가 종종 있었다. 한 클래스가 너무 많은 역할을 하는 것은 아닌지 확인하자.**

<br>

**5. 리뷰 사항 중 전략 패턴에 대한 코멘트가 있었다. 전략 패턴에 대해 공부해보고, 적용해보자.**

<br>

**6. Interface 가 정말 필요한 지에 대해 생각해보자.**

예를 들어, BoardService 라는 service 클래스가 있을 때 이것을 BoardService(I), BoardServiceImpl 의 형태로 작성한 코드가 있다. 여러 구현체가 공통적으로 묶이지 않는 상황에서 정말 필요한 것인지? 에 대해 생각해봐야한다. 

그리고 ~~~Impl 이라는 명칭도 좋지 않은 것 같다라는 피드백이 있었다.

<br>

**7. Composition 에 대해 주의깊게 생각해보자.**

예를 들어, `Man 클래스`에 `xPosition`, `yPosition` 이 있다면 `Position class` 로 빼는 것은 어떤지? 에 대해 생각해볼 수 있다.

<br>

**8. 일급 컬렉션에 대해 생각해보자.**

<br>

**9. Optional 을 활용하자.**

단, 아래와 같은 피드백도 있었다.

> _" Optional의 경우 필드에 사용을 지양해야 합니다. 정확히는 사용하면 안됩니다. 그 이유로 Optional은 변수의 null 유무를 파악하기 위해 나왔습니다. 더 정확히는 메서드의 반환값의 null 유무를 체크하기 위해 설계했기 때문에 메서드의 반환에만 사용하도록 지향해야 합니다. Optional에 대해서는 더 공부해보시면 좋을 것 같습니다:) 예를 들어, 자바 8 인 액션 책을 살펴보시면 좋습니다. "_

<br>

**10. NPE 에 대해 주의하자.**

가령 아래와 같은 코드가 있다.

```
...

if(book.equals("comic")) {
    ...
}
```

위 코드를 아래와 같이 개선할 수 있다.

```
// ("comic" 을 변수 처리하는 것은 생략)
...

if("comic".equals(book)) {
    ...
}
```