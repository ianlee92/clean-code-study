### ๐ 2023๋ 2์ 22์ผ

# ๐ 7์ฅ ์ค๋ฅ ์ฒ๋ฆฌ

- ํด๋ฆฐ ์ฝ๋๋ ์ฝ๊ธฐ๋ ์ข์์ผ ํ์ง๋ง ์์ ์ฑ๋ ๋์์ผ ํ๋ค.

- ์์ธ์ฒ๋ฆฌ๋ฅผ ๋น์ฆ๋์ค ๋ก์ง๊ณผ ๋ถ๋ฆฌํด ๋์์ ์ธ ์ฌ์์ผ๋ก ๊ณ ๋ คํ๋ฉด ํผํผํ๊ณ  ๊นจ๋ํ ์ฝ๋๋ฅผ ์์ฑํ  ์ ์๋ค.

## null์ ์ ๋ฌํ์ง ๋ง๋ผ

**Bad:**

```java
public class MetricsCalculator
{
 public double xProjection(Point p1, Point p2) {
 return (p2.x โ p1.x) * 1.5;
 }
 โฆ
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
  return (p2.x โ p1.x) * 1.5;
 }
}
```

# ๐ 8์ฅ ๊ฒฝ๊ณ

- ์์คํ์ ๋ค์ด๊ฐ๋ ๋ชจ๋  ์ํํธ์จ์ด๋ฅผ ์ง์  ๊ฐ๋ฐํ๋ ๊ฒฝ์ฐ๋ ๋๋ฌผ๋ค. ํจํค์ง๋ฅผ ์ฌ๊ฑฐ๋, ์คํ ์์ค๋ฅผ ์ด์ฉํ๊ฑฐ๋, ์ฌ๋ด ๋ค๋ฅธ ํ์ด ์ ๊ณตํ๋ ์ธ๋ถ ์ฝ๋๋ฅผ ์ฐ๋ฆฌ ์ฝ๋์ ๊น๋ํ๊ฒ ํตํฉํด์ผ๋ง ํ๋ค.

## ๊ฒฝ๊ณ ์ดํผ๊ณ  ์ตํ๊ธฐ

- ์ฌ์ฉ๋ฒ์ด ๋ถ๋ช์น ์์ ํ์ฌ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๊ฐ์ ธ์๋ค๊ณ  ๊ฐ์ ํ์. ํ๋ฃจ๋ ์ดํ๊ฐ ๋ฌธ์๋ฅผ ์ฝ์ผ๋ฉฐ ์ฌ์ฉ๋ฒ์ ๊ฒฐ์ ํ๊ณ , ์ฐ๋ฆฌ ์ชฝ ์ฝ๋๋ฅผ ์์ฑํด ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ์์๋๋ก ๋์ํ๋์ง ํ์ธํ๋ค. ๋๋ก๋ ๋ฒ๊ทธ๊ฐ ์์ฑํ ์ฝ๋์์ ๋ฐ์ํ ๊ฒ์ธ์ง, ๋ผ์ด๋ธ๋ฌ๋ฆฌ์์ ๋ฐ์ํ ๊ฒ์ธ์ง ์ฐพ์๋ด๋๋ผ ์ค๋ ์๊ฐ ๊ณจ๋จธ๋ฆฌ๋ฅผ ์๊ธฐ๋ ํ๋ค.

- ์ธ๋ถ ์ฝ๋๋ฅผ ์ตํ๊ธด ์ด๋ ต๋ค. ์ธ๋ถ ์ฝ๋๋ฅผ ํตํฉํ๊ธฐ๋ ์ด๋ ต๋ค. ๋ ๊ฐ์ง๋ฅผ ๋์์ ํ๋ ๊ฑด ๋ฐฐ๋ก ์ด๋ ต๋ค. ์ฐ๋ฆฌ ์ชฝ ์ฝ๋๋ฅผ ์์ฑํด ์ธ๋ถ ์ฝ๋๋ฅผ ํธ์ถํ๋ ๋์  ๋จผ์  ๊ฐ๋จํ ํ์คํธ ์ผ์ด์ค๋ฅผ ์์ฑํด ์ธ๋ถ ์ฝ๋๋ฅผ ์ตํ๋ ๋ฐฉ์์ผ๋ก ๋ค๋ฅด๊ฒ ์ ๊ทผํ๋ฉด ์ด๋จ๊น? ์ง ๋ด์ปคํฌ(Jim Newkirk)๋ ์ด๋ฅผ ํ์ต ํ์คํธ๋ผ ๋ถ๋ฅธ๋ค.

- ํ์ต ํ์คํธ๋ ํ๋ก๊ทธ๋จ์์ ์ฌ์ฉํ๋ ค๋ ๋ฐฉ์๋๋ก ์ธ๋ถ API๋ฅผ ํธ์ถํ๋ค. ํต์ ๋ ํ๊ฒฝ์์ API๋ฅผ ์ ๋๋ก ์ดํดํ๋์ง ํ์ธํ๋ ์์ด๋ค. ํ์ต ํ์คํธ๋ API๋ฅผ ์ฌ์ฉํ๋ ค๋ ๋ชฉ์ ์ ์ด์ ์ ๋ง์ถ๋ค.

# ๐ 9์ฅ ๋จ์ ํ์คํธ

## ํ์คํธ ํ๋์ ํ๋์ ๊ฐ๋์ ์์ฑํ์ธ์

- ํ์คํธ๋ ๋จ์ผ ์ฑ์ ์์น์ ๋ฐ๋ผ์ผ ํฉ๋๋ค. ๋จ์ ํ์คํธ ํ๋๋น ํ๋์ assert ๊ตฌ๋ฌธ์ ์์ฑํ์ธ์.

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

## ํ์คํธ์ ์ด๋ฆ์ ํ์คํธ์ ์๋๊ฐ ๋๋ฌ๋์ผ ํฉ๋๋ค

- ํ์คํธ๊ฐ ์คํจํ  ๋, ํ์คํธ์ ์ด๋ฆ์ ์ด๋ค ๊ฒ์ด ์๋ชป๋์๋์ง ๋ณผ ์ ์๋ ์ฒซ ๋ฒ์งธ ํ์์๋๋ค.

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
