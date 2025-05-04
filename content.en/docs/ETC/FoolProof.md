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

# What is ‘Fool Proof’ ?

어리석은 사람도 사용할 수 있도록 한다. 잘못 사용하기 어렵게 한다.

기본적으로 메서드, 클래스 등의 ‘코드’는 잘못 사용하기 어렵게(Fool Proof) 작성한다.

# How to make ‘Fool Proof’ code?

1. 내부에서 검증한다.

클래스, 메서드 내부에서 외부에서 인입되는 값을 검증한다. 외부로부터 인입되는 값은 신뢰하지 않는다.

2. 타입(Class)으로 보장한다.

잘못된 값이 인입되지 않도록 타입으로 값의 범위를 강제한다.\
\* 해당 타입은 1번이 전제될 것이다.

3. 직관적인 이름을 사용한다.

다른 의도로 이해할 수 있는 상황을 제거한다.
