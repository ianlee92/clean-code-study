# 11장

도시 - **추상화**와 **모듈화** 덕분에 큰 그림을 이해하지 못할지라도 개인이 관리하는 구성요소는 효율적으로 돌아감

⇒ 시스템 수준에서도 코드를 깨끗하게 유지하는 방법

소프트웨어 시스템은 준비 과정과 런타임 로직을 분리해야 한다

시스템… 관심사의 분리!

**특정한 관심사에 따라 기능을 나누고, 각 기능을 독립적으로 개발한 뒤 이를 조합하는 방식**

관심사의 분리(SoC)는 소프트웨어 개발에서 가장 기본적인 원칙 중 하나이며, SOLID 원칙 5개 중 2개(단일 책임 및 인터페이스 분리)가 이 개념에서 직접 파생될 정도로 매우 중요하다.

원칙은 간단하다. 프로그램을 하나의 단일 블록으로 작성하지 말고 작은 조각으로 나누어 각각 간단한 개별 작업을 완료할 수 있도록 만드는 것이다.

## 관심사의 분리 장점

- 코드가 더욱 명료해짐 : 자신이 어떤 일을 하고, 어떤 목적을 가지고 설계된 코드인지 보다 잘 드러나게 됨.
- 코드 재사용성이 올라감 : 여러 역할이 엉켜있는 코드보다, 역할 별로 잘 분리되어 있는 코드를 재사용하기가 쉬움.
- 유지 보수가 용이함 : 변경 사항이 발생했을 때 해당 관심사에 연관된 코드만 수정하면 됨.
- 테스트 코드를 작성하기 쉬워짐 : 얽혀있는 로직보다 분리되어 있는 로직에 대한 테스트가 보다 더 간단해짐

- Redux와 Create React App의 창시자로 React 생태계에서 아주 유명한 Dan Abramov는 2015년 본인의 블로그에 `Presentational - Container` 패턴으로 React 컴포넌트를 관리하는 방법을 제안했다. ([본문 링크](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0))
- **Presentational Component** : UI Only 컴포넌트 (`View`)
  - state, effect 등 로직 없음 → 화면을 렌더링하는 데 필요한 코드만 존재 (갖더라도 UI에 대한 상태만)
  - 함수 컴포넌트로 구현
- **Container Component** : Logic Only 컴포넌트 (`Logic`)

  • 하지만 Hooks의 등장으로 더이상 이 패턴을 권장하지 않는다.

  커스텀 훅스가 등장하고 관심사의 분리는 보통 한 컴포넌트에서 이뤄지기도 한다.

  **의존성 주입 Dependency Injection**

  소프트웨어 개발에 있어서 의존성이란 특정 객체가 어떤 요청 및 기능을 수행함에 있어서 외부 객체의 도움을 받을때 이 외부 객체를 특정 객체의 `의존성`

  nodejs 개발환경에서는 npm install을 통해 다양한 서드파티 패키지들을 설치해서 사용,,,

  우리의 어플리케이션 구현이 의존하는 서드파티 라이브러리들도 의존성

  의존한다는 이야기는 기능 구현에 있어서 필수적으로 사용해야 한다는 의미

  의존성을 선언하고 사용하는데 다양한 방법들이 있지만

  하나의 객체 코드가 복잡해지거나 가까운 미래에 다양한 기능이 추가 될 수 있을 때는 우리가 작성하는 코드가

  `확장 가능한지`,`재사용 가능한지`에 대해 고민해봐야 한다.

  의존성을 잘못 사용할 때는 두 객체 간의 관계가 강하게 결합(Coupling)되어 변경에 유연하지 않게 되기 때문에 이에 대한 해결 방안이 필요

  → 교체가능, 테스트 용이, 컴포넌트 통합시 어려움을 겪지 않는지..

  React에서의 의존성 주입방법

  ## props

  import 문을 통해서 바로 의존 객체를 참조할 수 있지만 props로 전달받아보겠다.

  ```
  import { DependencyProvider as DI } from "./provider";
  const App = ({ DI = DependencyProvider,  }) => {
    return (
      <DI>
        <Order {...props}/>
      </DI>)
  }
  ```

  App 컴포넌트에서는 default props 를 설정함으로 인해 DI를 명시적으로 주입할 경우 특정 용도로 사용할 수 있다.

  ```
  <App DI={MockProvider} />
  ```

  다음과 같이 static object/function으로도 주입이 가능하다.

  ## static object

  ```
  const App = () => {
    const { useMovies } = App.dependencies;
  }

  App.dependencies = {
    useMovies
  }
  ```

  ## static function

  ```
  const App = () => {
    const { useMovies } = App.dependencies();
  }

  App.dependencies = injectDependencies({
    useMovies
  })
  ```

  ## context api

  provider에서 상태를 제공할 수 있기에 이를 redux와 같은 스토어로 착각하는데에서 `context api는 상태 관리 툴이다` 라는 오해가 비롯된 것 같다.

  상태 관리 툴이 되려면 `상태 값을 업데이트 할 수 있어야 하는데 context api는 이러한 기능을 제공하지 않기 때문에 상태 관리 툴이 아니다.`

  context api는 상태관리 툴이 아닌 의존성 주입 도구로 보는 것이 더 타당하다.앞서 테스트 코드에서 살펴본 것 처럼 다음과 같이 의존성을 주입할 수 있다.

  ```
   <DependencyProvider serviceCreator={
       createOrderServiceMock
     }>
       <OrderMovies/>
     </DependencyProvider>
  ```

  ## 확장

  TDD(테스트 주도 개발)과 리펙터링으로 얻어지는 깨끗한 코드는 코드 수준에서 시스템을 조정하고 확장하기 쉽게 만든다.

  관심사를 적절히 분리해 관리한다면 소프트웨어 아키텍처는 발전할 수 있다.

  처음부터 올바르게 시스템을 만들 수 있다는 믿음은 미신이다. 대신에 우리는 오늘 주어진 사용자 스토리에 맞춰 시스템을 구현해야 한다. 내일은 새로운 스토리에 맞춰 시스템을 조정하고 확장하면 된다. 이것이 반복적이고 점짐적인 애자일 방식의 핵심이다. 테스트 주도 개발, 리펙터링, 깨끗한 코드는 코드 수준에서 시스템을 조정하고 확장하기 쉽게 만든다.

  • 대부분의 프록시 코드는 자동화가 가능. 여러 자바 프레임워크는 내부적으로 프록시 사용

  ### 의사 결정을 최적화하라

  큰 시스템에서는 한 사람이 모든 결정을 내릴 수는 없다.

  따라서 결정은 최대한 많은 정보가 모일 때까지 미루고 시기가 되었을 경우 해당 파트의 책임자(여기서는 모듈화된 컴포넌트)에게 맡기는 것이 불필요한 고객 피드백과 고통을 덜어줄 것이다.

  ### 명백한 가치가 있을 때 표준을 현명하게 사용하라

  표준을 사용하면 아이디어와 컴포넌트를 재사용하기 쉽고, 적절한 경험을 가지는 사람을 구하기 쉽고, 좋은 아이디어를 캡슐화하기 쉽고, 컴포넌트를 엮기 쉽지만 때로는 표준이 불필요함에도 사용하는 경우가 있다

  ⇒ 표준은 확실한 이득을 가져올 경우 사용하라

  ### 시스템은 도메인 특화 언어가 필요하다

  - **DSL :** 간단한 스크립트 언어나 표준 언어로 구현한 API.
  - 좋은 DSL → 도메인 개념과 그 개념을 구현한 코드 사이에 존재하는 의사소통 간극 ↓
  - DSL을 사용하면 모든 추상화 수준과 모든 도메인을 POJO로 표현 가능

  ### 결론

  - 깨끗하지 못한 아키텍처는 도메인 논리 흐리며 기민성을 떨어뜨린다. 기민성이 떨어지면 생산성이 낮아져 TDD가 제공하는 장점이 사라진다
  - 모든 추상화 단계에서 의도는 명확이 표현해야 한다. POJO를 작성하고 관점 혹은 관점과 유사한 매커니즘을 사용해 각 구현 관심사를 분리해야 한다.
  - 관점 지향 프로그래밍(Aspect-Oriented Programming, AOP)는 횡단 관심사에 대처에 모듈성을 확보하는 일반적인 방법론이다.
  - 자바에서는 자바 프록시, 순수 자바 AOP 프레임워크, AspectJ 언어를 통해 AOP를 지원한다.
  - 관점으로 관심사를 분리하면 테스트 주도 아키텍처 구축이 가능해지고 좋은 API를 만들 수 있다. (지엽적인 관리와 결정이 가능해진다.)
  - 너무 표준에 집착하기보다는 도메인 특화 언어를 사용하는 등 개발자가 적절한 추상화 수준에서 코드 의도를 표현할 수 있는 것이 좋다.
  - 시스템을 설계하든 모듈을 설계하든 실제로 돌아가는 가장 단순한 수단을 사용해야 한다.

  ***

  [https://velog.io/@kykim_dev/관심사의-분리Separation-of-Concerns-SoC와-Custom-Hook](https://velog.io/@kykim_dev/%EA%B4%80%EC%8B%AC%EC%82%AC%EC%9D%98-%EB%B6%84%EB%A6%ACSeparation-of-Concerns-SoC%EC%99%80-Custom-Hook)

  ***

  # 12장

  착실하게 따르기만 하면 우수한 설계가 나오는 간단한 규칙을 소개하는 챕터.

창발성을 이용해 SRP와 DIP 같은 원칙을 쉽게 적용하자.

**창발적 설계로 깔끔한 코드를 구현하자**

1. 모든 테스트를 실행한다. → 검증이 가능하고 변경이 쉬워짐
2. 중복을 없앤다.
3. 프로그래머 의도를 표현하라.
4. 클래스와 메서드 수를 최소로 줄여라.

## **모든 테스트를 실행하라**

설계는 의도한 대로 돌아가는 시스템을 내놓아야 한다.

테스트를 철저히 거쳐 모든 테스트 케이스를 항상 통과하는 시스템, 즉 테스트가 가능한 시스템을 만들려고 애쓰면 설계 품질이 높아진다.

모든 테스트를 실행하게 되면 크기가 작고 목적 하나만 수행하는 클래스가 나온다.

SRP를 준수하는 클래스는 테스트가 훨씬 더 쉽다.

테스트 케이스가 많을 수록 개발자는 테스트가 쉽게 코드를 작성한다.

결합도가 높으면 테스트 케이스를 작성하기 어렵다.

DIP와 같은 원칙을 적용하려 애써가며 DI, 인터페이스, 추상화등의 도구를 사용해 결합도를 낮추기 때문에 설계 품질이 높아진다.

**'테스트 케이스를 만들고 계속 돌려라' 라는 단순한 규칙을 따르면 낮은 결합도와 높은 응집력을 갖는 객체 지향 코드가 나온다.**

**리팩터링**

테스트 케이스를 모두 작성했다면 이제 코드와 클래스를 정리해도 괜찮다.

구체적으로 코드를 점진적으로 리팩터링 해나간다.

리팩터링 단계에서는 소프트웨어 설계 품질을 높이는 기법이라면 무엇이든 적용해도 괜찮다.

응집도를 높이고 결합도를 낮추고 관심사를 분리하고

시스템 관심사를 모듈로 나누고

함수와 클래스 크기를 줄이고

더 나은 이름을 선택하는 등 다양한 기법을 동원한다.

## **중복을 없애라**

중복은 추가 작업, 추가 위험, 불필요한 복잡도를 뜻한다.

똑같은 코드는 당연히 중복이고, 구현 중복도 중복의 한 형태이다.

```arduino
int size(){}
boolean isEmpty(){}
```

각 메서드를 따로 구현하는 방법도 있지만 서로 반환값이 다르다.

isEmpty메서드에서 size메서드를 이용하면 코드를 중복해 구현할 필요가 없어진다.

```arduino
boolean isEmpty(){
    return 0 == size();
}
```

깔끔한 시스템을 만드려면 단 몇 줄이라도 중복을 제거하겠다는 의지가 필요하다.

공통적인 코드를 새 메서드로 중복을 제거하고 보니 SRP를 위반하는 경우도 발생한다.

그러므로 다른 메서드를 생성해 SRP원칙을 지켜도 좋다.

다른 팀원이 이러한 새로운 메서드를 좀 더 추상화하여 다른 맥락에서 재사용 기회를 포착할지도 모른다.

이러한 `소규모 재사용`은 시스템 복잡도를 극적으로 줄이는 효과를 가져온다.

소규모 재사용을 제대로 익혀야 `대규모 재사용`이 가능하다.

## 프로그래머 의도를 표현하라.

대다수의 엉망인 코드를 접한 경험이 있을 것이다.

스스로 엉망인 코드를 내놓은 경험도 있을 것이다.

자신이 이해하는 코드를 짜기는 쉽지만 나중에 코드를 유지보수할 사람이 코드를 짜는 사람만큼이나 문제를 깊이 이해할 가능성은 희박하다.

소프트웨어 프로젝트 비용 중 대다수는 장기적인 유지보수에 들어간다.

코드를 변경하면서 버그의 싹을 심지 않으려면 유지보수 개발자가 시스템을 제대로 이해해야 한다.

하지만 시스템이 복잡해지면서 유지보수 개발자가 시스템을 제대로 이해하느라 들어가는 비용이 늘어날 수록

코드를 오해할 가능성도 커진다.

그러므로 `코드는 개발자의 의도를 분명하게 표현`해야 한다.

개발자가 코드를 명백하게 짤수록 다른사람이 코드를 이해하기 쉬워지며 결합이 줄어들고 유지보수 비용이 적게든다.

코드를 명백하게 작성하는 몇가지 팁.

**좋은 이름을 선택한다.**

```erlang
  이름과 기능이 완전히 관련없는 클래스나 함수로 유지보수 담당자를 놀라게 해서는 안 된다.
```

**함수와 클래스 크기를 가능한 줄인다.**

```erlang
  작은 클래스와 작은 함수는 이름 짓기도 쉽고, 구현하기도 쉽고, 이해하기도 쉽다.
```

**표준 명칭을 사용한다.**

```erlang
  클래스가 command와 visitor 같은 표준 패턴을 사용해 구현한다면 클래스 이름에 패턴 이름을 넣어준다.

  다른 개발자가 클래스 설계 의도를 이해하기 쉬워진다.

  디자인 패턴은 의사소통과 표현력 강화가 주요 목적이다.
```

**단위 테스트 케이스를 꼼꼼히 작성한다.**

```prolog
  테스트 케이스는 소위 예제로 보여주는 문서이다.

  ( 잘 만든 테스트 케이스를 읽으면 클래스 기능이 한눈에 들어온다. )

  하지만 표현력을 높이는 가장 중요한 방법은 노력이다.

  코드만 돌아가게 만든 후 다음 문제로 직행하는 사례가 너무 흔하다.

  나중에 읽을 사람을 고려해 조금이라도 읽기 쉽게 만들려는 충분한 고민은 거의 찾기 어렵다.

  나중에 코드를 읽을 사람은 바로 자신일 가능성이 높다는 사실을 명심하자.

  그러므로 `자신의 작품을 조금 더 자랑하자`.

  함수와 클래스에 조금 더 시간을 투자하자.

  더 나은 이름을 선택하고 큰 함수를 작은 함수 여럿으로 나누고 자신의 작품에 조금만 더 주의를 기울이자.

  주의는 대단한 재능이다.
```

## **클래스와 메서드 수를 최소로 줄여라**

중복을 제거하고 의도를 표현하고 SRP를 준수한다는 기본적인 개념도 극단으로 치닫게 되면 득보다는 실이 많아진다.

클래스와 메서드 크기를 줄이자고 조그만 클래스와 메서드를 수없이 만드는 사례도 존재한다.

그래서 이 규칙은 함수와 클래스 수를 가능한 줄이라고 제안한다.

때로는 무의미하고 독단적인 정책 탓에 클래스와 메서드 수가 늘어나기도 한다.

클래스마다 무조건 인터페이스를 생성하라고 요구하는 구현 표준이 좋은 방법이다.

자료 클래스와 동작 클래스는 무조건 분리해야 한다는 주장하는 개발자도 좋은 개발자이다.

`가능한 독단적인 견해는 멀리하고 실용적인 방식을 선택해야 한다`.

목표는 함수와 클래스 크기를 작게 유지하면서 동시에 시스템 크기도 작게 유지하는 데 있다.

하지만 이 규칙은 간단한 설계 규칙 중 우선 순위가 가장 낮다.

다시 말해 클래스와 함수 개수를 줄이는 작업도 중요하지만

테스트 케이스르 만들고 중복을 제거하고

의도를 분명히 표현하는 작업이 더 중요하다는 뜻이다.

### 결론

---

앞선 챕터에서는 함수와 클래스를 최대한 쪼개어라라고 설명했지만

이번 챕터에서는 함수와 클래스 수를 가능한 줄이라고 설명하였다.

저자가 하고싶은 말은

하나의 책임을 가지며 추상화에 의존하며 최소한의 개수와 최대한 쪼개는 코드 작성 방법이

이상적인 방법이라고 제시하는 것 같다.

---

# 13장

### **동시성이란?**

멀티 스레드를 사용하는 프로그램을 말한다.

### **동시성이 필요한 이유?**

동시성은 결합을 없애는 작업이다.

무엇과 언제를 분리하는 전략을 말한다.

단일 스레드는 무엇을 언제 실행하는지 예상할 수 있다.

디버깅하기엔 좋지만 작업 효율이 좋지 않은 경우가 있다.

그럴 때 멀티 스레드를 사용한다. 무엇과 언제를 분리시켜 효율을 증대시킨다.

### **1. 동시성은 항상 성능을 높여준다.**

아니다. **때로** 성능을 높여준다.

- 대기 시간이 아주 길어 여러 스레드가 프로세스를 공유하는 경우
- 여러 프로세스가 동시에 처리할 독립적인 계산이 충분히 많은 경우

추가로

- 동시성을 사용할 환경인지? 받아주는 쪽에서 동시성을 받을 여력이 되는지도 봐야한다.

### **2. 동시성을 구현해도 설계는 변하지 않는다.**

단일 스레드와 멀티 스레드 시스템은 설계가 아예 다르다.

무엇과 언제를 분리하면 시스템 구조가 달라진다.

### **3. 스프링 컨테이너를 사용하면 동시성을 이해할 필요가 없다.**

언제나 그렇듯 이해를 해야 한다. 몰라도 개발은 할 수 있지만 모르면 모르고 지나가기 때문에 아는 게 좋다.

오해 중에 사실도 있다.

### **1. 멀티 스레드 시스템은 부하가 많을 것이다.**

맞다. 한번에 많은 요청, 많은 작업을 하려니 당연히 부하가 많다.

### **2. 동시성은 복잡하다.**

복잡하다. 고려해야 할 사항이 많다.

### **3. 일반적으로 동시성 버그는 재현하기 어렵다.**

단순 로직 문제면 확인하기 쉽지만.

그게 아닌 경우가 가끔 있어서 재현하기 어려운 경우가 있다.

### **4. 단일 -> 멀티 스레드로 설계를 바꾸는 경우 근본적인 설계 전략을 고민해야 한다.**

이건 좀 케바케인 거 같다.

내가 개발해보면 엄청나게 근본적인 설계까지 바꿔야 하는 경우가 있지 않았다.

일단 책에선 그렇다니 가끔 그런 경우가 있나보다.

**결론 : 여튼 하나의 소스에선 여러 경로가 있는데 예상하지 못한 일부 경로에서 생각지도 못한 문제가 발생할 수 있기 때문에 동시성 구현하기가 어렵다.**

### **동시성 방어 원칙**

그렇다면 위와 같이 우리가 생각하지도 못한 경로에서 에러나는 걸 어떻게 방지할 수 있을까?에 대해 알아보자.

### **1. SRP(Single Responsibility Principle)를 지키자**

SRP = 클래스, 메소드를 변경할 이유는 1가지여야 한다. 책임이 1개여야 한다.

- 동시성도 하나의 책임이다.
- 동시성 코드는 복잡하고 어렵다.
- 독자적인 개발, 변경, 조율 주기가 있다.

**다른 코드와 분리하자.**

### **2. 공유 자료를 최대한 줄이자.**

Sequnce의 getNextSeq에서 문제가 됐던 게 바로 seq를 공유 자료로 사용했기 때문인다.

getNextSeq 메소드를 임계영역으로 정하고 synchronized 를 붙혀주면 해결할 수 있다.

```java
public class Sequence {    private int seq = 7;    // synchronized 추가    public synchronized int getNextSeq() {        return ++seq;    }}
```

물론 공유 자료를 임계 영역에 잘 넣어둔다면 괜찮겠지만 실수가 발생할 수 있는 코드기 때문에 항상 주의를 기울여야 한다.

**안 쓸 수 있으면 안 쓰는 방향으로 가자.**

### **3. 자료 사본을 사용하라**

임계 영역을 사용하지 않을 거면 사본을 사용하는 것도 한 가지 방법이다.

공유 자료를 복사해서 대체할 수 있는 경우 사용하면 된다.

복사 vs 임계 영역에 대한 부하가 걱정이라면 테스트를 해보자.

**임계 영역 대신 객체를 복사해서 사용하자.**

### **4. 스레드는 가능한 독립적으로 구현하라**

2, 3에서 말한 거와 같은 맥락인 거 같다.

**각 스레드가 독립적으로 도는 것처럼 공유 객체를 사용하지 말자는 내용이다.**

### **5. 라이브러리를 이해하라**

개발하다보면 동시성이 필요한 경우 java.util.concurrent에 있는 클래스를 사용하라 라는 말을 자주 듣는다.

**자바에선 이미 동시성을 위한 클래스들이 있다. 그걸 사용하자.**

### **6. 실행 모델을 이해하라**

위키에 정리가 잘 되어있어 링크로 대체한다.

[생산자-소비자](https://ko.wikipedia.org/wiki/%EC%83%9D%EC%82%B0%EC%9E%90-%EC%86%8C%EB%B9%84%EC%9E%90_%EB%AC%B8%EC%A0%9C)

[독자-저자](https://ko.wikipedia.org/wiki/%EB%8F%85%EC%9E%90-%EC%A0%80%EC%9E%90_%EB%AC%B8%EC%A0%9C)

[식사하는 철학자들](https://ko.wikipedia.org/wiki/%EC%8B%9D%EC%82%AC%ED%95%98%EB%8A%94_%EC%B2%A0%ED%95%99%EC%9E%90%EB%93%A4_%EB%AC%B8%EC%A0%9C)

[잠자는 이발사](https://ko.wikipedia.org/wiki/%EC%9E%A0%EC%9E%90%EB%8A%94_%EC%9D%B4%EB%B0%9C%EC%82%AC_%EB%AC%B8%EC%A0%9C)

**위 실행 모델을 이해하자**

### **7. 동기화하는 메서드 사이에 존재하는 의존성을 이해하라**

synchronized가 붙은 메서드 사이에 의존성을 두지말자.

정말 필요하다면

클라이언트에서 잠금 - 클라이언트에서 첫 번째 메서드를 호출 전에 서버를 잠근다. 마지막 메서드를 호출할 때까지.

서버에서 잠금 - 서버에다 "서버를 자금고 모든 메서드를 호출 후 잠금을 해제하는" 메서드를 구현. 클라이언트에서 이 메서드 호출

연결 서버 - 중간 서버를 둬서 중간 서버에서 잠금을 수행

을 사용하자. (클라이언트에서 잠근이나 서버에서 잠금이나 똑같은 말 아닌가 싶다... 이해가 안 간다.)

### **8. 동기화 부분을 작게 만들자**

synchronized를 사용하는 메서드를 잘게 쪼개자.

너무 큰 단위로 하면 성능에 문제가 생긴다.

### **9. 올바른 종료 코드는 구현이 어렵다**

깔끔하게 종료하는 코드를 올바르게 구현하긴 어렵다.

예를들면 데드락이 있다.

이런 문제를 풀기 위한 알고리즘은 기존에 나와있는 방식을 사용하자. 어렵다.

### **10 . 스레드 코드 테스트를 잘 하자**

멀티 스레드 환경에선 이해가 안 가는, 재현이 정말 어려운 에러들이 많이 발생한다. 이를 초기에 잡으려면 테스트를 잘 해야한다.

하나씩 알아보자.

- **말이 안 되는 실패는 잠정적인 스레드 문제로 취급하라.**

멀티 스레드 환경에선 가끔씩 알 수 없는 에러가 발생한다.

수천, 수백만번에 1번씩 나는 에러라도 일회성 문제로 치부하지말고 원인과 해결책을 찾아보자.

- **멀티 스레드로 개발하기 전에 단일 스레드 환경에서 돌아가게 먼저 개발하자.**

멀티 스레드가 필요한 이유는 어차피 효율 때문인다.

근데 멀티 스레드로 개발하는 건 위에서도 말했지만 어렵다.

그러기에 단일 스레드로 먼저 잘 돌아가는 코드를 개발 후 멀티 스레드로 전환하자.

- **다양한 환경에서 돌려보자.**
- 스레드 수를 1개, 2개, 3개, 그 이상으로 테스트 해보자.
- 실환경, 테스트환경에서 돌려본다.
- 테스트 코드를 빨리, 천천히, 다양한 속도로 돌려보자.
- **스레드 수를 쉽게 조절할 수 있게 코드를 작성하자.**

처음부터 적절한 스레드 수를 알 수 없다.

환경변수로 스레드 수를 운영 중에도 변경할 수 있게 하는 방법을 고민해보자.

- **프로세서 수보다 많은 스레드를 돌려보자.**

프로세서 수보다 많은 스레드를 돌려 스와핑을 유도해서 임계 영역을 빼먹은 코드나 데드락을 일으키는 코드를 찾아보자.

- **다른 플랫폼에서 돌려보자.**

운영체제마다 다른 스레드 정책이 걱정된다면 다른 플랫폼에 돌려보자.

- **코드에 보조 코드를 넣어 강제로 실패를 일으키게 해보자.**

wait, yield, priority 등과 같은 스레드를 조작할 수 있는 메서드들을 중간중간에 넣어 돌려보자.

예상밖에 문제가 보일지도 모른다.

### **정리**

멀티 스레드로 구현하긴 어렵다.

빨라야 할 거 같은데 빠르지 않은 경우가 엄청 많다.

위에 적어놓은 문제들과 미신과 오해를 잘 보고 이해하자.

동시성 관련해서 이렇게 짧게 정리한다는 게 맞나싶다.

부록에 동시성 부록이 있다. 한번 읽어보자 자세하게 나와있다.

나중에 기회가 되면 정리해보겠다.

---

[https://deview.kr/data/deview/session/attach/1_Inside React (동시성을 구현하는 기술).pdf](<https://deview.kr/data/deview/session/attach/1_Inside%20React%20(%E1%84%83%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF).pdf>)

[https://velog.io/@ggombee/JS-Event-Loop](https://velog.io/@ggombee/JS-Event-Loop)

[https://programming119.tistory.com/242](https://programming119.tistory.com/242)

[https://velog.io/@perfumellim/쉽게-떠먹여주는-React-18-업데이트](https://velog.io/@perfumellim/%EC%89%BD%EA%B2%8C-%EB%96%A0%EB%A8%B9%EC%97%AC%EC%A3%BC%EB%8A%94-React-18-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8)
