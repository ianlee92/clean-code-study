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

## 함수의 매개변수는 2개 혹은 그 이하가 이상적입니다

- 함수 매개변수의 개수를 제한하는 것은 함수를 테스트하기 쉽게 만들어주기 때문에 놀라울 정도로 중요합니다. 함수 매개변수가 3개 이상인 경우, 각기 다른 인수로 여러 다른 케이스를 테스트해야 하므로 경우의 수가 매우 많아집니다.

- 한 개 혹은 두 개의 매개변수가 이상적인 경우고, 가능하다면 세 개는 피해야 합니다. 그 이상의 경우에는 합쳐야 합니다. 두 개 이상의 매개변수를 가질 경우, 함수가 많은 것을 할 가능성이 높아집니다. 그렇지 않은 경우, 대부분 상위 객체는 하나의 매개변수로 충분할 것입니다.

- 많은 매개변수를 사용해야 한다면 객체 리터럴을 사용하는 것을 고려해보세요.

- 함수가 기대하는 속성을 명확하게 하기 위해, 구조분해구문을 사용할 수 있습니다.

**Bad:**

```jsx
function createMenu(
  title: string,
  body: string,
  buttonText: string,
  cancellable: boolean
) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);
```

**Good:**

```jsx
function createMenu(options: {
  title: string,
  body: string,
  buttonText: string,
  cancellable: boolean,
}) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

**Better:**

[타입 앨리어스](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)를 사용해서 가독성을 더 높일 수 있습니다:

```jsx
type MenuOptions = {
  title: string,
  body: string,
  buttonText: string,
  cancellable: boolean,
};

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

## 함수는 한 가지만 해야합니다 (SRP)

- 이것은 소프트웨어 공학에서 단연코 가장 중요한 규칙입니다. 함수가 한 가지 이상의 역할을 수행할 때 작성하고 테스트하고 추론하기 어려워집니다. 함수를 하나의 행동으로 정의할 수 있을 때, 쉽게 리팩토링할 수 있으며 코드를 더욱 명료하게 읽을 수 있습니다. 이 가이드에서 이 부분만 자기것으로 만들어도 당신은 많은 개발자보다 앞설 수 있습니다.

**Bad:**

```jsx
function emailClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```jsx
function emailClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

## 함수는 단일 행동을 추상화해야 합니다

- 함수가 한 가지 이상을 추상화한다면 그 함수는 너무 많은 일을 하게 됩니다. 재사용성과 쉬운 테스트를 위해서 함수를 쪼개세요.

**Bad:**

```jsx
function parseCode(code: string) {
  const REGEXES = [
    /* ... */
  ];
  const statements = code.split(" ");
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good:**

```jsx
const REGEXES = [
  /* ... */
];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(" ");
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

## 중복된 코드를 제거해주세요

- 코드가 중복되지 않도록 최선을 다하세요. 중복된 코드는 어떤 로직을 변경할 때 한 곳 이상을 변경해야 하기 때문에 좋지 않습니다.

- 당신은 종종 두 개 이상의 사소한 차이점이 존재한다고 생각해서 거의 비슷한 코드를 중복 작성합니다. 하지만 그 몇가지 다른 것으로 인해 같은 역할을 하는 두 개 이상의 함수를 만들게 됩니다. 중복된 코드를 제거하는 것은 조금씩 다른 역할을 하는 것을 묶음으로써 하나의 함수/모듈/클래스로 처리하는 추상화를 만드는 것을 의미합니다.

**Bad:**

```jsx
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink,
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio,
    };

    render(data);
  });
}
```

**Good:**

```jsx
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    };
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    };
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

## `Object.assign` 혹은 구조 분해를 사용해서 기본 객체를 만드세요

**Bad:**

```jsx
type MenuConfig = {
  title?: string,
  body?: string,
  buttonText?: string,
  cancellable?: boolean,
};

function createMenu(config: MenuConfig) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: "Bar" });
```

**Good:**

```jsx
type MenuConfig = {
  title?: string,
  body?: string,
  buttonText?: string,
  cancellable?: boolean,
};

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true,
    },
    config
  );

  // ...
}

createMenu({ body: "Bar" });
```

**Better:**

[타입 앨리어스](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)를 사용해서 가독성을 더 높일 수 있습니다:

```jsx
type MenuConfig = {
  title?: string,
  body?: string,
  buttonText?: string,
  cancellable?: boolean,
};

function createMenu({
  title = "Foo",
  body = "Bar",
  buttonText = "Baz",
  cancellable = true,
}: MenuConfig) {
  // ...
}

createMenu({ body: "Bar" });
```

## 함수 매개변수로 플래그를 사용하지 마세요

- 플래그 인수는 추하다. 함수로 boolean을 넘기는 것 자체가 함수가 여러 가지를 처리 한다고 대놓고 공표하는 셈이다.
  함수는 한 가지의 일을 해야합니다. boolean 변수로 인해 다른 코드가 실행된다면 그 함수를 쪼개도록 하세요.

**Bad:**

```jsx
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**

```jsx
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

## 조건문을 캡슐화하세요

**Bad:**

```jsx
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**Good:**

```jsx
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

## 부정 조건문을 피하세요

**Bad:**

```jsx
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**Good:**

```jsx
function isEmailUsed(email): boolean {
  // ...
}

if (!isEmailUsed(node)) {
  // ...
}
```

## 동사와 키워드

- 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야 한다.
  - ex) writeField(name)
- 함수 이름에 키워드를 추가하는 형식. 즉, 함수 이름에 인수 이름을 넣는다.
  - assertEquals보다 assertExpectedEqualsActual(expected, actual)이 더 좋다.
  - 그러면 인수 순서를 기억할 필요가 없어진다.

## 오류 코드보다 예외를 사용하라 (Try/Catch 블록 뽑아내기)

- 오류 코드 대신 예외를 사용하면 오류 처리 코드가 원래 코드에서 분리되므로 코드가 깔끔해진다.
- 정상 동작과 오류 처리 동작을 분리하면 코드를 이해하고 수정하기 쉬워진다.

**Bad:**

```jsx
if (deletePage(page) === E_OK) {
  if (registry.deleteReference(page.name === E_OK)) {
    if (configKeys.deleteKey(page.name.makeKey() === E_OK)) {
      logger.log("page deleted");
    } else {
      logger.log("configKey not deleted");
    }
  } else {
    logger.log("deleteReference from registry failed");
  }
} else {
  logger.log("delete failed");
  return E_ERROR;
}
```

**Good:**

```jsx
function deletePage(page) {
  try {
    deletePageAndAllReferences(page);
  } catch (e) {
    logError(e);
  }
}
function deletePageAndAllReferences(page) {
  deletePage(page);
  registry.deleteReference(page.name);
  configKeys.deleteKey(page.name.makeKye());
}
function logError(e) {
  logger.log(e.getMessage());
}
```
