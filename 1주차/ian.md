### 📅 2023년 1월 11일

# 📚 1장 깨끗한 코드

- 론 제프리스

  - 모든 테스트를 통과한다.
  - 중복이 없다.
  - 시스템 내 모든 설계 아이디어를 표현한다.
  - 클래스, 메서드, 함수 등을 최대한 줄인다.

> 짤막한 문단 몇 개로 론은 이 책 내용을 요약했다. 중복을 피하라. 한 기능만 수행하라. 제대로 표현하라. 작게 추상화하라. 이상이다.

- 코드를 읽는 시간 대 코드를 짜는 시간 비율이 10 대 1을 훌쩍 넘는다. 새 코드를 짜면서 우리는 끊임없이 기존 코드를 읽는다.
- 기존 코드를 읽어야 새 코드를 짜므로 읽기 쉽게 만들면 사실은 짜기도 쉬워진다.
- SOLID 원칙의 중요성

# 📚 2장 의미 있는 이름

## 암시하는 이름은 사용하지 말고 의미있는 변수 이름을 사용하세요.

### 예시1)

**Bad:**

```jsx
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Good:**

```jsx
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

### 예시2)

**Bad:**

```jsx
// 코드의 맥락이 코드 자체에 명시적으로 드러나지 않는다.
function getThem() {
  let list1 = [];
  for (const x of theList) {
    if (x[0] === 4) list1.push(x);
  }
  return list1;
}
```

**Good:**

```jsx
// 각 개념에 이름이 붙어있어서 역할을 파악하기 쉽다.
function getThem() {
  let flaggedCells = [];
  for (const cell of gameBoard) {
    if (cell[STATUS_VALUE] === FLAGGED) flaggedCells.push(cell);
  }
  return flaggedCells;
}
```

**Better:**

```jsx
// isFlagged라는 명시적인 함수를 사용해 FLAGGED라는 상수를 감췄다.
function getThem() {
  let flaggedCells = [];
  for (const cell of gameBoard) {
    if (cell.isFlagged()) flaggedCells.push(cell);
  }
  return flaggedCells;
}
```

## 발음할 수 있는 변수 이름을 사용하세요.

- 발음할 수 없는 이름은 그 변수에 대해서 바보 같이 소리를 내 토론할 수밖에 없습니다.

**Bad:**

```jsx
type DtaRcrd102 = {
  genymdhms: Date,
  modymdhms: Date,
  pszqint: number,
};
```

**Good:**

```jsx
type Customer **=** {
  generationTimestamp: Date;
  **modificationTimestamp**: Date;
  **recordId**: number;
}
```

## 검색할 수 있는 이름을 사용하세요.

- 코드를 쓸 때보다 읽을 때가 더 많기 때문에 우리가 쓰는 코드는 읽을 수 있고 검색이 가능해야 합니다. 프로그램을 이해할 때 의미있는 변수 이름을 짓지 않으면 읽는 사람으로 하여금 어려움을 줄 수 있습니다. 검색 가능한 이름을 지으세요.

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

## 불필요한 문맥은 추가하지 마세요.

**Bad:**

```jsx
type Car = {
  carMake: string,
  carModel: string,
  carColor: string,
};

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Good:**

```jsx
type Car = {
  make: string,
  model: string,
  color: string,
};

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

## 단축 평가나 조건문 대신 기본 매개변수를 사용하세요.

**Bad:**

```jsx
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Good:**

```jsx
function loadPages(count: number = 10) {
  // ...
}
```

## 의도를 알려주기 위해 enum을 사용하세요.

**Bad:**

```jsx
const GENRE = {
  ROMANTIC: "romantic",
  DRAMA: "drama",
  COMEDY: "comedy",
  DOCUMENTARY: "documentary",
};

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // Projector의 선언
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
      // 실행되어야 하는 로직
    }
  }
}
```

**Good:**

```jsx
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // Projector의 선언
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // 실행되어야 하는 로직
    }
  }
}
```

# 📚 3장 함수
