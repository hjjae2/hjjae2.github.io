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

전달하는 이미지 크기를 줄일 수 있지만 디코딩 시 사용하는 리소스 사용량 측면도 고려하면, \
JPEG 를 WebP 로 완전 대체하기에는 무리가 있어 보인다.

## JPEG를 WebP로 대체할 수 있을까?

확인하고 싶었던 부분은 "기존에 사용하던 JPEG 를 WebP 로 대체해버리면 안될까?" 였다.

운영 중인 이미지 플랫폼의 CF 에서 일 평균 50TB ~ 75TB 정도의 데이터를 응답하는데, \
구글이 공개한 수치인 25~34% 절감 수치를 적용해보면 약 12.5TB ~ 25TB 정도의 데이터 절감 효과가 기대됐다.

결과적으로, 아직까지 완전한 대체는 불가한 것 같고 클라이언트에서 장치 환경에 따라 선택적으로 제공하도록 하는 수 밖에 없는 것 같다.

## 문제점?

"디코딩" 과정이 문제가 된다. (= 렌더링 장치의 리소스 사용량에 영향을 미치게 된다.)

이미지를 렌더링할 때, 이미지를 디코딩하는 과정이 필요하다.
이때 스마트폰과 같이 성능이 낮은 장치의 경우 하드웨어의 도움(= 하드웨어 가속)을 받게 되는데, \
WebP 의 경우 하드웨어 가속을 지원하지 않는 장치가 많다고(?) 볼 수 있다.

공식적인 문서를 찾지는 못했는데, 일부 문서를 참고해보면 Apple 의 장치들이 지원하지 않는다고 한다. (https://namu.wiki/w/WebP) \
Android 또한 공식적으로 지원한다는 내용은 없는 것 같다.

스마트폰의 성능이 좋아짐에 따라 이 차이가 점차 신경쓰이지 않게 될 수도 있겠지만, \
아직까지는 하드웨어 가속을 지원하고 있는 JPEG 를 완전 대체하기에는 무리가 있어 보인다.

## WebP

구글에서 개발한 이미지 파일 형식으로, 2010년에 발표됐다고 한다.
WebP 는 웹 환경에서 최적화된 이미지 지원을 위해 설계됐다.

WebP 무손실 이미지는 PNG 이미지에 비해 26% 더 작다고 하며, \
WebP 손실 이미지는 JPEG 이미지보다 25-34% 더 작다고 한다.

## WebP 작동 방식

손실 WebP 압축은 예측 코딩 방식을 사용해 이미지를 인코딩한다. \
예측 코딩은 인접한 픽셀 블록의 값을 사용해 블록의 값을 예측한 다음, 실제 값과의 차이(잔차)를 인코딩하는 방식이다.

잔차 값을 사용함으로써, 엔트로피 코딩 기법을 적용했을 때 더 효율적인 압축이 가능하게 된다. \
(자주 나타나는 값을 짧은 비트로 인코딩할 수 있기 때문이다.) 이런 손실 압축은 VP8 동영상 코덱에서 사용하는 것과 동일한 방식이라고 한다.

무손실 WebP 압축은 이미 본 이미지 파편을 사용해 새 픽셀일 구성하거나(gemini 물어보니 LZ77 정도를 생각하면 된다고 한다.) \
로컬 팔레트를 사용할 수 있다고 한다.

[RIFF 컨테이너 형식](https://developers.google.com/speed/webp/docs/riff_container?hl=ko&_gl=1*1g0s5yy*_up*MQ..*_ga*NjE1OTU4NzcxLjE3NjE5Mjk1MDk.*_ga_SM8HXJ53K2*czE3NjE5Mjk1MDgkbzEkZzAkdDE3NjE5Mjk1MDgkajYwJGwwJGgw#riff_file_format)
을 사용한다고 하는데 이건 따로 보자.

## 호환성

대부분의 모던 브라우저에서 WebP 형식을 지원한다. (https://caniuse.com/webp)

## References

* https://developers.google.com/speed/webp?hl=ko
* https://caniuse.com/webp