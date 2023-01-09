```
1장은 깨끗한 코드에 대한 여러 생각이들 담긴, 서론으로 보여서 따로 정리하지 않았습니다. 👀
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
