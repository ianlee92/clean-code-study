# 1장

`정리, 정돈, 청소, 청결, 생활화` 이 책에 추천사에 나와있는 단어들이다.

이 책을 통해 이것들을 어떻게 강조하고 있는지 이해를 했으면 좋겠다.

**코드 품질을 측정하는 유일한 척도 = 분당 내지르는 WTF 횟수**

장인 정신으로 코드를 짜는게 무엇일까?

좋은 코드, 나쁜 코드 그 차이는 무엇일까?

개발을 할때 항상 시간에 쫓기는 클래일이 다반사였고, 돌아가는 코드를 우선 만들고 나중에 리팩토링 하자 라는 생각을 많이 했다. (~~나도 안고칠건 알고있다~~)

역시나.

이 책에서는 그부분을 가장 먼저 꼬집었다.

이 책을 읽기 위한 마음 가짐을 먼저 달리하라는 의미로 받아들였다.

좋은 코드는 무엇일까..

- [ ] 모든 테스트를 통과하였는가?
- [ ] 중복이 없는가?
- [ ] 시스템 내 모든 설계 아이디어를 표현했는가?
- [ ] 클래스, 메서드, 함수 등을 최대한 줄였는가?

일단 위 세개는 모두 만족해야 되는 것 같고…

**‘주의’** : 누군가 주의 깊게 작성했다는 느낌이 들어야 한다. 그말은 즉 주의깊게 나중에 한다 생각하지말고 한번 할때 잘 작성해야 한다.

**‘가독성’** : 다른사람이 고치기 쉬운 코드여야 한다. 이건 정말 공감된다…

가독성이 좋은 코드는 그만큼 짤때 운영을 고려해서 짰을 거라 생각하고 그만큼 누가와도 고치기 쉬운 코드가 될 것이다.

---

# 2장

### 의도를 분명히 밝히자

명명을 할때 그것의 의도를 명시하는 단어를 사용하는 것이 좋다. 그래야 코드가 하는일을 짐작할 수 있다.

### 그릇된 정보를 피하라

리스트, 흡사한 이름 사용에 주의해야한다.

### 의미있게 구분하라

동일한 범위 안에서는 다른 두 개념에 같은 이름을 사용하지 않는다.

### 발음하기 쉬운 이름을 사용하라

서로 소통할때 발음 하기 어려운 이름은 재미는 있을지언정 문제를 일으킨다.

### 검색하기 쉬운 이름을 사용하라

검색가능한 코드, 유지보수를 생각한다. 숫자가 아닌 상수화를 하는게 어찌보면 중요하다.

**Bad:**

```jsx
// 86400000이 도대체 뭐지?
setTimeout(restart, 86400000);
```

**Good:**

```jsx
// 대문자로 이루어진 상수로 선언하세요.
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

### 인코딩을 피하라

헝가리식 표기법….(~~정처기….ㅆ…~~), 멤버 변수 접두어, 인터페이스 클래스와 구현 클래스.. 모두 고려하자

### 메서드 이름은 동사

값앞에 get,set,is를 붙이는게 적당하다.

### 기발한 이름은 피하라

~~Holy~~.. 기발한 이름은 피하자.

### 한 개념에 한단어 사용

그리고.. 이건 좀 공감하는 또 다른 부분인데 제발… 같은 목적을 하는 메서드를 get,fetch 이런식으로 각각 부르지 말자…😅

### 말장난을 하지마라

add,insert,append 구분 잘해서 사용하자

### 해법 영역에서 가져온 이름을 사용하라

특정 이론이나.. 모를만한 …이름으로 자랑하지말자..

### 문제 영역에서 가져온 이름을 사용하라

문제영역과 해법 영역을 구분할 수 있어야 한다.

### 의미 있는 맥락을 추가하라

여러개의 상품에 대한 변수가 있을때 그 상품을 구분하는 카테고리 접두어라던지 … 이런것들을 두어 의미있는 변수로 만들어보자🥺

### 불필요한 맥락을 없애라

클래스/타입/객체의 이름에 의미가 있다면, 그안에 있는 변수들은 굳이 특정 의미를 반복해서 담지 않아도 된다… 근데 이건 상황에 따라 조금 다를듯…ㅎㅎ

---

# 3장

함수의 매개변수는 2개 혹은 그 이하가 이상적..

(테스트의 경우의 수 줄이기)

함수 속성을 명확히 하기 위해, [구조 분해](https://basarat.gitbook.io/typescript/future-javascript/destructuring) 구문을 사용할 수 있다. 장점은..

1. 어떤 사람이 함수 시그니쳐(매개변수의 타입, 반환값의 타입 등)를 볼 때, 어떤 속성이 사용되는지 즉시 알 수 있다.
2. 명명된 매개변수처럼 보이게 할 때 사용할 수 있다.
3. 또한 구조 분해는 함수로 전달된 매개변수 객체의 특정한 원시 값을 복제하며 이것은 사이드 이펙트를 방지하는데 도움을 준다. 유의사항: 매개변수 객체로부터 구조 분해된 객체와 배열은 **복제되지 않는다.**
4. 타입스크립트는 사용하지 않은 속성에 대해서 경고를 주며, 구조 분해를 사용하면 경고를 받지 않을 수 있다.

### 한 가지만 해라

한가지 이상의 역할을 하게 만든다면.. 그에 따른 테스트를 만들기는 더더욱 어려워지는 상황 발생..

한가지를 잘하게 만들자.

### 함수가 무엇을 하는지 알 수 있도록 함수 이름을 지어라

즉, 함수 내부동작에서 하는 어떠한 일을 요약한게 바로 함수 이름이 되어야 한다.

### 함수 당 추상화 수준은 하나로!

단일행동을 추상화 해야한다. 함수가 한가지 이상을 추상화 한다면,, 너무 많은 일을 하게 되니 주의하자. 항상 재사용성과 테스트를 고려하면서 코드를 짜자.

### 중복된 코드를 제거하라,, 반복하지 마라!

이건 정말정말 중요하다. 항상 중복된 코드가 없나 확인하는 습관을 갖자.

추상화를 올바르게 하는 것은 중요하며, 이것은 [SOLID](https://738.github.io/clean-code-typescript/#solid) 원칙을 따르는 이유기도 하다.

### 부수 효과를 일으키지 마라!

시간적인 결합을 일으키는 부수효과는 혼란을 일으킨다.

### 오류코드보다 예외를 사용하라

명령 함수에서 오류 코드를 반환하는 방식은 명령/조회 분리 규칙을 미묘하게 위반한다.

즉 상황에 따라 달라질수도 있겠지만, 오류처리를 항상 고려하면서 코드를 짜는것이 중요하다.
