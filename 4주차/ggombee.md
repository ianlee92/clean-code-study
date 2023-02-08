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
