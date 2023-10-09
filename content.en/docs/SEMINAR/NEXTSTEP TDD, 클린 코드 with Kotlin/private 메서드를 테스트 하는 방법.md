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

## 1. 클래스로 추출한다.

> 보통 private 메서드는 SRP 가 잘 지켜지는 메서드(단순한고, 작은 메서드)로 구성되는 경우가 많다.

private 메서드에 포함된 로직을 처리하는 별도의 클래스로 분리할 수 있다.

이렇게 생성된 클래스는 역시나 SRP 가 잘 지켜지는 클래스가 되고, 해당 로직을 public 메서드로 바꿔도 어색함이 없다.

## 2. private 메서드를 포함한 public 메서드를 테스트한다.

간접적으로 테스트하는 것이다. 

> 당연하게도(?) 1번이 더 좋겠다.

## 3. public 으로 변경한다.

> 당연하게도(?) 1, 2번이 더 좋겠다.