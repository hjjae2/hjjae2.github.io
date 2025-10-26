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

# 설명

https://aws.amazon.com/ko/lambda/edge/

요약하면, CF(CloudFront) 엣지 로케이션에서 수행하는 AWS Lambda 함수를 의미한다.

> CloudFront Functions 라는 것도 있는데, 차이는 [이 문서](https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/edge-functions-choosing.html)에서
> 확인할 수 있다. CloudFront Functions 는 상대적으로 간단한 작업에 적합하다. Lambda@Edge 에 비해 기능에 제약이 있다.

Lambda@Edge 는 CF 의 이벤트에 반응하여 실행된다. 다음 4가지 이벤트가 있다.

- Viewer Request : 사용자 → CF 호출 구간
- Origin Request : CF → 오리진 서버 호출 구간
- Origin Response : 오리진 서버 → CF 응답 구간
- Viewer Response : CF → 사용자 응답 구간

# 참고

여러 제약이 있는데, 내가 인지해야 할 제약은 다음 정도가 있었다.

| 내용                                                                                                                                                                                                       | 참고                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lambda 생성 시 지역은 ‘us-east-1’ 생성한다.                                                                                                                                                                        | https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-how-it-works-tutorial.html#lambda-edge-how-it-works-tutorial-create-function                             |
| Lambda@Edge의 아키텍처는 x86_64 로 동작해야 한다.                                                                                                                                                                     |                                                                                                                                                                                               |
| Lambda@Edge 는 Layer 를 사용할 수 없다. 환경 변수를 사용할 수 없다.                                                                                                                                                         | https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/lambda-at-edge-function-restrictions.html                                                                            |
| Origin Response Event 의 경우 응답 본문이 포함되지 않는다. <br/> When you're working with the HTTP response, Lambda@Edge does not expose the body that is returned by the origin server to the origin-response trigger. | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-generating-http-responses.html#lambda-updating-http-responses                                                       |
| 계정 내 Lambda@Edge 를 연결한 CF 수 제한 : 500 * 상향 조절 가능                                                                                                                                                          | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-limits.html#limits-lambda-at-edge  <br> https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html |
| 동시 실행 수 : 1,000 per AWS Region * 상향 조절 가능                                                                                                                                                                |
| 동일 함수(Lambda)에 연결 가능한 CF 수 제한 : 500                                                                                                                                                                      |                                                                                                                                                                                               |
| 라이브러리를 포함한 람다 함수의 최대 크기 제한 : 50MB                                                                                                                                                                        |                                                                                                                                                                                               |
| 초당 호출량 제한 : 10,000 per AWS Region                                                                                                                                                                        |                                                                                                                                                                                               |
| 함수의 메모리 크기 제한 : 128MB ~ 10,240MB                                                                                                                                                                         |                                                                                                                                                                                               |
| 함수의 타임아웃 제한 : ~ 30s                                                                                                                                                                                      |                                                                                                                                                                                               |
| 헤더, 본문 응답 크기 제한 : 1MB                                                                                                                                                                                    |                                                                                                                                                                                               |