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

# 프레셰 거리 (Fréchet Distance)

두 곡선(경로)의 모양 유사성을 측정해볼 수 있다.

두 사물체가 서로 줄로 연결된 채로 각자의 경로로 길을 걸을 때, 줄에 필요한 최소한의 거리(=프레셰 거리)를 측정한다.

이때,

1. 두 사물체의 속도는 다를 수 있다.
2. 두 사물체는 뒤로 이동하지 않는다.
3. 두 사물체는 두 경로의 시작점에서 시작해야 한다.
4. 두 사물체는 두 경로의 끝점에서 끝나야 한다.

**줄의 길이(= 프레셰 거리)가 짧을수록 두 경로는 유사하다고 판단할 수 있다.**

> 줄이 늘어나지 않은 상태에서 두 사물체가 이동한다고 생각해보면, 두 사물체의 경로가 유사하다고 생각할 수 있다.

# 예시

https://medium.com/tblx-insider/how-long-should-your-dog-leash-be-ba5a4e6891fc

위 예시에서 아래의 조합이 발생할 수 있다.

```text
A-C // 사람 A, 개 C, Frechet 거리: 1
B-D // 사람 B, 개 D, Frechet 거리: 1
B-E // 사람 B(멈춤), 개 E, Frechet 거리: 2
```

```text
A-C // 사람 A, 개 C, Frechet 거리: 1
A-D // 사람 A(멈춤), 개 D, Frechet 거리: √2
B-E // 사람 B, 개 E, Frechet 거리: 2
```

두 조합에서 발생할 수 있는 가장 긴 거리 중 최소값(=프레셰 거리)은 2이다.
