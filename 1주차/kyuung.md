```
1장은 깨끗한 코드에 대한 여러 생각들이 담긴, 서론으로 보여서 따로 정리하지 않았습니다. 👀
```

# 2장, 의미있는 코드

## 의도를 분명하게 밝혀라

변수, 함수, 클래스 이름은 `존재 이유/수행 기능/사용 방법`과 같은 의도를 분명히 드러내야 한다.

```
// 맥락이 명시적으로 드러나지 않는 코드
const getTheme = () => list.map((x) => x[0] === 4)
```

- 0번째는 왜 중요한가?
- 4는 무슨 의미인가?
- 반환하는 list를 어떻게 사용하는가?

<br/>

```
// 각 개념에 이름을 붙인 코드
const getFlaggedCells1 = () => gameBoard.map((cell) => cell[STATUS_VALUE] === FLAGGED)
```

````
// 칸을 간단한 클래스로 만들어 isFlagged라는 명시적인 함수로 상수를 감추기
const getFlaggedCells2 = () => gameBoard.map((cell) => cell.isFlagged())```
````

<br/>

## 그릇된 정보를 피하라

- 서로 흡사한 이름을 사용하지 않도록 주의한다.
- 널리 쓰이는 의미있는 단어를 다른 의미로 사용해선 안된다.
- 유사한 개념은 유사한 표기법을 사용한다. 일관성이 떨어지는 표기법은 그릇된 정보다.

<br/>

## 의미있게 구분하라

- 컴파일러나 인터프리터만 통과하려는 생각으로 코드를 구현하는 프로그래머는 스스로 문제를 일으킨다.
- 연속적인 숫자를 덧붙인 이름은 의도적인 이름과 정반대다.
- Info나 Data는 의미가 불분명한 불용어다. ProductData와 ProductInfo는 개념을 구분하지 않은 채 이름만 달리한 경우다.
- customerInfo는 customer와, accountData는 account와, theMessage는 message와 구분이 안된다. 읽는 사람이 차이를 알도록 이름을 지어라.

<br/>

## 검색하기 쉬운 이름을 사용하라

- 문자 하나를 사용하는 이름과 상수는 텍스트 코드에서 쉽게 눈에 띄지 않는다는 문제점이 있다.
- s는 검색하기 어렵지만, sum은 검색하면 금방 찾을 수 있다.

<br/>

## 인코딩을 피하라

- **헝가리식 표기법** : 당시는 컴파일러가 타입을 점검하지 않았으므로 프로그래머에게 타입을 기억할 단서가 필요했다. 이제는 컴파일러가 타입을 강제하고 기억하므로, 변수 이름에 타입을 인코딩 할 필요가 없다.
- **멤버 변수 접두어** : 이제는 멤버 변수에 m\_이라는 접두어를 붙일 필요도 없다.
- **인터페이스 클래스와 구현 클래스** : 내가 다루는 클래스가 인터페이스라는 사실을 남에게 알리고 싶지 않다. 클래스 사용자는 그냥 ShapeFactory라고만 생각하면 좋겠다. ( ShapeFactoryImp가 IShapeFactory보다 좋다. )

<br/>

## 이름

- **클래스 이름** : 클래스 이름과 객체 이름은 명사나 명사구가 적합하다.
- **메서드 이름** : 메서드 이름은 동사나 동사구가 적합하다. `접근자/변경자/조건자`는 표준에 따라 값 앞에 `get,set,is`를 붙인다.

  <br/>

## 한 개념에 한 단어를 사용하라

- 추상적인 개념 하나에 단어 하나를 선택해 이를 고수한다. 똑같은 메서드를 클래스마다 fetch, retrieve, get으로 제각각 부르면 혼란스럽다.
- 동일 코드 기반에 controller, manager, driver를 섞어 쓰면 혼란스럽다.

  <br/>

## 말장난을 하지 마라

- 때로는 같은 맥락이 아닌데도 일관성을 고려해 동일한 단어를 선택해선 안된다.
- 지금까지 구현한 add 메서드는 모두가 기존값에 두 개를 더하거나, 이어서 새로운 값을 만든다고 가정하자. 새로 작성하는 메서드는 집합에 값 하나를 추가한다. 해당 메서드는 add가 아닌 insert나 append라는 이름이 적당하다.

  <br/>

## 해법 영역에서 가져온 이름을 사용하라

- 코드를 읽는 사람도 프로그래머라는 사실을 명심한다.
- 모든 이름을 문제 영역에서 가져오는 정책은 현명하지 못하다.
- JobQueue를 모르는 프로그래머가 있을까? 기술 개념에는 기술 이름이 가장 적합한 선택이다.

  <br/>

## 문제 영역에서 가져온 이름을 사용하라

- 적절한 프로그래머 용어가 없다면, 문제 영역에서 이름을 가져온다.
- 문제 영역 개념과 관련이 깊은 코드라면 문제 영역에서 이름을 가져와야 한다/

  <br/>

## 의미 있는 맥락을 추가하라

- 대다수의 이름은 스스로 의미가 분명하지 않다. 그래서 클래스/함수/이름 공간에 넣어 맥락을 부여한다.
- 모든 방법이 실패하면 마지막 수단으로 접두어를 붙인다.

```
// 맥락이 불분명한 변수
const printGuessStatistics = (candidate, count) => {
  let number
  let verb
  let pluralModifier

  if (count === 0) {
    number = 'no'
    verb = 'are'
    pluralModifier = 's'
  } else if (count === 1) {
    number = '1'
    verb = 'is'
    pluralModifier = ''
  } else {
    number = count.toString()
    verb = 'are'
    pluralModifier = 's'
  }

  const guessMessage = `there ${verb}, ${number}, ${candidate}, ${pluralModifier}`
  console.log(guessMessage)
}

printGuessStatistics('3', 5)

```

- 일단 함수가 좀 길다.
- 세 변수를 함수 전반에서 사용한다. ( `number/verb/pluralModifier` )

  <br/>

```
// GuessStatisticsMessage 클래스를 만들에 변수를 속하게 하기 + 함수 쪼개기

class GuessStatisticsMessage {
  number
  verb
  pluralModifier

  make(candidate, count) {
    this.createPluralDependentMessageParts(count)
    return `there ${this.verb}, ${this.number}, ${candidate}, ${this.pluralModifier}`
  }

  createPluralDependentMessageParts(count) {
    if (count === 0) this.thereAreNoLetters()
    else if (count === 1) this.thereIsOneLatter()
    else this.thereAreManyLetters(count)
  }

  thereAreManyLetters(count) {
    this.number = count.toString()
    this.verb = 'are'
    this.pluralModifier = 's'
  }

  thereIsOneLatter() {
    this.number = '1'
    this.verb = 'is'
    this.pluralModifier = ''
  }

  thereAreNoLetters() {
    this.number = 'no'
    this.verb = 'are'
    this.pluralModifier = 's'
  }
}

```

- GuessStatisticsMessage라는 클래스를 만든 후 세 변수를 클래스에 넣었다.
  이제 세 변수는 맥락이 분명해진다. ( GuessStatisticsMessage라는에 속한다. )
- 이렇게 맥락을 개선하면 함수를 쪼개기가 쉬워지므로 알고리즘도 좀 더 명확해진다.

  <br/>

## 불필요한 맥락을 없애라

- Gas Station Deluxe라는 어플을 만든다고 해서 모든 클래스 이름이 GSD로 시작하는 것은 바람직하지 못하다.
- 의미가 분명한 경우에 한해서, 짧은 이름이 긴 이름보다 좋다.
- accountAddress와 customerAddress는 클래스 인스턴스로는 좋은 이름이나, 클래스 이름으로는 적합하지 못하다. ( Address는 클래스 이름으로 적합하다. )
- 포트 주소, MAC주소, 웹 주소를 구분해야 한다면 PostalAddress, MAC, URI라는 이름도 괜찮다.

  <br/>
  <br/>

# 3장, 함수

## 작게 만들어라

- 함수를 만드는 첫째 규칙은 `작게`다.
- 중첩구조가 생길만큼 함수가 커져서는 안된다. 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안된다.
- 아래 정도가 적당한 길이의 함수다.

```
const renderPageWithSetupsAndTeardowns = (pageData, isSuite) => {
  try {
    if (isTestPage(pageData)) {
      includeSetupAndTeardownPages(pageData, isSuite)
      return pageData.getHtml()
    }
  } catch (err) {
    console.log(err)
  }
}
```

  <br/>

## 한 가지만 해라

- 함수는 한가지를 해야하며, 그 한가지를 잘 해야한다.
- 위에 예시 함수는 3가지 일을 하고 있다. ( 페이지가 테스트 페이지인지 판단하며, 그렇다면 설정 페이지와 해제 페이지를 넣는다. 그리고 페이지를 HTML로 렌더링한다. )
- 우리가 함수를 만드는 이유는 큰 개념을 다음 추상화 수준에서 여러 단계로 나눠 수행하기 위해서다.

  <br/>

## 추상화 수준은 하나로!

- 한가지 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야한다.
- 위 예시의 if문을 `includeSetupAndTeardownsIfTestPaste`라는 함수로 만든다면, 똑같은 내용을 다르게 표현했을 뿐 추상화 수준은 바뀌지 않는다.
- 한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다.
- 특정 표현이 근본 개념인지 아니면 세부사항인지 구분하기 어려워진다.

  <br/>

## 위에서 아래로 코드 읽기: 내려가기 규칙

- 코드는 위에서 아래로 이야기처럼 읽혀야 좋다.
- **내려가기 규칙** : 한 함수 다음에는 추상화 수준이 한 단계 낮은 함수가 온다. => 위에서 아래로 읽으면 함수 추상화 수준이 한 단계 낮은 함수가 온다.

  <br/>

## Switch 문

- switch문은 작게 만들기 어렵다.
- 본질적으로 switch문은 N가지를 처리하기 때문에, 한 가지 작업만 하는 switch문도 만들기 어렵다.
- 하지만 switch문을 사용하지 않을 수 없으니, **다형성을 이용하여** switch문을 저차원 클래스에 숨기고 절대로 반복하지 않는 방법이 있다.

```
const calculatePay = (employee) => {
  switch (employee.type) {
    case COMMISSIONED:
      return calculateCommissionedPay(employee)

    case HOURLY:
      return calculateHourlyPay(employee)

    case SALARIED:
      return calculateSalariedPay(employee)

    default:
      throw new InvalidEmployeeType(employee.type);
  }
}
```

- 위 예시 함수에는 아래와 같은 문제가 있다.
  - 함수가 길다.
  - 새 직원 유형을 추가하면 더 길어진다.
  - 한가지 작업만 수행하지 않는다.
  - SRP를 위반한다. => 코드를 변경할 이유가 여럿이기 때문
  - OCP를 위반한다. => 새 직원 유형을 추가 할 때마다 코드를 변경하기 때문
  - 위 함수와 구조가 동일한 함수가 무한정 존재할 수 있다.

```
class Employee {
    isPayDay() {
      ...
    }
    calculatePay() {
      ...
    };
    deliverPay() {
      ...
    }
}
class EmployeeFactory {
  makeEmployee (e) {
    switch (e.type) {
      case COMMISSIONED:
        return new CommissionedEmployee(e);
      case HOURLY:
        return new HourlyEmployee(e);
      case SALARIED:
        return new SalariedEmployee(e);
      default:
        throw new InvalidEmployeeType(e.type);
    }
  }
}
const calculatePay = (e) => {
    const employee = new EmployeeFactory.makeEmployee(e);
    return employee.calculatePay();
}
```

( 바로 위의 코드는 자바를 잘 몰라서, 변환할 수 없어.. [해당 블로그 링크](https://kyungyeon.dev/posts/72)를 참고했습니다. )

- switch문을 통해 적절한 파생 클래스의 인스턴스를 생성한다.
- calculatePay, isPayday, deliverPay등과 같은 함수는 Employee 인터페으스를 거쳐 호출된다.
- 그러면 다형성으로 인해 실제 파생 클래스의 함수가 실행된다.

  <br/>

## 서술적인 이름을 사용하라

- 함수가 작고 단순할수록 서술적인 이름을 고르기도 쉬워진다.
- 이름을 붙일 때는 일관성이 있어야 한다.
- 모듈 내에서 함수 이름은 같은 문구, 명사, 동사를 사용한다.

  - includeSetupAndTearedownPages, includeSetupPages, includeSuiteSetPage 등이 좋은 예다.

  <br/>

## 함수 인수

- 함수에서 인수 개수는 적을수록 좋다. 4개 이상이라면, 특별한 이유가 필요하다.
- 출력 인수는 입력 인수보다 이해하기 어렵다.
- 많이 쓰는 단항 형식
  - 인수에 boolean값을 기대하며 질문을 던지는 경우
  - 인수로 뭔가를 변환해 결과를 반환하는 경우
  - 이벤트 함수
- 플래그 인수
  - 함수에 부울 값을 넘기는 것은 true/false에 따라 다르게 처리한다는 뜻이며, 함수가 한꺼번에 여러가지 일을 한다고 말하는 것이다.
- 이항 함수
  - 인수가 2개인 함수는 인수가 1개인 함수보다 이해하기 어렵다.
  - 좌표계처럼 2가지 값이 꼭 필요하지 않는 이상, 가능하다면 단항 함수로 바꾸도록 해야한다.
- 삼항 함수
  - 인수가 3개인 함수는 2개인 함수보다 이해하기 어렵다.
  - 3항 함수를 만들 때는 신중히 고려해야 한다.
- 인수 객체
  - 인수가 2-3개 필요하다면 일부를 독자적인 클래스 변수로 선언할 수 있는지 확인한다.
- 인수 목록
  - 때로는 인수 개수가 가변적인 함수도 필요하다.
  - 가변 인수를 취하는 함수는 단항/이항/삼항 함수로 취급할 수 있으며, 이를 넘어서 인수를 사용할 경우에는 문제가 있다.
- 동사와 키워드

  - 함수의 의도나 인수의 순서와 의도를 제대로 표현하려면 좋은 함수 이름이 필수다.
  - 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다. `writeField(name)`
  - 함수 이름에 인수를 넣어 인수를 줄인다. assertEquals(message, expected, actual) => assertExpectedEqualsActual(expected, actual)

  <br/>

## 부수 효과를 일으키지 마라

- 부수효과는 다른 일을 몰래 하는 것이다.
- 시간적인 결합이나 순서 종속성을 초래할 수 있다.

```
const checkPassword = (userName, password) => {
    const user = User.find(userName)
    if(user){
        ...
        if(invalidPassword){
            Session.initialize()
            return true
        }
    }
    return false
}
```

- Session.initialize()로 인해, checkPassword는 의도치 않게 세션을 날려버릴 수 있다.
- 만약 시간적인 결합이 필요하다면 함수 이름에 `checkPasswordAndInitializeSession`이라고 분명히 명시한다.
  <br/>

## 오류 코드보다 예외를 사용하라

- `if (deletePage(page) === E_OK)` 보다, 예외 (try catch)를 사용하면 오류 처리 코드가 원래 코드에서 분리되므로 코드가 깔끔해진다.
- Try/Catch 블록 뽑아내기

  ```
  const deletePage = (page) => {
  try {
    deletePageAndReferences(page)
  } catch (e) {
    logError(e)
    }
  }

  ```

- 오류를 처리하는 함수는 한가지 오류만 처리해야한다.

  <br/>

## 구조적 프로그래밍

- 함수는 return문이 하나여야 한다.
- 루프 안에서 break나 continue, goto를 사용해선 안된다.
- 함수를 작게 만든다면, 간혹 return, break, continue를 여러차례 사용해도 괜찮다.
  - 오히려 때로는 단일 입/출구 규칙보다 의도를 표현하기 쉬워진다.
    <br/>

## 함수를 어떻게 짜죠..

- 처음에는 길고 복잡하지만, 코드를 다듬고 함수를 만들고 이름을 바꾸고 중복을 제거한다.
- 처음부터 단위테스트 코드를 작성하며, 수정할 때는 단위테스트를 통과해야한다.

  <br/>

## 결론

- 모든 시스템은 특정 응용 분야 시스템을 기술할 목적으로 프로그래머가 설계한 도메인 특화 언어로 만들어진다.
- 함수는 그 언어에서 동사며, 클래스는 명사다.
- 시스템은 구현할 프로그램이 아니라 풀어갈 이야기다. 프로그래밍 언어라는 수단을 사용해 좀 더 표현력이 강한 언어를 만들어 이야기를 표현한다. 시스템에서 발생하는 모든 동작을 설명하는 함수 계층이 바로 그 언어에 속한다.
