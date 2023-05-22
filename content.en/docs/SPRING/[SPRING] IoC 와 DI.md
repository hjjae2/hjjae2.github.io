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

## IoC (Inversion of Control)

- 제어(관리)의 역전
- 프로그램에 대해서 개발자가 관리하는 것이 아닌, 제 3의 존재가 관리한다.

**IoC의 이점**
1. 역할 / 책임을 분리한다. (의존성 주입 등 관리의 일부를 제 3의 존재가 한다. 즉 역할이 분리되는 것이다.)
2. 유연하게 코드를 작성할 수 있다. (개발 코드에 집중할 수 있다. 편리하다.)
3. 개발자가 관리한다는 것은 권한도 강해짐을 의미한다. 이를 제어할 수 있다.

<br><br>

## DI (Dependencies Injection)

- IoC 이라는 큰 범주의 개념 중 구체적인 한 특징 (?)
- 제 3의 존재가 의존성을 주입/관리해준다. (이것 또한 제어의 역전이다.)
- Spring Framework 은 IoC 중에서도 DI 의 개념이 두드러진다.
- **DI는 클래스타입이 고정되어 있지 않고 인터페이스 타입의 파라미터를 통해 다이나믹하게 구현 클래스를 결정해서 제공 받을수 있어야 한다.** (즉, 동적으로, 다양하게 객체를 이용할 수 있어야 한다.)

<br>
<br>

**토비의 스프링에서 말하는 DI 조건 3가지**
1. 클래스 모델, 코드에는 런타임 시점의 의존 관계가 드러나지 않는다. 그러기 위해서는 인터페이스에만 의존하고 있어야 한다. (?)
2. 런타임 시점의 의존 관계는 컨테이너나 팩토리 같은 제 3의 존재가 결정한다.
3. 의존 관계는 사용할 객체에 대한 참조를 외부에서 주입해줌으로써 만들어진다.

<br>

> Reference
> 1. https://biggwang.github.io/2019/08/31/Spring/IoC,%20DI%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C/