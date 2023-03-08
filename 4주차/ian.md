### 📅 2023년 2월 22일

# 📚 7장 오류 처리

- 클린 코드는 읽기도 좋아야 하지만 안정성도 높아야 한다.

- 예외처리를 비즈니스 로직과 분리해 독자적인 사안으로 고려하면 튼튼하고 깨끗한 코드를 작성할 수 있다.

## null을 전달하지 마라

**Bad:**

```java
public class MetricsCalculator
{
 public double xProjection(Point p1, Point p2) {
 return (p2.x – p1.x) * 1.5;
 }
 …
}
```

**Good:**

```java
public class MetricsCalculator
{
 public double xProjection(Point p1, Point p2) {
  if (p1 == null || p2 == null) {
    throw InvalidArgumentException(
    "Invalid argument for MetricsCalculator.xProjection");
  }
  return (p2.x – p1.x) * 1.5;
 }
}
```

# 📚 8장 경계

- 시스템에 들어가는 모든 소프트웨어를 직접 개발하는 경우는 드물다. 패키지를 사거나, 오픈 소스를 이용하거나, 사내 다른 팀이 제공하는 외부 코드를 우리 코드에 깔끔하게 통합해야만 한다.

## 경계 살피고 익히기

- 사용법이 분명치 않은 타사 라이브러리를 가져왔다고 가정하자. 하루나 이틀간 문서를 읽으며 사용법을 결정하고, 우리 쪽 코드를 작성해 라이브러리가 예상대로 동작하는지 확인한다. 때로는 버그가 작성한 코드에서 발생한 것인지, 라이브러리에서 발생한 것인지 찾아내느라 오랜 시간 골머리를 앓기도 한다.

- 외부 코드를 익히긴 어렵다. 외부 코드를 통합하기도 어렵다. 두 가지를 동시에 하는 건 배로 어렵다. 우리 쪽 코드를 작성해 외부 코드를 호출하는 대신 먼저 간단한 테스트 케이스를 작성해 외부 코드를 익히는 방식으로 다르게 접근하면 어떨까? 짐 뉴커크(Jim Newkirk)는 이를 학습 테스트라 부른다.

- 학습 테스트는 프로그램에서 사용하려는 방식대로 외부 API를 호출한다. 통제된 환경에서 API를 제대로 이해하는지 확인하는 셈이다. 학습 테스트는 API를 사용하려는 목적에 초점을 맞춘다.

# 📚 9장 단위 테스트

## 테스트 하나에 하나의 개념을 작성하세요

- 테스트는 단일 책임 원칙을 따라야 합니다. 단위 테스트 하나당 하나의 assert 구문을 작성하세요.

**Bad:**

```jsx
import { assert } from "chai";

describe("AwesomeDate", () => {
  it("handles date boundaries", () => {
    let date: AwesomeDate;

    date = new AwesomeDate("1/1/2015");
    assert.equal("1/31/2015", date.addDays(30));

    date = new AwesomeDate("2/1/2016");
    assert.equal("2/29/2016", date.addDays(28));

    date = new AwesomeDate("2/1/2015");
    assert.equal("3/1/2015", date.addDays(28));
  });
});
```

**Good:**

```jsx
import { assert } from "chai";

describe("AwesomeDate", () => {
  it("handles 30-day months", () => {
    const date = new AwesomeDate("1/1/2015");
    assert.equal("1/31/2015", date.addDays(30));
  });

  it("handles leap year", () => {
    const date = new AwesomeDate("2/1/2016");
    assert.equal("2/29/2016", date.addDays(28));
  });

  it("handles non-leap year", () => {
    const date = new AwesomeDate("2/1/2015");
    assert.equal("3/1/2015", date.addDays(28));
  });
});
```

## 테스트의 이름은 테스트의 의도가 드러나야 합니다

- 테스트가 실패할 때, 테스트의 이름은 어떤 것이 잘못되었는지 볼 수 있는 첫 번째 표시입니다.

**Bad:**

```jsx
describe("Calendar", () => {
  it("2/29/2020", () => {
    // ...
  });

  it("throws", () => {
    // ...
  });
});
```

**Good:**

```jsx
describe("Calendar", () => {
  it("should handle leap year", () => {
    // ...
  });

  it("should throw when format is invalid", () => {
    // ...
  });
});
```
