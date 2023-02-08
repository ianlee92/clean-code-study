# 7장

### 오류코드보다 예외를 사용하라

알고리즘의 분리가 중요

→ exception을 사용하여 실제로직과 예외처리 부분이 나뉘어져 필요부분에 집중할 수 있는 코드를 만들자.

### try-catch-finally문부터 작성하라

예외가 발생할 코드는 이렇게 시작하자.

→ catch 블록에서 예외 유형을 좁히는 방법으로 리팩터링이 가능해진다.

→ 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테스트를 통과하게 코드를 작성

(트랜잭션의 본질)

### 미확인 예외를 사용하라

예외처리에 드는 비용 대비 이득을 생각하자.

상위레벨 메소드에서 하위레벨 메소드의 디테일에 대해 알아야 하기 때문에 캡슐화도 깨지기 마련 비용도 높아지고,,

→ 경우에 따라 checked exceptions를 사용해야 되지만 득<실

### 예외의 의미를 제공하라

예외가 발생한 원인과 충분한 정보를 제공

### 호출자를 고려해 예외 클래스를 정의하라

오류를 잡아내는 방법을 중요하게 여겨야한다.

써드파티 라이브러리를 사용하는 경우 그것들을 wrapping함으로써

1. 라이브러리 교체 등의 변경이 있는 경우 대응하기 쉬워진다.
2. 라이브러리를 쓰는 곳을 테스트할 경우 해당 라이브러리를 가짜로 만들거나 함으로써 테스트하기 쉬워진다.
3. 라이브러리의 api 디자인에 종속적이지 않고 내 입맛에 맞는 디자인을 적용할 수 있다.

보통 특정 부분의 코드에는 exception 하나로 충분히 예외처리 할 수 있다.

한 exception만 잡고 나머지 하나는 다시 throw하는 경우 등 정말 필요한 경우에만 다른 exception 클래스를 만들어 사용하자.

### 정상 흐름을 정의하라

예외사항을 처리해야 하는경우 코드가 더러워지는것을 방지하자

### Null을 리턴하지 마라

지속적으로 널체크를 하게 된다.. 이는 또다른 비용을 초래할 수 있음.

→ 써드파티 라이브러리 null 리턴 가능성이 존재한다면 예외처리를 하거나 Special Case Object를 리턴하는 메서드로 래핑할 것.

---

### 결론

예외처리를 로직에서 제거하고 모든 메서드에 대한 독립적인 사고를 하도록 노력하자.

### 참조

### 1. Checked exception VS Unchecked Exception

Checked Exception과 Unchecked Exception의 가장 명확한 구분 기준은 ‘꼭 처리를 해야 하느냐’이다. Checked Exception이 발생할 가능성이 있는 메소드라면 반드시 로직을 try/catch로 감싸거나 throw로 던져서 처리해야 한다. 반면에 Unchecked Exception은 명시적인 예외처리를 하지 않아도 된다. 이 예외는 피할 수 있지만 개발자가 부주의해서 발생하는 경우가 대부분이고, 미리 예측하지 못했던 상황에서 발생하는 예외가 아니기 때문에 굳이 로직으로 처리를 할 필요가 없도록 만들어져 있다.출처: [http://www.nextree.co.kr/p3239/](http://www.nextree.co.kr/p3239/)

### 2. Open/Closed Principle

The Open Close Principle states that the design and writing of the code should be done in a way that new functionality should be added with minimum changes in the existing code. The design should be done in a way to allow the adding of new functionality as new classes, keeping as much as possible existing code unchanged.출처: [http://www.oodesign.com/open-close-principle.html](http://www.oodesign.com/open-close-principle.html)

### 3. Special Case Pattern(by Martin Fowler)

- [http://www.captaindebug.com/2011/04/null-return-values-and-special-case.html#.VM9VUsbLgXR](http://www.captaindebug.com/2011/04/null-return-values-and-special-case.html#.VM9VUsbLgXR)
- [http://martinfowler.com/eaaCatalog/specialCase.html](http://martinfowler.com/eaaCatalog/specialCase.html)

---

# 8장

우리는 새로운 패키지나 오픈소스를 사용해야 할 상황에 직멶한다. 이 코드들을 우리 내부 코드와 “클린하게” 통합시켜야 한다.

### 외부코드 사용하기

인터페이스를 제공 및 사용하는 입장사이에는 필연적인 긴장감이 존재하기 마련. 이것을 경계에서의 긴장이라 한다.

```jsx
public class Sensors {
    // 경계의 인터페이스(이 경우에는 Map의 메서드)는 숨겨진다.
    // Map의 인터페이스가 변경되더라도 여파를 최소화할 수 있다. 예를 들어 Generic을 사용하던 직접 캐스팅하던 그건 구현 디테일이며 Sensor클래스를 사용하는 측에서는 신경쓸 필요가 없다.
    // 이는 또한 사용자의 목적에 딱 맞게 디자인되어 있으므로 이해하기 쉽고 잘못 사용하기 어렵게 된다.

    private Map sensors = new HashMap();

    public Sensor getById(String id) {
        return (Sensor)sensors.get(id);
    }
    //snip
}
```

### 경계 살피기

외부코드를 사용하면 적은시간에 더 좋은 효율을 낼 수 있다.

하지만, 안주하지않고 테스트를 꼭 진행해야 한다.

(수많은 디버깅…오류..발생……에딭….위지윅…..\_)

### log4j 익히기

```jsx
// 1.
    // 우선 log4j 라이브러리를 다운받자.
    // 고민 많이 하지 말고 본능에 따라 "hello"가 출력되길 바라면서 아래의 테스트 코드를 작성해보자.
    @Test
    public void testLogCreate() {
        Logger logger = Logger.getLogger("MyLogger");
        logger.info("hello");
    }

    // 2.
    // 위 테스트는 "Appender라는게 필요하다"는 에러를 뱉는다.
    // 조금 더 읽어보니 ConsoleAppender라는게 있는걸 알아냈다.
    // 그래서 ConsoleAppender라는 객체를 만들어 넣어줘봤다.
    @Test
    public void testLogAddAppender() {
        Logger logger = Logger.getLogger("MyLogger");
        ConsoleAppender appender = new ConsoleAppender();
        logger.addAppender(appender);
        logger.info("hello");
    }

    // 3.
    // 위와 같이 하면 "Appender에 출력 스트림이 없다"고 한다.
    // 이상하다. 가지고 있는게 이성적일것 같은데...
    // 구글의 도움을 빌려, 다음과 같이 해보았다.
    @Test
    public void testLogAddAppender() {
        Logger logger = Logger.getLogger("MyLogger");
        logger.removeAllAppenders();
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n"),
            ConsoleAppender.SYSTEM_OUT));
        logger.info("hello");
    }

    // 성공했다. 하지만 ConsoleAppender를 만들어놓고 ConsoleAppender.SYSTEM_OUT을 받는건 이상하다.
    // 그래서 빼봤더니 잘 돌아간다.
    // 하지만 PatternLayout을 제거하니 돌아가지 않는다.
    // 그래서 문서를 살펴봤더니 "ConsoleAppender의 기본 생성자는 unconfigured상태"란다.
    // 명백하지도 않고 실용적이지도 않다... 버그이거나, 적어도 "일관적이지 않다"고 느껴진다.
```

```jsx
// 조금 더 구글링, 문서 읽기, 테스트를 거쳐 log4j의 동작법을 알아냈고 그것을 간단한 유닛테스트로 기록했다.
// 이제 이 지식을 기반으로 log4j를 래핑하는 클래스를 만들수 있다.
// 나머지 코드에서는 log4j의 동작원리에 대해 알 필요가 없게 됐다.

public class LogTest {
    private Logger logger;

    @Before
    public void initialize() {
        logger = Logger.getLogger("logger");
        logger.removeAllAppenders();
        Logger.getRootLogger().removeAllAppenders();
    }

    @Test
    public void basicLogger() {
        BasicConfigurator.configure();
        logger.info("basicLogger");
    }

    @Test
    public void addAppenderWithStream() {
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n"),
            ConsoleAppender.SYSTEM_OUT));
        logger.info("addAppenderWithStream");
    }

    @Test
    public void addAppenderWithoutStream() {
        logger.addAppender(new ConsoleAppender(
            new PatternLayout("%p %t %m%n")));
        logger.info("addAppenderWithoutStream");
    }
}
```

### 학습테스트는 공짜 이상이다

투자하는 노력보다 얻는 성과가 거 크다.

### 아직 존재하지 않는 코드를 사용하기

아직 개발되지않은 모듈이 필요한데, 구현되지 않았을경우 우리의 일정이 밀리는 것을 가만히 두고 보지 않는다.

그렇기에 미리미리 생각해서 설계하고 더 빠르게 개발할 수 있도록 설계하자. (더미로직)

### 깨끗한 경계

좋은 소프트웨어 디자인은 변경이 생길 경우 많은 재작업이 필요하지 않다.

우리의 내부코드가 외부코드를 많이 관여하지 않도록 막아야 한다.

우리가 컨트롤 할수 있는 코드에 의지하자. 그렇지 않으면 외부코드들이 우리를 조종할 것이다.

---

# 9장

### TDD 법칙 세 가지

- 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
- 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
- 현재 실패하는 테스트를 통과하는 정도로만 실제 코드를 작성한다.

이 규칙에 따르면 개발과 테스트가 대략 30초 주기로 묶임, 실제 코드를 사실상 전부 테스트하는 테스트 케이스가 나온다.

하지만 이런 테스트 코드는 관리 문제를 일으킨다.

### 깨끗한 테스트 코드 유지하기

몇 년 전 저자는 **테스트 코드에 실제 코드와 동일한 품질 기준을 적용하지 않아야 한다고 명시적으로 결정한 팀**을 코치해달라는 요청을 받음. '**지저분해도 빨리**'

변수 이름은 신경 쓸 필요가 없고, 테스트 함수는 간결하거나 서술적일 필요가 없었고, 테스트 코드는 잘 설계하거나 주의해서 분리할 필요가 없었다. 그저 돌아만 가면, 그러니까 실제 코드를 테스트만 하면 그만이었다.

하지만 팀은 지저분한 테스트 코드를 내놓으나 테스트를 안 하나 오십보 백보라는, 아니 오히려 더 못하다는 사실을 깨닫지 못했다. 문제는 실제 코드가 진화하면 테스트 코드도 변해야 한다는 데 있다.

그런데 테스트 코드가 지저분하면 할수록 변경하기 어려워진다.

이 경우실제 코드를 짜는 시간보다 테스트 케이스를 추가하는 시간이 더 걸리기 쉽상이다.

실제 코드를 변경해 기존 테스트 케이스가 실패하기 시작하면, 지저분한 코드로 인해 실패하는 테스트 케이스를 점점 더 통과시키기 어려워진다. 그래서 테스트 코드는 계속해서 늘어나는 부담이 되버린다.

새 버전을 출시할 때마다 팀이 테스트 케이스를 유지하고 보수하는 비용도 늘어난다.

점차….. 테스트 코드는 개발자 사이에서 가장 큰 불만으로 자리잡게 되고, 관리자가 예측값이 너무 큰 이유를 물어보면 팀은 테스트 코드를 비난한다. 결국 테스트 슈트를 폐기하지 않으면 안 되는 상황에 처한다.

하지만 테스트 슈트가 없으면 개발자는 자신이 수정한 코드가 제대로 도는지 확인할 방법이 없다.

시스템 이쪽을 수정해도 저쪽이 안전하다는 사실을 검증하지 못하게 되고, 그래서 결함율이 높아지기 시작한다.

의도하지 않은 결함 수가 많아지면 개발자는 변경을 주저한다.

변경하면 득보다 해가 크다 생각해 더 이상 코드를 정리하지 않는다. 그러면서 코드가 망가지기 시작한다.

결국 테스트 슈트도 없고, 얼기설기 뒤섞인 코드에, 좌절한 고객과, 테스트에 쏟아 부은 노력이 허사였다는 실망감만 남는다.

어떤 면에서 그들이 옳았다. **테스트에 쏟아 부은 노력은 확실히 허사였다.** 하지만 **실패를 초래한 원인은 테스트 코드를 막 짜도 좋다고 허용한 결정이었다.** 테스트 코드를 깨끗하게 짰다면 테스트에 쏟아 부은 노력은 허사로 돌아가지 않았을 터이다. 내가 이처럼 어느 정도 자신 있게 말하는 이유는 내가 참여하고 조언한 많은 팀이 깨끗한 단위 테스트 코드로 성공했기 때문이다.

**테스트 코드는 실제 코드 못지 않게 중요하다.** 사고와 설계와 주의가 필요하며 실제 코드 못지 않게 깨끗하게 짜야 한다.

### 테스트는 유연성, 유지보수성, 재사용성을 제공한다

테스트 코드를 깨끗하게 유지하지 않으면 결국은 잃어버린다. 그리고 테스트 케이스가 없으면 실제 코드를 유연하게 만드는 버팀목도 사라진다.

**코드에 유연성, 유지보수성, 재사용성을 제공하는 버팀목이 바로 단위 테스트**다.

테스트 케이스가 있으면 변경이 두렵지 않으니까! 테스트 케이스가 없다면 모든 변경이 잠정적인 버그다. 아키텍쳐가 아무리 유연하더라도, 설계를 아무리 잘 나눴더라도, 테스트 케이스가 없으면 개발자는 변경을 주저한다. 버그가 숨어들까 두렵기 때문이다.

하지만 테스트 케이스가 있다면 공포는 사실상 사라진다. 테스트 커버리지가 높을수록 공포는 줄어든다. 아키텍처가 부실한 코드나 설계가 모호하고 엉망인 코드라도 별다른 우려 없이 변경할 수 있다. 아니, 오히려 안심하고 아키텍처와 설계를 개선할 수 있다.

그러므로 실제 코드를 점검하는 자동화된 단위 테스트 슈트는 설계와 아키텍쳐를 최대한 깨끗하게 보존하는 열쇠다. 테스트는 유연성, 유지보수성, 재사용성을 제공한다. **테스트 케이스가 있으면 변경이 쉬워지기 때문이다.**

따라서 테스트 코드가 지저분하면 코드를 변경하는 능력이 떨어지며 코드 구조를 개선하는 능력도 떨어진다. 테스트 코드가 지저분할수록 실제 코드도 지저분해진다. 결국 테스트 코드를 잃어버리고 실제 코드도 망가진다.

## 깨끗한 테스트 코드

`가독성`이 중요하다..

**BUILD-OPERATE-CHECK 패턴**

→ 잡다하고 세세한 코드를 거의 다 없앴다. 테스트 코드는 본론에 진짜 필요한 자료 유형과 함수만 사용한다. 그러므로 코드를 읽는 사람은 잡다하고 세세한 코드에 헷갈릴 필요 없이 코드가 수행하는 기능을 재빨리 이해한다.

**도메인에 특화된 테스트 언어**

→ 도메인에 특화된 언어DSL로 테스트 코드를 구현하는 기법을 보여준다. 흔히 쓰는 시스템 조작 API를 사용하는 대신 API 위에 함수와 유틸리티를 구현한 후 그 함수와 유틸리티를 사용하므로 테스트 코드를 짜기도 읽기도 쉬워진다. 이렇게 구현한 유틸리티는 테스트 코드에서 사용하는 특수 API가 된다. 즉, 테스트를 구현하는 당사자와 나중에 테스트를 읽어볼 독자를 도와주는 테스트 언어이다.

**이중 표준**

**테스트 API코드에 적용하는 표준은 실제 코드에 적용하는 표준과 확실히 다르다.**
 단순하고, 간결하고, 표현력이 풍부해야 하지만, 실제 코드만큼 효율적일 필요는 없다. 실제 환경이 아니라 테스트 환경에서 돌아가는 코드이기 때문인데, 실제 환경과 테스트 환경은 요구사항이 판이하게 다르다.

실제 환경에서는 절대로 안 되지만 테스트 환경에서는 전혀 문제없는 방식이 있다. 대개 메모리나 CPU 효율과 관련 있는 경우다. **코드의 깨끗함과는 철저히 무관하다.**

### 테스트 당 assert 하나

TEMPLATE METHOD 패턴을 사용하면 중복을 제거할 수 있다

**테스트당 개념 하나**

독자가 각 절이 거기에 존재하는 이유와 각 절이 테스트하는 개념을 모두 이해해야 한다.

가장 좋은 규칙은 "개념 당 assert 문 수를 최소로 줄여라"와 "테스트 함수 하나는 개념 하나만 테스트하라"라 하겠다.

### F.I.R.S.T

Fast : 빨라야 한다.

Independent : 서로 의존하면 안된다. 결함이 숨겨진다.

Repeatable : 반복 가능해야 한다. 어느 상황이든 반복해서 실행 가능해야 한다.

Self-Validating : boolean 값으로 결과를 내어 로그 파일을 읽지 않도록 만들어야 한다.

Timely: 적시에 작성해야 한다. 실제 코드를 구현하기 전에 작성하는 것이 좋다.

---

### 결론

이 장은 주제를 수박 겉핥기 정도로만 훑었다. 사실상 **깨끗한 테스트 코드**라는 주제는 책 한 권을 할애해도 모자랄 주제다. 테스트 코드는 실제 코드만큼이나 프로젝트 건강에 중요하다. 어쩌면 실제 코드보다 더 중요할지도 모르겠다. 테스트 코드는 실제 코드의 유연성, 유지보수성, 재사용성을 보존하고 강화하기 때문이다. 그러므로 테스트 코드는 지속적으로 깨끗하게 관리하자. 표현력을 높이고 간결하게 정리하자. 테스트 API를 구현해 도메인 특화 언어(Domain Specific Language, DSL)를 만들자. 그러면 그만큼 테스트 코드를 짜기가 쉬워진다.

테스트 코드가 방치되어 망가지면 실제 코드도 망가진다. 테스트 코드를 깨끗하게 유지하자.
