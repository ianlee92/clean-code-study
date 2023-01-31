# 6장, 객체와 자료 구조객체와 자료 구조

<aside>
💡 어째서 수많은 프로그래머가 get 함수와 set 함수를 당연하게 public으로 사용할까?

</aside>
<br/>

## 자료 추상화

- 자료를 세세하게 공개하기보다는 추상적인 개념으로 표현하는 편이 좋다.

### ❌ 자료를 세세하게 공개하는 코드

```java
public class Point {
  private double x;
  private double y;
}
```

- 극좌표계를 사용하는구나 라고 구체적으로 알 수 있다.

```java
public interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
}
```

- 변수를 읽어 반환하는 함수라는 것을 알 수 있다.
- 아무 생각 없이 조회/설정 함수를 추가하는 방법이 가장 나쁘다.

### ✅ 자료를 추상적인 개념으로 표현하는 코드

```java
public interface Point {
  double getX();
  double getY();
  void setCatesian(double x, double y);
  double getR();
  double getTheta();
  void setPolar(double r, double theta);
}
```

- 정확히 어떤 좌표계를 사용하는지 알 수 없다.

```java
public interface Vehicle {
  double getPercentFuelRemaining();
}
```

- 자동차 연료 상태를 백분율이라는 추상적인 개념으로 알려준다.

# 자료 / 객체 비대칭

- 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개된다.
- 자료 구조는 자료를 그대로 공개하며, 별다른 함수는 제공하지 않는다.

<aside>
💡 때로는 단순한 자료구조와 절차적인 코드가 가장 적합한 상황도 있다.

</aside>

### 절차적인 코드

- 새로운 함수를 추가하기 쉽우며, 새로운 자료구조를 추가하기 어렵다.
- 새로운 자료타입보다 새로운 함수가 필요한 경우, 절차적인 코드가 적합하다.

```java
// 도형 클래스 1
public class Square {
	public Point topLeft;
	public double side;
}

// 도형 클래스 2
public class Rectangle {
	public Point topLeft;
	public double height;
	public double width;
}

// 도형 클래스 3
public class Circle {
	public Point center;
	public double radius;
	public double width;
}

// 도형이 동작하는 방식을 제공하는 Geometry 클래스
public class Geometry {
	public final double PI = 3.141592653585793;

	public double area(Object shape) throws NoSuchShapeException {
		if(shape instanceOf Square) {
			Square s = (Square)shape;
			return s.side * s.side;
		}
	else if(shape instanceOf Rectangle) {
			Rectangle r = (Rectangle)shape;
			return r.height * r.width;
		}
	else if(shape instanceOf Circle) {
			Circle c = (Circle)shape;
			return PI * c.radius * c.radius
		}
	}
}
```

- 3개의 도형 클래스는 간단한 **자료구조**이며, **아무런 메서드도 제공하지 않는다.**
- 위 코드는 클래스가 절차적이지만, Geometry클래스에 메서드를 추가해도 도형 클래스들은 아무 영향을 받지 않는다.
- 하지만 새로운 도형을 추가하려면, Geometry클래스에 속한 모든 메서드를 수정해야 한다.

### 객체지향 코드

- 새로운 자료구조를 추가하기 쉬우며, 새로운 함수를 추가하기 어렵다.
- 새로운 자료타입이 필요한 경우 객체 지향 기법이 적합하다.

```java
public class Square implements Shape {
	public Point topLeft;
	public double side;

	public double area() {
		return side*side;
	}
}

public class Rectangle implements Shape {
	public Point topLeft;
	public double height;
	public double width;

	public double area() {
		return height*width;
	}
}

public class Circle implements Shape {
	public Point center;
	public double radius;
	public double width;

	**public double area() {
		return PI*radius*radius;
	}**
}
```

- area()는 다형 메서드여서, 새 도형을 추가해도 기존 함수에 아무런 영향을 미치지 않는다.
- 하지만 새로운 함수를 추가하려면, 도형 클래스를 전부 고쳐야 한다.

## 디미터 법칙

- 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙
- 객체 조회 함수로 내부 구조를 공개하면 안된다는 의미다.
- class C의 메서드 f는 아래와 같은 객체의 메서드만 호출해야 한다.
  - class C
  - f가 생성한 객체
  - f가 인수로 넘어온 객체
  - C 인스턴스 변수에 저장된 객체

### 기차 충돌

- 다음 코드는 디미터 법칙을 어긴다.
  - `String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();`
  - 각 함수가 반환하는 객체의 함수를 호출한 후 반환하는 객체의..로 이어지기 때문
- 이 코드는 일반적으로 조잡해 보이기 때문에 아래처럼 작성하는 것이 좋다.

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
String outputDir = scratchDir.getAbsolutePath();
```

```java
final String outputDir = cxtx.opts.scratchDir.absolutePath;
```

- 조회 함수를 사용하지 않고, 위와 같이 자료구조로 나열하면 디미터 법칙을 논할 필요가 없다.
  - 자료 구조 → 공개 변수만 포함
  - 객체 → 비공개 변수 & 공개 함수 포함
- 하지만, 단순한 자료 구조에서 조회 함수와 설정 함수를 정의하라 요구하는 프레임워크와 표준도 존재하긴 한다. ( ex. bean )

### 잡종 구조

- 때때로 절반은 객체, 절반은 자료 구조인 잡종 구조가 나온다.
  - 중요한 기능을 수행하는 함수
  - 비공개 변수를 그대로 노출하는 공개 변수나 공개 조회/설정 함수
- 새로운 함수, 새로운 자료구조를 추가하기 어렵다.

### 구조체 감추기

- 객체라면 뭔가를 하라고 말해야 한다. 속을 드러내라고 말하면 한된다.

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
String outputDir = scratchDir.getAbsolutePath();
```

위 코드에서, ctxt, options, scratchDir이 진짜 객체라면?

- 객체라면 내부 구조를 감춰야 하니까, 줄줄이 엮어서는 안된다.

위 코드에서 하고 싶은 일은 절대 경로를 가져오는 것인데, 아래에서 해당 경로로 임시 파일을 생성한다.

그렇다면 ctxt에게 임시 파일을 생성하하라고 하면 적당한 코드가 된다.

- ctxt는 내부 구조를 드러내지 않는다.
- 모듈에서 해당 함수는 자신이 몰라야 하는 여러 객체를 탐색할 필요가 없다. ⇒ 디미터 법칙을 위반하지 않는다.

## 자료 전달 객체 DTO

- 주료 구조체의 전형적인 형태는 공개 변수만 있고 **함수가 없는 클래스**다.
- 이런 자료 구조체를 때로는 **자료 전달 객체 (DTO)** 라고 한다.
- DTO는 데이터베이스와 통신하거나 소켓에서 받은 메세지의 구문을 분석할 때 유용하다.
- 흔히 DTO는 **데이터베이스에 저장된 가공되지 않은 정보를 애플리케이션 코드에서 사용할 객체로 변환하는 일련의 단계에서 가장 처음으로 사용하는 구조체다.**

좀 더 일반적인 형태는 bean 구조다.

비공개 변수를 조회/설정 함수로 조작한다.

```java
class Address {
	private final String postalCode;

	private final String city;

	private final String street;

	private final String streetNumber;

	private final String apartmentNumber;

	public Address(String postalCode, String city, String street, String streetNumber, String apartmentNumber) {
		this.postalCode = postalCode;
		this.city = city;
		this.street = street;
		this.streetNumber = streetNumber;
		this.apartmentNumber = apartmentNumber;
	}

	public String getPostalCode() {
		return postalCode;
	}

	public String getCity() {
		return city;
	}

	public String street() {
		return street;
	}

	public String streetNumber() {
		return streetNumber;
	}

	public String apartmentNumber() {
		return apartmentNumber;
	}
}
```

### 활성 레코드

- 활성 레코드는 DTO의 특수한 형태다.
- 공개 변수가 있거나 비공개 변수에 조회/설정 함수가 있는 자료구조지만, 대게 save나 find같은 탐색 함수도 제공한다.
- 활성 레코드는 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한다.
- 활성 레코드에 비즈니스 규칙 메서드를 추가해 이러한 자료 구조를 객체로 취급하는 방식이 흔하지만, 이런 방식은 바람직하지 않다.
- 활성 레코드는 자료구조로 취급해야 한다.
  - 비즈니스 규칙을 담으면서 내부 자료를 숨기는 객체는 따로 생성한다.

## 결론

- 객체는 동작을 공개하고 자료를 숨긴다.
  - 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기는 쉬운 반면, 기존 객체에 새 동작을 추가하기는 어렵다.
  - 새로운 자료 타입을 추가하는 유연성이 필요할 때 적합하다.
- 자료 구조는 별다른 동작 없이 자료를 노출한다.
  - 기존 자료 구조에 새 동작을 추가하기는 쉬우나, 기존 함수에 새 자료구조를 추가하기는 어렵다.
  - 새로운 동작을 추가하는 유연성이 필요하면 자료구조 + 절차적인 코드가 적합하다.

<br />
<br />

# 10장, 클래스

## 클래스 체계

- 변수 목록
  - public static 상수가 있다면 제일 먼저 나온다,
  - private static은 그 다음에 나온다.
  - 비공개 인스턴스 변수가 그 다음에 나온다.
  - 공개 변수가 필요한 경우는 거의 없다.
- 함수 목록
  - 공개 함수가 나온다.
  - 비공개 함수는 자신을 호출하는 공개 함수 직후에 넣는다. ( 추상화 단계가 아래로 내려간다. )

## 캡슐화

- 변수와 유틸리티 함수는 공개하지 않는 편이 낫지만, 반드시 숨겨야 한다는 규칙도 없다.
- 같은 패키지 안에서 테스트 코드가 함수를 호출하거나 변수를 사용해야 한다면 그 함수나 변수를 protected로 선언하거나 패키지 전체로 공개한다.
  - 하지만 그 전에 비공개 상태를 유지할 것을 최대한 노력해볼 것

## 클래스는 작아야 한다

- 클래스의 크기는 매우 중요하다.
- 책임은 1개 , 메서드는 5개정도가 적당하다.
- 클래스 이름은 클래스 책임을 기술해야 한다.
  - 간결한 이름이 떠오르지 않다면 책임이 너무 많아서다.
  - Processor, Manager, Super와 같은 모호한 이름은 지양한다.
- 클래스 설명은 if and or but을 사용하지 않고서 25단어 내외로 가능해야 한다.

## 단일 책임 원칙 Single Responsibility Principle

- 클래스나 모듈을 변경할 이유가 단 하나뿐이어야 한다는 원칙
  - 책임이 하나여야 한다.
- 변경할 이유(책임)을 파악하려 하다보면, 코드를 추상화하기도 시워진다.
- 자잘한 단일 책임 클래스가 많아지면 큰 그림을 이해하기 어려워진다고 우려하는 경우
  - 클래스 크기가 어떻든 절대적인 부품 수는 비슷하며, 익힐 내용의 양은 비슷하다.
- 규모가 어느 수준에 이르는 시스템은 논리가 많고 복잡하기 때문에, 체계적인 정리가 필수다.

## 응집도

- 응집도가 높다는 말은, 클래스에 속한 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미다.
- 함수를 작게, 매개변수 목록을 짧게
- 각 클래스 메서드는 클래스 인스턴스 변수를 하나 이상 사용해야 한다.
- 일반적으로 메서드가 변수를 더 많이 사용할수록 메서드와 클래스의 응집도가 더 높다.
- 모든 인스턴스 변수를 메서드마다 사용하는 클래스는 응집도가 가장 높지만, 가능하지도 바람직하지도 않다.

```java
// 응집도가 높은 클래스
public class Stack {
    private int **topOfStack** = 0;
    List<Integer> elements = new LinkedList<Integer>();

    public int size() {
        return **topOfStack**;
    }

    public void push(int element) {
        **topOfStack**++;
        elements.add(element);
    }

    public int pop() throws PoppedWhenEmpty {
        if (topOfStack == 0)
            throw new PoppedWhenEmpty();
        int element = elements.get(--**topOfStack**);
        elements.remove(topOfStack);
        return element;
    }
}
```

- 함수를 작게, 매개변수 목록을 짧게 작성하다보면 때때로 몇몇 메서드만이 사용하는 인스턴스 변수가 아주 많아진다. 이 때 새로운 클래스로 쪼개야 한다.
- 응집도가 높아지도록 변수와 메서드를 적절히 분리해 새로운 클래스 두세 개로 쪼개준다.

### 응집도를 유지하면 작은 클래스 여럿이 나온다.

- 클래스가 응집력을 잃는다면 쪼개야 한다,
- 큰 함수를 작은 함수 여럿으로 쪼개다보면 종종 작은 클래스 여럿으로 쪼갤 수 있는 상황이 있다.

# 변경하기 쉬운 클래스

- 대다수 시스템은 지속적인 변경이 가해진다.

```java
// 향후 update 메서드가 추가되면 수정해야 하는 클래스
public class Sql {
    public Sql(String table, Column[] columns)
    public String create()
    public String insert(Object[] fields)
    public String selectAll()
    public String findByKey(String keyColumn, String keyValue)
    public String select(Column column, String pattern)
    public String select(Criteria criteria)
    public String preparedInsert()
    private String columnList(Column[] columns)
    private String valuesList(Object[] fields, final Column[] columns)
	private String selectWithCriteria(String criteria)
    private String placeholderList(Column[] columns)
}
```

- 새로운 SQL 문을 지원하려면 반드시 Sql 클래스에 손대야 한다.
- 기존 SQL문 하나를 수정하려고 해도 반드시 Sql클래스에 손대야 한다.

```java
// 공개 인터페이스를 각각 Sql클래스에서 파생하는 클래스로 변경한 코드
// 각 기능 메서드를 클래스로 분리했다.

abstract public class Sql {
	public Sql(String table, Column[] columns)
	abstract public String generate();
}
public class CreateSql extends Sql {
	public CreateSql(String table, Column[] columns)
	@Override public String generate()
}

public class SelectSql extends Sql {
	public SelectSql(String table, Column[] columns)
	@Override public String generate()
}

public class InsertSql extends Sql {
	public InsertSql(String table, Column[] columns, Object[] fields)
	@Override public String generate()
	private String valuesList(Object[] fields, final Column[] columns)
}

public class SelectWithCriteriaSql extends Sql {
	public SelectWithCriteriaSql(
	String table, Column[] columns, Criteria criteria)
	@Override public String generate()
}

public class Where {
	public Where(String criteria) public String generate()
	public String generate() {
}

public class ColumnList {
	public ColumnList(Column[] columns) public String generate()
	public String generate() {
}

...
```

- 비공개 메서드는 해당하는 파생 클래스로 옮겼다. `InsertSql`
- 모든 파생 클래스가 공통으로 사용하는 비공개 메서드는 `Where` 와 `ColumnList` 라는 두 유틸리티 클래스에 넣었다.
- 각 클래스는 극도로 단순하며, 테스트를 하기도 쉬워졌다.
- update 문을 추가할 때 기존 클래스를 변경할 필요도 없다.
  - `class UpdateSql extends Sql` 을 하면 다른 코드가 망가질 일도 없다.
- 위 클래스들은 한가지 일만 하며, 확장에 개방적이고 수정에 폐쇄적이다.
- 새 기능을 수정하거나 기존 기능을 변경할 때 건드릴 코드가 최소인 시스템 구조가 바람직하다.

## 변경으로부터 격리

- Portfolio class
  - 외부 API를 사용해 값을 계산

```java
// 외부 API를 요청 할 class의 interface (StockExchange)
public interface StockExchange {
	Money currentPrice(String symbol);
}
```

```java
// StockExchange를 인자로 받는 portfolio class
public Portfolio {
	**private StockExchange exchange;**
	public Portfolio(**StockExchange exchange**) {
		this.exchange = exchange;
	}
	// ...
}
```

위를 활용해, 테스트코드 작성

```java
public class PortfolioTest {
	private FixedStockExchangeStub exchange;
	private Portfolio portfolio;

	@Before
	protected void setUp() throws Exception {
		exchange = new FixedStockExchangeStub();
		exchange.fix("MSFT", 100);
		portfolio = new Portfolio(exchange);
	}

	@Test
	public void GivenFiveMSFTTotalShouldBe500() throws Exception {
		portfolio.add(5, "MSFT");
		Assert.assertEquals(500, portfolio.value());
	}
}
```

- 위와 같이 시스템의 결합도를 낮추면 유연성과 재사용성은 더욱 높아진다.
  - 결합도 낮다 ⇒ 시스템 요소가 다른 요소 / 변경으로부터 잘 격리되어 있다.
- 위는 DIP를 충족하는 클래스이다. ⇒ 상세한 구현이 아니라 **추상화에 의존해야 한다는 원칙**
- 위의 portfolio 클래스는 상세한 구현 클래스가 아닌, **StockExchange** 인터페이스에 의존한다.
  - 이 인터페이스는 주식 기호를 받아 현재 주식 가격을 반환한다는 추상적인 개념을 표현한다.
