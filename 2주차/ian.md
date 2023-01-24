### 📅 2023년 1월 18일

# 📚 4장 주석

## 주석은 나쁜 코드를 보완하지 못한다

- 자신이 저지른 난장판을 주석으로 설명하려 애쓰는 대신에 그 난장판을 깨끗이 치우는 데 시간을 보내라!

## 코드로 의도를 표현하라

**Bad:**

```jsx
// subscription이 활성화 상태인지 체크합니다.
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

몇 초만 더 생각하면 코드로 대다수 의도를 표현할 수 있다. 많은 경우 주석으로 달려는 설명을 함수로 만들어 표현해도 충분하다.

## 좋은 주석

- 결과를 경고하는 주석

```jsx
// 여유 시간이 충분하지 않다면 실행하지 마십시오.
public void _testWithReallyBigFile() {

}
```

- TODO 주석
  - TODO로 떡칠한 코드는 바람직하지 않다. 그러므로 주기적으로 TODO 주석을 점검해 없애도 괜찮은 주석은 없애라고 권한다.

```tsx
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

- 중요성을 강조하는 주석

```jsx
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

## 나쁜 주석

- 대부분의 주석이 이 곳에 속한다.
- 주석으로 처리한 코드
  - 주석으로 처리한 코드만큼 밉살스러운 관행도 드물다.
  - 주석으로 처리된 코드는 다른 삶들이 지우기를 주저한다. 이유가 있어 남겨놓았으리라고, 중요하니까 지우면 안 된다고 생각한다. 그래서 질 나쁜 와인병 바닥에 앙금이 쌓이듯 쓸모 없는 코드가 점차 쌓여간다.

# 📚 5장 형식 맞추기

## 형식을 맞추는 목적

- 코드의 형식은 너무 중요하다. 코드의 형식은 의사소통의 일환이다.
- 앞으로의 가독성에 지대한 영향을 미치게 된다.

## 개념은 빈 행으로 분리하라

```jsx
// 빈 행을 넣지 않을 경우
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

빈 행만으로 훨씬 가독성 좋은 코드를 만들 수 있다. 또한 줄바꿈을 통해 개념의 분리를 할 수 있다.

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

## 수직 거리

- 서로 밀접한 개념은 세로로 가까이 두어야 한다. 이 함수에서 저 함수로 오가며 소스 파일을 위 아래로 뺑뺑이 도는 것은 결코 좋은 경험이 아니다.
- 물론 두 개념이 서로 다른 파일에 속한다면 규칙이 통하지 않는다. 하지만 타당한 근거가 없다면 서로 밀접한 개념은 한 파일에 속해야 마땅하다.
- 변수선언은 사용하는 위치에서 최대한 가까이 선언한다.

## 가로 형식 맞추기

- 가로 형식은 평균적으로 45자 근처이다. 저자는 120자 정도로 행 길이를 제한한다.

## 팀규칙

- 프로그래머라면 각자 선호하는 규칙이 있다. 하지만 팀에 속한다면 자신이 선호해야 할 규칙은 바로 팀 규칙이다.
- 개개인이 따로국밥처럼 맘대로 짜대는 코드는 피해야 한다.
