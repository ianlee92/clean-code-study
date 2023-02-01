### 📅 2023년 2월 1일

# 📚 6장 객체와 자료 구조

## 디미터 법칙

- 디미터 법칙: 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙

간단히 말해서 하나의 코드 Line에는 점(.) 하나만 찍으라는 말.

```java
// 기차 충돌
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

ctxt 객체가 Options 포함하고, Options가 ScratchDir을 포함하고, ScratchDir가 AbsolutePath를 포함하고 있다는 것을 외부로 공개한 셈.

이런 구조를 잡종 구조 라고 한다.

- 잡종 구조: 절반은 객체, 절반은 자료 구조인 것. 마치 비공개(private)로 설정해놓고 그대로 노출하는(public) 것과 다른 것이 없는 셈

객체의 데이터를 조회(Get)하는 것이 아니라 객체에게 명령을 내리면 감출 수 있다.

```java
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

# 📚 10장 클래스

## 클래스는 작아야 한다

클래스는 만들 때 최대한 작아야 한다.

즉, 클래스가 맡은 책임을 적게 가져가라 라는 의미.

단일책임원칙(SRP)를 지키자!

## 응집도

클래스의 인스턴스 변수 수가 작아야 한다.

클래스에서 메서드가 인스턴스 변수를 많이 사용할 수록 응집도가 높은 설계라 볼 수 있다.

응집도가 높다는 말은 클래스에 속한 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미.

```java
public class Stack {
  private int topOfStack = 0;
  List<Integer> elements = new LinkedList<>();

  public int size() {
    return topOfStack;
  }

  public void push(int element) {
    topOfStack++;
    elements.add(element);
  }

  public int pop() throws PoppedWhenEmpty {
    if (topOfStack == 0) {
      throw new PoppedWhenEmpty();
    }

    int element = elements.get(--topOfStack);
    elements.remove(topOfStack);
    return element;
  }
}
```

위의 예제와 같은 클래스는 모든 메서드에 모든 인스턴스 변수가 사용되고 있는 아주 바람직한 응집도 높은 클래스라 볼 수 있다.

응집도가 높은 클래스를 설계하기 위해서는 다음과 같은 원칙을 지키면 된다.

- 함수를 작게, 매개변수 목록을 짧게
- SRP를 준수하기 위해 리팩토링을 하자

## 변경하기 쉬운 클래스

- OCP를 지키기 위해서 쪼개고 또 쪼개야 한다

## 변경으로부터 격리

- DIP 추상화는 상세한 구현이 아니라 추상화에 의존해야 한다는 원칙
