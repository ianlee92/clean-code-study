### π 2023λ 1μ 11μΌ

# π 1μ₯ κΉ¨λν μ½λ

- λ‘  μ νλ¦¬μ€

  - λͺ¨λ  νμ€νΈλ₯Ό ν΅κ³Όνλ€.
  - μ€λ³΅μ΄ μλ€.
  - μμ€ν λ΄ λͺ¨λ  μ€κ³ μμ΄λμ΄λ₯Ό νννλ€.
  - ν΄λμ€, λ©μλ, ν¨μ λ±μ μ΅λν μ€μΈλ€.

> μ§€λ§ν λ¬Έλ¨ λͺ κ°λ‘ λ‘ μ μ΄ μ± λ΄μ©μ μμ½νλ€. μ€λ³΅μ νΌνλΌ. ν κΈ°λ₯λ§ μννλΌ. μ λλ‘ νννλΌ. μκ² μΆμννλΌ. μ΄μμ΄λ€.

- μ½λλ₯Ό μ½λ μκ° λ μ½λλ₯Ό μ§λ μκ° λΉμ¨μ΄ 10 λ 1μ νμ© λλλ€. μ μ½λλ₯Ό μ§λ©΄μ μ°λ¦¬λ λμμμ΄ κΈ°μ‘΄ μ½λλ₯Ό μ½λλ€.
- κΈ°μ‘΄ μ½λλ₯Ό μ½μ΄μΌ μ μ½λλ₯Ό μ§λ―λ‘ μ½κΈ° μ½κ² λ§λ€λ©΄ μ¬μ€μ μ§κΈ°λ μ¬μμ§λ€.
- SOLID μμΉμ μ€μμ±

# π 2μ₯ μλ―Έ μλ μ΄λ¦

## μμνλ μ΄λ¦μ μ¬μ©νμ§ λ§κ³  μλ―Έμλ λ³μ μ΄λ¦μ μ¬μ©νμΈμ.

### μμ1)

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

### μμ2)

**Bad:**

```jsx
// μ½λμ λ§₯λ½μ΄ μ½λ μμ²΄μ λͺμμ μΌλ‘ λλ¬λμ§ μλλ€.
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
// κ° κ°λμ μ΄λ¦μ΄ λΆμ΄μμ΄μ μ­ν μ νμνκΈ° μ½λ€.
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
// isFlaggedλΌλ λͺμμ μΈ ν¨μλ₯Ό μ¬μ©ν΄ FLAGGEDλΌλ μμλ₯Ό κ°μ·λ€.
function getThem() {
  let flaggedCells = [];
  for (const cell of gameBoard) {
    if (cell.isFlagged()) flaggedCells.push(cell);
  }
  return flaggedCells;
}
```

## λ°μν  μ μλ λ³μ μ΄λ¦μ μ¬μ©νμΈμ.

- λ°μν  μ μλ μ΄λ¦μ κ·Έ λ³μμ λν΄μ λ°λ³΄ κ°μ΄ μλ¦¬λ₯Ό λ΄ ν λ‘ ν  μλ°μ μμ΅λλ€.

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

## κ²μν  μ μλ μ΄λ¦μ μ¬μ©νμΈμ.

- μ½λλ₯Ό μΈ λλ³΄λ€ μ½μ λκ° λ λ§κΈ° λλ¬Έμ μ°λ¦¬κ° μ°λ μ½λλ μ½μ μ μκ³  κ²μμ΄ κ°λ₯ν΄μΌ ν©λλ€. νλ‘κ·Έλ¨μ μ΄ν΄ν  λ μλ―Έμλ λ³μ μ΄λ¦μ μ§μ§ μμΌλ©΄ μ½λ μ¬λμΌλ‘ νμ¬κΈ μ΄λ €μμ μ€ μ μμ΅λλ€. κ²μ κ°λ₯ν μ΄λ¦μ μ§μΌμΈμ.

**Bad:**

```jsx
// 86400000μ΄ λλμ²΄ λ­μ§?
setTimeout(restart, 86400000);
```

**Good:**

```jsx
// λλ¬Έμλ‘ μ΄λ£¨μ΄μ§ μμλ‘ μ μΈνμΈμ.
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

## λΆνμν λ¬Έλ§₯μ μΆκ°νμ§ λ§μΈμ.

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

## λ¨μΆ νκ°λ μ‘°κ±΄λ¬Έ λμ  κΈ°λ³Έ λ§€κ°λ³μλ₯Ό μ¬μ©νμΈμ.

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

## μλλ₯Ό μλ €μ£ΌκΈ° μν΄ enumμ μ¬μ©νμΈμ.

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
  // Projectorμ μ μΈ
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
      // μ€νλμ΄μΌ νλ λ‘μ§
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
  // Projectorμ μ μΈ
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // μ€νλμ΄μΌ νλ λ‘μ§
    }
  }
}
```

# π 3μ₯ ν¨μ

## ν¨μμ λ§€κ°λ³μλ 2κ° νΉμ κ·Έ μ΄νκ° μ΄μμ μλλ€

- ν¨μ λ§€κ°λ³μμ κ°μλ₯Ό μ ννλ κ²μ ν¨μλ₯Ό νμ€νΈνκΈ° μ½κ² λ§λ€μ΄μ£ΌκΈ° λλ¬Έμ λλΌμΈ μ λλ‘ μ€μν©λλ€. ν¨μ λ§€κ°λ³μκ° 3κ° μ΄μμΈ κ²½μ°, κ°κΈ° λ€λ₯Έ μΈμλ‘ μ¬λ¬ λ€λ₯Έ μΌμ΄μ€λ₯Ό νμ€νΈν΄μΌ νλ―λ‘ κ²½μ°μ μκ° λ§€μ° λ§μμ§λλ€.

- ν κ° νΉμ λ κ°μ λ§€κ°λ³μκ° μ΄μμ μΈ κ²½μ°κ³ , κ°λ₯νλ€λ©΄ μΈ κ°λ νΌν΄μΌ ν©λλ€. κ·Έ μ΄μμ κ²½μ°μλ ν©μ³μΌ ν©λλ€. λ κ° μ΄μμ λ§€κ°λ³μλ₯Ό κ°μ§ κ²½μ°, ν¨μκ° λ§μ κ²μ ν  κ°λ₯μ±μ΄ λμμ§λλ€. κ·Έλ μ§ μμ κ²½μ°, λλΆλΆ μμ κ°μ²΄λ νλμ λ§€κ°λ³μλ‘ μΆ©λΆν  κ²μλλ€.

- λ§μ λ§€κ°λ³μλ₯Ό μ¬μ©ν΄μΌ νλ€λ©΄ κ°μ²΄ λ¦¬ν°λ΄μ μ¬μ©νλ κ²μ κ³ λ €ν΄λ³΄μΈμ.

- ν¨μκ° κΈ°λνλ μμ±μ λͺννκ² νκΈ° μν΄,Β κ΅¬μ‘°λΆν΄κ΅¬λ¬Έμ μ¬μ©ν  μ μμ΅λλ€.

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

[νμ μ¨λ¦¬μ΄μ€](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)λ₯Ό μ¬μ©ν΄μ κ°λμ±μ λ λμΌ μ μμ΅λλ€:

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

## ν¨μλ ν κ°μ§λ§ ν΄μΌν©λλ€ (SRP)

- μ΄κ²μ μννΈμ¨μ΄ κ³΅νμμ λ¨μ°μ½ κ°μ₯ μ€μν κ·μΉμλλ€. ν¨μκ° ν κ°μ§ μ΄μμ μ­ν μ μνν  λ μμ±νκ³  νμ€νΈνκ³  μΆλ‘ νκΈ° μ΄λ €μμ§λλ€. ν¨μλ₯Ό νλμ νλμΌλ‘ μ μν  μ μμ λ, μ½κ² λ¦¬ν©ν λ§ν  μ μμΌλ©° μ½λλ₯Ό λμ± λͺλ£νκ² μ½μ μ μμ΅λλ€. μ΄ κ°μ΄λμμ μ΄ λΆλΆλ§ μκΈ°κ²μΌλ‘ λ§λ€μ΄λ λΉμ μ λ§μ κ°λ°μλ³΄λ€ μμ€ μ μμ΅λλ€.

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

## ν¨μλ λ¨μΌ νλμ μΆμνν΄μΌ ν©λλ€

- ν¨μκ° ν κ°μ§ μ΄μμ μΆμννλ€λ©΄ κ·Έ ν¨μλ λλ¬΄ λ§μ μΌμ νκ² λ©λλ€. μ¬μ¬μ©μ±κ³Ό μ¬μ΄ νμ€νΈλ₯Ό μν΄μ ν¨μλ₯Ό μͺΌκ°μΈμ.

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

## μ€λ³΅λ μ½λλ₯Ό μ κ±°ν΄μ£ΌμΈμ

- μ½λκ° μ€λ³΅λμ§ μλλ‘ μ΅μ μ λ€νμΈμ. μ€λ³΅λ μ½λλ μ΄λ€ λ‘μ§μ λ³κ²½ν  λ ν κ³³ μ΄μμ λ³κ²½ν΄μΌ νκΈ° λλ¬Έμ μ’μ§ μμ΅λλ€.

- λΉμ μ μ’μ’ λ κ° μ΄μμ μ¬μν μ°¨μ΄μ μ΄ μ‘΄μ¬νλ€κ³  μκ°ν΄μ κ±°μ λΉμ·ν μ½λλ₯Ό μ€λ³΅ μμ±ν©λλ€. νμ§λ§ κ·Έ λͺκ°μ§ λ€λ₯Έ κ²μΌλ‘ μΈν΄ κ°μ μ­ν μ νλ λ κ° μ΄μμ ν¨μλ₯Ό λ§λ€κ² λ©λλ€. μ€λ³΅λ μ½λλ₯Ό μ κ±°νλ κ²μ μ‘°κΈμ© λ€λ₯Έ μ­ν μ νλ κ²μ λ¬ΆμμΌλ‘μ¨ νλμ ν¨μ/λͺ¨λ/ν΄λμ€λ‘ μ²λ¦¬νλ μΆμνλ₯Ό λ§λλ κ²μ μλ―Έν©λλ€.

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

## `Object.assign`Β νΉμ κ΅¬μ‘° λΆν΄λ₯Ό μ¬μ©ν΄μ κΈ°λ³Έ κ°μ²΄λ₯Ό λ§λμΈμ

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

[νμ μ¨λ¦¬μ΄μ€](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)λ₯Ό μ¬μ©ν΄μ κ°λμ±μ λ λμΌ μ μμ΅λλ€:

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

## ν¨μ λ§€κ°λ³μλ‘ νλκ·Έλ₯Ό μ¬μ©νμ§ λ§μΈμ

- νλκ·Έ μΈμλ μΆνλ€. ν¨μλ‘ booleanμ λκΈ°λ κ² μμ²΄κ° ν¨μκ° μ¬λ¬ κ°μ§λ₯Ό μ²λ¦¬ νλ€κ³  λλκ³  κ³΅ννλ μμ΄λ€.
  ν¨μλ ν κ°μ§μ μΌμ ν΄μΌν©λλ€. boolean λ³μλ‘ μΈν΄ λ€λ₯Έ μ½λκ° μ€νλλ€λ©΄ κ·Έ ν¨μλ₯Ό μͺΌκ°λλ‘ νμΈμ.

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

## μ‘°κ±΄λ¬Έμ μΊ‘μννμΈμ

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

## λΆμ  μ‘°κ±΄λ¬Έμ νΌνμΈμ

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

## λμ¬μ ν€μλ

- λ¨ν­ ν¨μλ ν¨μμ μΈμκ° λμ¬/λͺμ¬ μμ μ΄λ€μΌ νλ€.
  - ex) writeField(name)
- ν¨μ μ΄λ¦μ ν€μλλ₯Ό μΆκ°νλ νμ. μ¦, ν¨μ μ΄λ¦μ μΈμ μ΄λ¦μ λ£λλ€.
  - assertEqualsλ³΄λ€ assertExpectedEqualsActual(expected, actual)μ΄ λ μ’λ€.
  - κ·Έλ¬λ©΄ μΈμ μμλ₯Ό κΈ°μ΅ν  νμκ° μμ΄μ§λ€.

## μ€λ₯ μ½λλ³΄λ€ μμΈλ₯Ό μ¬μ©νλΌ (Try/Catch λΈλ‘ λ½μλ΄κΈ°)

- μ€λ₯ μ½λ λμ  μμΈλ₯Ό μ¬μ©νλ©΄ μ€λ₯ μ²λ¦¬ μ½λκ° μλ μ½λμμ λΆλ¦¬λλ―λ‘ μ½λκ° κΉλν΄μ§λ€.
- μ μ λμκ³Ό μ€λ₯ μ²λ¦¬ λμμ λΆλ¦¬νλ©΄ μ½λλ₯Ό μ΄ν΄νκ³  μμ νκΈ° μ¬μμ§λ€.

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
