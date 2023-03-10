# 11장. 시스템

## 시스템 제작과 시스템 사용을 분리하라 (p.194)

```md
제작과 사용은 아주 다르다.

소프트웨어 시스템은 (애플리케이션 객체를 제작하고 의존성을 서로 '연결'하는) 준비 과정과
(준비 과정 이후에 이어지는) 런타임 로직을 분리해야 한다.

체계적이고 탄탄한 시스템을 만들고 싶다면 흔히 쓰는 좀스럽고 손쉬운 기법으로 모듈성을 깨서는 절대로 안 된다.

설정 논리는 일반 실행 논리와 분리해야 모듈성이 높아진다.
```

## ABSTRACT FACTORY 패턴 (p.197)

찾아본 결과, 팩토리 메서드 패턴을 좀 더 캡슐화한 방식로 나옴

**팩토리 메서드 패턴**

- 조건에 따른 객체 생성을 팩토리 클래스로 위임하여, 팩토르 클래스에서 객체를 생성하는 패턴

**추상 팩토리 패턴**

- 서로 관련이 있는 객체들을 통째로 묶어서 팩토리 클래스로 만들고, 이들 팩토리를 조건에 따라 생성하도록 다시 팩토리를 만들어서 객체를 생성하는 패턴

## 의존성 주입 (p. 198)

```md
제어 역전에서는 한 객체가 맡은 보조 책임을 새로운 객체에게 전적으로 떠넘긴다.

새로운 객체는 넘겨받은 책임만 맡으므로 단일 책임 원칙을 지키게 된다.
```

[Q. 프런트엔드에서의 DI 개념?](https://sac4686.tistory.com/55)
- React의 Context API
  - 소비자/ 공급자로서 "필요한 것"과 "인스턴스화"를 구분
- DI를 명확하게 제시함으로서 유지보수성을 높이고 VIEW끼리의 서로간의 의존성을 낮출 수 있다.

## 테스트 주도 시스템 아키텍처 구축 (p. 210)

```md
코드 수준에서 아키텍처 관심사를 분리할 수 있다면, 진정한 테스트 주도 아키텍처 구축이 가능해진다.
```

## 명백한 가치가 있을 때 표준을 현명하게 사용하라 (p. 212)

```md
표준을 사용하면 아이디어와 컴포넌트를 재사용하기 쉽고, 적절한 경험을 가진 사람을 구하기 쉬우며,
좋은 아이디어를 캡슐화하기 쉽고, 컴포넌트를 엮기 쉽다.

하지만 때로는 표준을 만드는 시간이 너무 오래 걸려 업계가 기다리지 못한다.

어떤 표준은 원래 표준을 제정한 목적을 잊어버리기도 한다.
```

# 12장. 창발성

## 단순한 설계 규칙 네 가지 (feat. 켄트 백 ) (p. 216)

- 모든 테스트를 실행한다.
- 중복을 없앤다.
- 프로그래머 의도를 표현한다.
- 클래스와 메서드 수를 최소로 줄인다.

"클래스와 메서드 수를 최소로 줄인다." 이 부분은 조금 의아하다. 앞장에서는 작게 나누는 것이 좋다고 나오고...
그냥 코드 자체를 줄이라는 말인지..🤔

## 단순한 설계 규칙 1. 모든 테스트를 실행하라 (p.216 ~ p.217)

```md
테스트가 가능한 시스템을 만들려고 애쓰면 설계 품질이 더불어 높아진다. 크기가 작고 목적 하나만 수행하는 클래스가 나온다.

결합도가 높으면 테스트 케이스를 작성하기 어렵다.

"테스트 케이스를 만들고 계속 돌려라"라는 간단하고 단순한 규칙을 따르면 낮은 결합도와 높은 응집력이라는,
객체 지향 방법론이 지향하는 목표를 저절로 달성한다.

즉, 테스트 케이스를 작성하면 설계 품질이 높아진다.
```

## 단순한 설계 규칙 2~4. 리팩터링 (p.217)

```md
테스트 케이스를 모두 작성했다면 이제 코드와 클래스를 정리해도 괜찮다.

코드를 정리하면서 시스템이 깨질까 걱정할 필요가 없다. 테스트 케이스가 있으니까!
```

## 중복을 없애라 (p.217 ~ p.219)

```md
중복은 추가 작업, 추가 위험, 불필요한 복잡도를 뜻하기 때문에 우수한 설계에서 커다란 적이다.
```

## 표현하라 (p.221)

- 좋은 이름을 선택한다.
- 함수와 클래스 크기를 가능한 줄인다.
- 표준 명칭을 사용한다.
- 단위 테스트 케이스를 꼼꼼히 작성한다.

## 클래스와 메서드 수를 최소로 줄여라 (p.222)

```md
중복을 제거하고, 의도를 표현하고, SRP를 준수한다는 기본적인 개념도 극단으로 치달으면 득보다 실이 많아진다.

클래스와 메서드 크기를 줄이자고 조그만 클래스와 메서드를 수없이 만드는 사례도 없지 않다.

가능한 실용적인 방식을 택한다.

목표는 함수와 클래스 크기를 작게 유지하면서 동시에 시스템 크기도 작게 유지하는 데 있다.
```

# 13. 동시성

## 동시성이 필요한 이유? (p.226)

```md
동시성은 결합을 없애는 전략이다. 즉, 무엇과 언제를 분리하는 전략이다.

무엇과 언제를 분리하면 애플리케이션 구조와 효율이 극적으로 나아진다.

거대한 루프 하나가 아니라 작은 협력 프로그램 여럿으로 보인다.

따라서 시스템을 이해하기가 쉽고 문제를 분리하기도 쉽다.
```

## 단일 책임 원칙 (p.230)

```md
메서드/ 클래스/ 컴포넌트를 변경할 이유가 하나여야 한다는 원칙

즉, 동시성 관련 코드는 다른 코드와 분리해야 한다는 뜻
```

동시성을 구현할 때 고려하는 몇 가지

- 동시성 코드는 독자적인 개발, 변경, 조율 주기가 있다.
- 동시성 코드에는 독자적인 난관이 있다. 다른 코드에서 겪는 난관과 다르며 훨씬 어렵다.
- 잘못 구현한 동시성 코드는 별의별 방식으로 실패한다. 주변에 있는 다른 코드가 발목을 잡지 않더라도 동시성 하나만으로도 충분히 어렵다.