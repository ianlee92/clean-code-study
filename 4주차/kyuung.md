# 7장, 오류처리

> _오류 처리는 중요하다.
> 하지만 오류 처리 코드로 인해 프로그램 논리를 이해하기 어려워진다면 깨끗한 코드라 부르기 어렵다.
> 오류 처리를 프로그램 논리와 분리하면 독립적인 추론이 가능해지면 코드 유지보수성도 크게 높아진다._

## 오류 코드보다 예외를 사용하라

- 예외를 지원하지 않는 언어는 오류를 처리하고 보고하는 방법이 제한적이다.
  - 오류 플래그를 설정하거나 호출자에게 오류 코드를 반환하는 방법이 전부다.

```java
// 복잡한 코드
public class DeviceController {
    public void sendShutDown() {
        DeviceHandle handle = getHandle(DEV1);
        // 디바이스 상태를 점검한다.
        if (handle != DeviceHandle.INVALID) {
            // 레코드 필드에 디바이스 상태를 저장한다.
            retrieveDeviceRecord(handle);
            //디바이스가 일시정지 상태가 아니라면 종료한다.
            if (record.getStatus() != DEVICE_SUSPENDED) {
                pauseDevice(handle);
                clearDeviceWorkQueue(handle);
                closeDevice(handel);
            } else {
                logger.log("Device suspended. Unable to shut down");
            }
        } else {
            logger.log("Invalid handle for: " + DEV1.toString());
        }
    }
}
```

- 위와 같은 방법을 사용하면 호출자 코드가 복잡해진다.
  - 함수를 호출한 즉시 오류를 확인해야 하기 때문이다. ( 수많은 조건들! )

### try catch로 개선해보자

```java
// 오류를 발견하면 예외를 던지는 코드
public class DeviceController {
    public void sendShutDown() {
        try {
            tryToShutDown();
        } catch (DeviceShutDownError e) {
            logger.log(e);
        }
    }

    private void tryToShutDown() throws DeviceShutDownError {
        DeviceHandle handle = getHandle(DEV1);
        DeviceRecord record = retrieveDeviceRecord(handle);

        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
    }

    private DeviceHandle getHandle(DeviceId id) {
        ...
        throw new DeviceShutDownError("Invalid handle for:" + id.toString());
        ...
    }
}
```

- 이제 논리가 오류 처리 코드와 뒤섞이지 않는다. 이제는 각 개념을 독립적으로 살펴보고 이해할 수 있다.

## Try-Catch-Finally 문부터 작성하라

<aside>
💡 먼저 강제로 예외를 일으키는 테스트 케이스를 작성한 후 테슽를 통과하게 코드를 작성하는 방법을 권장한다. 
→ 그러면 자연스럽게 try 블록에서 실행할 로직부터 구현하게 되므로 범위 내에서 논리를 유지하기 쉬워진다.

</aside>

- 예외에서 프로그램 안에다 **범위를 정의**한다는 사실은 매우 흥미롭다.
  - try, catch
- `try-catch-finally` 문에서 try 블록에 들어가는 코드를 실행하면, 원하는 시점에 catch 블록으로 넘어갈 수 있다.
- 예외가 발생할 코드를 짤 때는 `try-catch-finally`문으로 시작하는 편이 좋다.
  - 함수를 실행할 때 기대하는 상태를 정의하기 쉬워지기 때문이다.

```java
// 파일이 없으면 **예외를 던지는지 알아보는 단위테스트 코드**
@Test(expected = StorageException.class)
public void retrieveSectionShouldThrowOnInvalidFileName() {
    sectionStore.retrieveSection("invalid - file");
}
```

```java
// (실패)
public List<RecordedGrip> retrieveSection(String sectionName) {
    // 실제로 구현할 때까지 비어 있는 더미를 반환
    return new ArrayList<RecordedGrip>();
}
```

- 하지만 위 코드가 예외를 던지지 않으므로 단위 테스트는 실패했다.

```java
// (성공)
public List<RecordedGrip> retrieveSection(String sectionName) {
    try {
        FileInputStream stream = new FileInputStream(sectionName); // 1)
        stream.close();
    } catch (FileNotFoundException e) {
        throw new StorageException("retrieval error", e);
    }
    return new ArrayList<RecordedGrip>();
}
```

- 코드가 예외를 던지므로, 이제는 테스트가 성공한다.
- catch 블록에서 예외 유형을 주어, 파일이 없으면 FileNotFoundException이 발생한다.
- try-catch 구조로 범위를 정의했으므로 TDD를 사용해 나머지 논리를 추가한다.
- 나머지 논리는 FileInputStream을 생성하는 코드와 close 호출문 사이에 넣으면 오류나 예외가 전혀 발생하지 않는다고 가정한다.

## 미확인 예외를 사용하라

<aside>
💡 예외에는 **확인된 예외** / **미확인 예외**가 있다.

</aside>

- 처음엔 확인된 예외를 멋진 아이디어라 생각했다. (물론 지금은 반드시 필요하지는 않다는 사실이 분명해졌다. )
  - 실제로 확인된 예외는 몇가지 장점을 제공하긴 한다.
  - C#, C++, 파이썬, 루비는 확인된 예외를 지원하지 않는다.
- 아주 중요한 라이브러리를 작성한다면 모든 예외를 잡아야 한다.
- 하지만 일반적인 애플리케이션은 의존성이라는 비용이 이익보다 크다.

### 확인된 예외는 OCP를 위반한다.

- Open Closed Principle (개방 폐쇄 원칙)
  - `뜻` 클래스나 모듈은 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 한다
  - 하위에서 단계에서 코드를 변경하면 상위 단계 메서드 선언부를 모두 고쳐야 한다.
- 대규모 시스템에 대한 예시
  - 최상위 함수가 아래 함수를 호출
  - 아래 함수는 그 아래 함수를 호출
  - 단계를 내려갈수록 호출하는 함수 수는 늘어난다.
  - 이제 최하위 함수를 변경해 새로운 오류를 던진다고 가정하다.
    - 변경한 함수를 호출하는 함수 모두가 catch블록에서 새로운 예외를 처리하거나
    - 선언부에 throw 절을 추가해야 한다.
      → 결과적으로 최하위 단계에서 최상위 단계까지 연쇄적인 수정이 일어난다.
      → throws 경로에 위치하는 모든 함수가 최하위 함수에서 던지는 예외를 알아야 하므로 캡슐화가 깨진다.

### 예외에 의미를 제공하라

- 예외를 던질 때는 전후 상황 충분히 덧붙인다.
  - 그러면 오류가 발생한 원인과 위치를 찾기가 쉬워진다.
- 오류 메세지에 정보를 담아 예외와 함께 던진다.
  - 실패한 연산 이름과 실패 유형을 언급해야 한다.
- 애플리케이션 로깅 기능을 사용한다면 catch 블록에서 오류를 기록하도록 충분한 정보를 넘겨준다.

### 호출자를 고려해 예외 클래스를 정의하라

- 오류가 발생한 위치로 오류를 분류할 수 있다.
  - 컴포넌트 / 유형 등등..
- 애플리케이션에서 **오류를 정의할 때는 오류를 잡아내는 방법**이 중요하다.

```java
// 다음은 예외를 형편없이 분류한 사례다.
ACMEPort port = new ACMEPort(12);

try {
    port.open();
} catch (DeviceResponseException e) {
    reportPortError(e);
    logger.log("Device response exception", e);
} catch (ATM1212UnlockedException e) {
    reportPortError(e);
    logger.log("Unlock response exception", e);
} catch (GMXError e) {
    reportPortError(e);
    logger.log("Device response exception", e);
} finally {
    ...
}
```

- 외부 라이브러리를 호출하는 `try-catch-finally` 문을 포함한 코드로, 외부 라이브러리가 던질 예외를 모두 잡아낸다.
- 대다수 상황에서 우리가 오류를 처리하는 방식은 비교적 일정하다.
  1. 오류를 기록한다.
  2. 프로그램을 계속 수행해도 좋을지 확인한다.

```java
// 호출하는 라이브러리의 API를 감싸면서 예외 유형 하나를 반환하면 어떨까?
public class LocalPort {
    private ACMEPort innerPort;

    public LocalPort(ACMEPort innerPort) {
        this.innerPort = innerPort;
    }

    public void open() {
        try {
            port.open();
        } catch (DeviceResponseException e) {
            **throw new PortDeviceFailure(e);**
        } catch (ATM1212UnlockedException e) {
            **throw new PortDeviceFailure(e);**
        } catch (GMXError e) {
            **throw new PortDeviceFailure(e);**
        }

        ...
    }
}
```

- LocalPort클래스는 ACMEPort 클래스가 throw하는 예외를 잡아 변환해주는 wrapper 클래스다.
- 외부 API를 사용할 때는 wrapper로 감싸는 것이 가장 좋다.
  - 나중에 다른 라이브러리로 갈아타도 비용이 적다.
  - wrapper 클래스에서 테스트코드를 넣어주는 방법으로 프로그램을 테스트하기도 쉬워진다.

## 정상 흐름을 정의하다

> _이제 비즈니스 논리와 오류처리가 잘 분리된 깨끗한 코드를 작성할 수 있게 되었다 !_

- 코드 대부분이 깨끗해보이지만, 오류 감지 코드가 프로그램 언저리로 밀려나고 있다.
- 외부 API를 감싸 독자적인 예외를 던지고, 코드 위에 처리기를 정의해 중단된 계산을 처리하지만, 때로는 중단이 적합하지 않은 때도 있다.

→ try/catch가 필요하지 않은 부분도 있다. 이럴 땐 조건으로 처리하자.

## null을 반환하지 마라

- null을 반환하는 습관은 나쁘다.
  - 누구라도 null확인을 빼먹는다면 애플리케이션이 큰일날 수 있다.
  - null이 아니라면 어떤 일이 일어날지 명확하지도 않다.
- null을 반환하고 싶다면, 그 대신 예외를 던지거나 특수 사례 객체를 반환해라
- 사용하려는 외부 API가 null을 반환한다면 wrapper 메서드를 구현해 예외를 던지거나 특정 조건을 처리하는 객체를 반환하는 방식을 고려해보자

## null을 전달하지 마라

- 메서드로 null을 전달하는 방식은 매우 나쁘다.
- 정상적인 인수로 null을 기대하는 API가 아니라면 메서드로 null을 전달하는 코드는 최대한 피한다.
- null을 기대하지 않는 함수에서 null을 인수로 받는다면, `NullPointerException`이 발생한다.
- 대다수 프로그래밍 언어는 호출자가 실수로 넘기는 null을 적절히 처리하는 방법이 없어, 처리하기 복잡하다.

## 결론

- 깨끗한 코드는 가독성도 높아야 하지만 안정성도 높아야 한다.
- **오류 처리를 프로그램 논리와 분리해 독자적인 사안으로 고려**해야 한다.
- **오류 처리를 프로그램 논리와 분리하면 독립적인 추론이 가능해지면 코드 유지보수성도 크게 높아진다.**

<br />
<br />

# 8장, 경계

> _시스템에 들어가는 모든 소프트웨어를 직접 개발하는 경우는 드물다.
> 어떤 식으로든 이 외부 코드를 우리 코드에 깔끔하게 통합해야만 한다.
> 어답터를 만들어서 분리하자!는 내용_

## 외부 코드 사용하기

- 패키지 제공자나 프레임워크 제공자는 적용성을 최대한 넓히려 애쓴다.
- 사용자는 자신의 요구에 집중하는 인터페이스를 바란다.

## 경계 살피고 익히기

- 외부 패키지 테스트가 우리의 책임은 아니지만, 우리 자산을 위해 우리가 사용할 코드를 테스트 하는 편이 바람직하다.
- 외부 코드를 익히기는 어렵고, 통합하기도 어렵다.
- 그럼 곧바로 우리쪽 코드를 작성해 외부 코드를 익히면 어떨까?
  1. 간단한 테스트 케이스를 작성한다.
  2. 프로그램에서 사용하려는 방식대로 외부 API를 호출한다.

## log4j 익히기

- 로깅 기능을 하는 라이브러리를 익혀보자.

### 문서를 자세히 읽기 전에 첫 번째 테스트 코드를 작성한다.

```java
@Test
public void testLogCreate() {
  Logger logger = Logger.getLogger("MyLogger");
  logger.info("hello");
}
```

### (반복) 오류가 발생하는 것을 보고 → 문서를 자세히 보고 → 수정한다.

```java
// Appender가 필요 !
@Test
public void testLogAddAppender() {
  Logger logger = Logger.getLoger("MyLogger");
  ConsoleAppender appender = new ConsolAppender();
  logger.addAppender(appender);
  logger.info("hello");
}
```

- 테스트코드가 성공할 때 까지 코드를 수정하다가, **성공한다면 모든 지식을 독자적인 로거 클래스로 캡슐화한다.**

## 아직 존재하지 않는 코드를 사용하기

- 또 다른 유형은 아는 코드와 모르는 코드를 분리하는 경계다.
- 상대 팀이 API를 아직 설계하지 않아 구체적은 방법을 모르는 상황
  1. 우리가 바라는 인터페이스를 정의하고, 클래스를 만들고, 메서드를 추가한다.
  2. 상대 팀이 API를 정의 한 후에는, `adapter`를 구현해 간극을 메운다.
     - `adapter 패턴`으로 API 사용을 캡슐화해 API가 바뀔 때 수정할 코드를 한곳으로 모은다.
  3. API 인터페이스가 나온 다음 테스트 케이스를 생성하 API를 올바르게 사용하는지 테스트한다.

## 깨끗한 경계 (결론)

- 소프트웨어 설계가 우수하다면 변경하는데 많은 투자와 재작업이 필요하지 않다.
- 통제하지 못하는 코드를 사용할 때는 너무 많은 투자를 하거나 향후 변경 비용이 지나치게 커지지 않도록 각별히 주의해야 한다.
- 경계에 위치하는 코드는 깔끔히 분리한다.
- 기대치를 정의하는 테스트케이스도 작성한다.
- 통제가 불가능한 외부 패키지에 의존하는 대신 통제가 가능한 우리 코드에 의존하는 편이 훨씬 좋다.
  - 외부 패키지를 호출하는 코드를 가능한 줄여 경계를 관리하자.
  - 새로운 클래스로 경계를 감싸거나 ADAPTER 패턴을 사용하여 우리가 원하는 인터페이스로 변환하자.

<br />
<br />

# 9장, 단위테스트

> _많은 프로그래머들이 제대로 된 테스트 케이스를 작성해야 한다는 중요한 사실을 놓쳐버렸다._

## TDD 법칙 세 가지

1. 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
2. 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
3. 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

<aside>
💡 실제 코드와 맞먹을 정도로 방대한 테스트 코드는 심각한 관리 문제를 유발하기도 한다.

</aside>

## 깨끗한 테스트 코드 (가독성)

- 테스트코드의 품질은 실제 코드 만큼 중요하다.
- 단위 테스트는 유연성, 유지모수성, 재사용성을 제공한다.
- 테스트 케이스가 있으면 변경이 쉬워진다.

BUILD-OPERATE-CHECK 패턴이 아래와 같은 테스트 구조에 적합하다.

```java
// 테스트 자료를 만든다.
public void testGetPageHierarchyAsXml() throws Exception {
  makePages("PageOne", "PageOne.ChildOne", "PageTwo");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
}

// 테스트 자료를 조작한다.
public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception {
  WikiPage page = makePage("PageOne");
  makePages("PageOne.ChildOne", "PageTwo");

  addLinkTo(page, "PageTwo", "SymPage");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
  assertResponseDoesNotContain("SymPage");
}

// 조작한 자료가 올바른지 확인한다.
public void testGetDataAsXml() throws Exception {
  makePageWithContent("TestPageOne", "test page");

  submitRequest("TestPageOne", "type:data");

  assertResponseIsXML();
  assertResponseContains("test page", "<Test");
}
```

- 흔히 쓰는 시스템 조작 API를 사용하는 대신 **API 위에다 함수나 유틸리티를 구현한 후 그 함수와 유틸리티를 사용**하므로, 테스트 코드를 읽고 쓰기가 쉬워진다.

## 이중 표준

- 테스트 API에 적용하는 표준은 실제 코드에 적용하는 표준과 확실히 다르다.

```java
@Test
public void turnOnCoolerAndBlowerIfTooHot() throws Exception {
  tooHot();
  assertEquals("hBChl", hw.getState());
}

@Test
public void turnOnHeaterAndBlowerIfTooCold() throws Exception {
  tooCold();
  assertEquals("HBchl", hw.getState());
}

@Test
public void turnOnHiTempAlarmAtThreshold() throws Exception {
  wayTooHot();
  assertEquals("hBCHl", hw.getState());
}

@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
  wayTooCold();
  assertEquals("HBchL", hw.getState());
}
```

- 의미만 안다면 문자열을 읽기만 해도 결과를 판단할 수 있다.

## 테스트당 assert 하나?

- Template Method 패턴을 이용하면 중복을 제거할 수 있다.
  - given / when 부분을 부모 클래스에두고, than 부분을 자식 클래스에 두면 된다.
  - given / when 부분을 @Before 함수에, than 부분을 @Test 함수에 넣는 방법도 있다.
- 훌륭한 지침이지만, 때로는 여러 assert문이 필요할 때도 있다.
  → 테스트당 개념 하나라는 것이 좀 더 좋을 것 같다.
- 테스트 함수 한개 → 개념 하나만 테스트하기

## F.I.R.S.T

- Fast: 테스트는 빨라야 한다.
- Independent: 테스트는 서로 의존하지 않고 독립적이어야 한다.
- Repeatable: 테스트는 환경에 구애받지 않고 반복 가능해야 한다.
- Self-Validating: 테스트는 boolean 값으로 결과를 내야 한다.
- Timely: 테스트는 적시에 작성해야 한다. ( ex. 단위 테스트는 실제코드를 작성하기 전에 작성해야 한다. )

## 결론

- 테스트코드는 매우 중요하다.
- 테스트 API를 구현해 도메인 특화 언어를 만들자.
- 테스트 코드가 방치되어 망가지면 실제 코드도 망가진다. 깨끗하게 유지해라.
