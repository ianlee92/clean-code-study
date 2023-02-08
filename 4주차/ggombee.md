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

---

# 9장
