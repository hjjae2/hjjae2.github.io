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

사람이 임의로 모였을 때 적어도 생일이 같은 두명이 존재할 확률을 구하는 문제 \
23명 이상이 모일 경우 두 명의 생일이 같은 확률은 1/2 를 넘는다.

## 계산

> * 1번째 사람: 아무 생일이나 가능 → 365/365
> * 2번째 사람: 1번째와 달라야 함 → 364/365
> * ...
> * N번째 사람: 앞의 N-1명과 달라야 함 → (365 - (N-1)) / 365

### 적어도 두 명의 생일이 같은 확률

$$= P(\text{collision}) = 1 - \frac{365}{365} \cdot \frac{364}{365} \cdots \frac{365-k+1}{365}$$

$$= P(\text{collision}) = 1 - \prod_{i=0}^{k-1} \left( \frac{365 - i}{365} \right)$$

### 근사식 1

> * 슬롯 개수 : n
> * 요소 개수 : k

$$\ln P_{\text{no-collision}} = \sum_{i=0}^{k-1} \ln\left(1 - \frac{i}{N}\right) \approx -\sum_{i=0}^{k-1} \frac{i}{N}$$

$$\ln P_{\text{no-collision}} \approx -\frac{1}{N} \sum_{i=0}^{k-1} i$$

$$\ln P_{\text{no-collision}} \approx -\frac{k(k-1)}{2N}$$

$$P_{\text{no-collision}} \approx \exp\left(-\frac{k(k-1)}{2N}\right)$$

$$P_{\text{collision}} \approx 1 - \exp\left(-\frac{k(k-1)}{2N}\right)$$

### 근사식 2

> $$x = \frac{k(k-1)}{2N}$$

$$P_{\text{collision}}$$
$$\approx 1 - e^{-x} \approx x \quad (\text{if } x \ll 1)$$
$$\approx 1 - \exp\left(-\frac{k(k-1)}{2N}\right)$$
$$\approx \frac{k(k-1)}{2N} \quad$$
$$\approx \frac{k^2}{2N}$$

> 단, 근사 조건: \
> $\frac{k^2}{2N} \ll 1$ \
> $k^2 \ll 2N$

