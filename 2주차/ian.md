### ๐ 2023๋ 1์ 25์ผ

# ๐ 4์ฅ ์ฃผ์

## ์ฃผ์์ ๋์ ์ฝ๋๋ฅผ ๋ณด์ํ์ง ๋ชปํ๋ค

- ์์ ์ด ์ ์ง๋ฅธ ๋์ฅํ์ ์ฃผ์์ผ๋ก ์ค๋ชํ๋ ค ์ ์ฐ๋ ๋์ ์ ๊ทธ ๋์ฅํ์ ๊นจ๋์ด ์น์ฐ๋ ๋ฐ ์๊ฐ์ ๋ณด๋ด๋ผ!

## ์ฝ๋๋ก ์๋๋ฅผ ํํํ๋ผ

**Bad:**

```jsx
// subscription์ด ํ์ฑํ ์ํ์ธ์ง ์ฒดํฌํฉ๋๋ค.
if (subscription.endDate > Date.now) {
}
```

**Good:**

```jsx
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) {
  /* ... */
}
```

๋ช ์ด๋ง ๋ ์๊ฐํ๋ฉด ์ฝ๋๋ก ๋๋ค์ ์๋๋ฅผ ํํํ  ์ ์๋ค. ๋ง์ ๊ฒฝ์ฐ ์ฃผ์์ผ๋ก ๋ฌ๋ ค๋ ์ค๋ช์ ํจ์๋ก ๋ง๋ค์ด ํํํด๋ ์ถฉ๋ถํ๋ค.

## ์ข์ ์ฃผ์

- ๊ฒฐ๊ณผ๋ฅผ ๊ฒฝ๊ณ ํ๋ ์ฃผ์

```jsx
// ์ฌ์  ์๊ฐ์ด ์ถฉ๋ถํ์ง ์๋ค๋ฉด ์คํํ์ง ๋ง์ญ์์ค.
public void _testWithReallyBigFile() {

}
```

- TODO ์ฃผ์
  - TODO๋ก ๋ก์น ํ ์ฝ๋๋ ๋ฐ๋์งํ์ง ์๋ค. ๊ทธ๋ฌ๋ฏ๋ก ์ฃผ๊ธฐ์ ์ผ๋ก TODO ์ฃผ์์ ์ ๊ฒํด ์์ ๋ ๊ด์ฐฎ์ ์ฃผ์์ ์์ ๋ผ๊ณ  ๊ถํ๋ค.

```tsx
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

- ์ค์์ฑ์ ๊ฐ์กฐํ๋ ์ฃผ์

```jsx
String listItemContent = match.group(3).trim();
// ์ฌ๊ธฐ์ trim์ ์ ๋ง ์ค์ํ๋ค. trim ํจ์๋ ๋ฌธ์์ด์์ ์์ ๊ณต๋ฐฑ์ ์ ๊ฑฐํ๋ค.
// ๋ฌธ์์ด์ ์์ ๊ณต๋ฐฑ์ด ์์ผ๋ฉด ๋ค๋ฅธ ๋ฌธ์์ด๋ก ์ธ์๋๊ธฐ ๋๋ฌธ์ด๋ค.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

## ๋์ ์ฃผ์

- ๋๋ถ๋ถ์ ์ฃผ์์ด ์ด ๊ณณ์ ์ํ๋ค.
- ์ฃผ์์ผ๋ก ์ฒ๋ฆฌํ ์ฝ๋
  - ์ฃผ์์ผ๋ก ์ฒ๋ฆฌํ ์ฝ๋๋งํผ ๋ฐ์ด์ค๋ฌ์ด ๊ดํ๋ ๋๋ฌผ๋ค.
  - ์ฃผ์์ผ๋ก ์ฒ๋ฆฌ๋ ์ฝ๋๋ ๋ค๋ฅธ ์ถ๋ค์ด ์ง์ฐ๊ธฐ๋ฅผ ์ฃผ์ ํ๋ค. ์ด์ ๊ฐ ์์ด ๋จ๊ฒจ๋์์ผ๋ฆฌ๋ผ๊ณ , ์ค์ํ๋๊น ์ง์ฐ๋ฉด ์ ๋๋ค๊ณ  ์๊ฐํ๋ค. ๊ทธ๋์ ์ง ๋์ ์์ธ๋ณ ๋ฐ๋ฅ์ ์๊ธ์ด ์์ด๋ฏ ์ธ๋ชจ ์๋ ์ฝ๋๊ฐ ์ ์ฐจ ์์ฌ๊ฐ๋ค.

# ๐ 5์ฅ ํ์ ๋ง์ถ๊ธฐ

## ํ์์ ๋ง์ถ๋ ๋ชฉ์ 

- ์ฝ๋์ ํ์์ ๋๋ฌด ์ค์ํ๋ค. ์ฝ๋์ ํ์์ ์์ฌ์ํต์ ์ผํ์ด๋ค.
- ์์ผ๋ก์ ๊ฐ๋์ฑ์ ์ง๋ํ ์ํฅ์ ๋ฏธ์น๊ฒ ๋๋ค.

## ๊ฐ๋์ ๋น ํ์ผ๋ก ๋ถ๋ฆฌํ๋ผ

```jsx
// ๋น ํ์ ๋ฃ์ง ์์ ๊ฒฝ์ฐ
package fitnesse.wikitext.widgets;
import java.util.regex.*;
public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
		Pattern.MULTILINE + Pattern.DOTALL);
	public BoldWidget(ParentWidget parent, String text) throws Exception {
		super(parent);
		Matcher match = pattern.matcher(text); match.find();
		addChildWidgets(match.group(1));}
	public String render() throws Exception {
		StringBuffer html = new StringBuffer("<b>");
		html.append(childHtml()).append("</b>");
		return html.toString();
	}
}
```

๋น ํ๋ง์ผ๋ก ํจ์ฌ ๊ฐ๋์ฑ ์ข์ ์ฝ๋๋ฅผ ๋ง๋ค ์ ์๋ค. ๋ํ ์ค๋ฐ๊ฟ์ ํตํด ๊ฐ๋์ ๋ถ๋ฆฌ๋ฅผ ํ  ์ ์๋ค.

```jsx
package fitnesse.wikitext.widgets;

import java.util.regex.*;

public class BoldWidget extends ParentWidget {
	public static final String REGEXP = "'''.+?'''";
	private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
		Pattern.MULTILINE + Pattern.DOTALL
	);

	public BoldWidget(ParentWidget parent, String text) throws Exception {
		super(parent);
		Matcher match = pattern.matcher(text);
		match.find();
		addChildWidgets(match.group(1));
	}

	public String render() throws Exception {
		StringBuffer html = new StringBuffer("<b>");
		html.append(childHtml()).append("</b>");
		return html.toString();
	}
}
```

## ์์ง ๊ฑฐ๋ฆฌ

- ์๋ก ๋ฐ์ ํ ๊ฐ๋์ ์ธ๋ก๋ก ๊ฐ๊น์ด ๋์ด์ผ ํ๋ค. ์ด ํจ์์์ ์  ํจ์๋ก ์ค๊ฐ๋ฉฐ ์์ค ํ์ผ์ ์ ์๋๋ก ๋บ๋บ์ด ๋๋ ๊ฒ์ ๊ฒฐ์ฝ ์ข์ ๊ฒฝํ์ด ์๋๋ค.
- ๋ฌผ๋ก  ๋ ๊ฐ๋์ด ์๋ก ๋ค๋ฅธ ํ์ผ์ ์ํ๋ค๋ฉด ๊ท์น์ด ํตํ์ง ์๋๋ค. ํ์ง๋ง ํ๋นํ ๊ทผ๊ฑฐ๊ฐ ์๋ค๋ฉด ์๋ก ๋ฐ์ ํ ๊ฐ๋์ ํ ํ์ผ์ ์ํด์ผ ๋ง๋ํ๋ค.
- ๋ณ์์ ์ธ์ ์ฌ์ฉํ๋ ์์น์์ ์ต๋ํ ๊ฐ๊น์ด ์ ์ธํ๋ค.

## ๊ฐ๋ก ํ์ ๋ง์ถ๊ธฐ

- ๊ฐ๋ก ํ์์ ํ๊ท ์ ์ผ๋ก 45์ ๊ทผ์ฒ์ด๋ค. ์ ์๋ 120์ ์ ๋๋ก ํ ๊ธธ์ด๋ฅผ ์ ํํ๋ค.

## ํ๊ท์น

- ํ๋ก๊ทธ๋๋จธ๋ผ๋ฉด ๊ฐ์ ์ ํธํ๋ ๊ท์น์ด ์๋ค. ํ์ง๋ง ํ์ ์ํ๋ค๋ฉด ์์ ์ด ์ ํธํด์ผ ํ  ๊ท์น์ ๋ฐ๋ก ํ ๊ท์น์ด๋ค.
- ๊ฐ๊ฐ์ธ์ด ๋ฐ๋ก๊ตญ๋ฐฅ์ฒ๋ผ ๋ง๋๋ก ์ง๋๋ ์ฝ๋๋ ํผํด์ผ ํ๋ค.
