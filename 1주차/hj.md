## 1장 깨끗한 코드

- 나쁜 코드가 쌓일수록 팀 생산성은 떨어진다.

- 비야네 스트롭스트룹
  - 우아하고 효율적인 코드를 좋아한다.
    보는 사람에게 즐거움을 선사해야 한다.
- 론 제프리스

  - 모든 테스트 케이스를 통과한다.
  - 중복이 없다.
  - 시스템 내 모든 설계 아이디어를 표현한다.
  - 클래스, 메서드, 함수 등을 최대한 줄인다.

  > 중복을 피해라, 한 기능만 수행하라, 제대로 표현하라 작게 추상화하라. 이상이다.

- 워드 커닝햄

  - 읽으면서 놀랄 일 없는 코드

- 코드 읽는 시간 대 코드를 짜는 시간 비율이 10대 1을 훌쩍 넘는다. 새 코드를 짜면서 우리는 끊임없이 기존 코드를 읽는다.

- 보이스 카우트 규칙 : 캠프장은 처음 왔을 때보다 더 깨끗하게 하고 떠나라.

❤️‍🔥연습해 연습!❤️‍🔥

<br />

## 2장 의미 있는 이름

- 의도를 분명히 밝혀라 _변수의 존재 이유는? 수행 기능은? 사용 방법은?_

  - 나쁜예시

    코드가 하는 일을 짐작하기 어려움, 코드의 함축성이 문제다.
    코드의 맥락이 명시적으로 드러나지 않는다.

    ```javascript
    function getThem() {
      const list1 = [];
      for (const x of theList) {
        if (x[0] === 4) {
          list1.push(x);
        }

        return list1;
      }
    }
    ```

  - 조금 바꿔볼까? 지뢰찾기라고 생각해보자.

    코드가 하는일이 명확해졌다.

    ```javascript
    function getFlaggedCells() {
      const flaggedCells = [];
      for (const cell of gameBoard) {
        if (cell[STATUS_VALUE] === FLAGGED) {
          flaggedCells.push(cell);
        }

        return flaggedCells;
      }
    }
    ```

  - 마지막으로, 상수를 감추고 `isFlagged`라는 명시적인 함수를 만들어보자.

    ```javascript
    function getFlaggedCells() {
      const flaggedCells = [];
      for (const cell of gameBoard) {
        if (cell.isFlagged()) {
          flaggedCells.push(cell);
        }

        return flaggedCells;
      }
    }
    ```

    단순히 이름만 고쳤는데 한수가 하는 일을 이해하기 쉬워졌다.

- 그릇된 정보를 피하라

  - 널리 쓰이는 의미가 있는 단어를 다른 의미 쓰기 금지
  - 흠사한 이름을 사용 금지
  - 일관성이 떨어지는 표기법 금지

- 발음하기 쉬운 이름을 사용하라.

  - 발음하기 어려운 이름은 토론하기 어렵다.
  - `genymdhms`이게 어떻게 발음할까용?

    ```typescript
    // 띠용또잉
    type DtaRcrd102 = {
      genymdhms: Date;
      modymdhms: Date;
      pszqint: number;
    };
    //good~ 이제 발음할 수 있겠다!!
    type Customer = {
      generationTimestamp: Date;
      modificationTimestamp: Date;
      recordId: number;
    };
    ```

- 검색하기 쉬운 이름을 사용하라.

  ```javascript
  //여기서 어떤 수를 찾으려고 한다면...?😨
   for (let j=0; j<34; j++) {
     const s += (t[j] * 4)/5;
   }

   //상수화 해놓으면 찾기쉽다!
   const WORK_DAYS_PER_WEEK = 5;
  ```

- 자신의 기억력을 자랑하지 마라. 명료함이 최고다.
- 한 개념에 한 단어를 사용하라.
  - `~controller`, `~manager`, `~driver` 다 같은 개념 아닌가?
- 해법 영역에서 가져온 이름을 사용하라 : 읽은 사람도 개발자다
- 문제 영역에서 가져온 이름을 사용하라 : 프로그래머 용어가 없으면 문제 영역 이름을 가져온다.
- 의미있는 맥락을 추가하라.
- 불필요한 맥락을 없애라.
- 고금 휘발유 충전소(GSD)를 만든다. 모든 모듈 이름이 GDS~ 형식으로 만들어진다면?😨
- 일반적으로 찖은 이름이 긴 이름보다 좋다. 단, 의미가 분명한 경우

## 3장 함수

- 작게 만들어라

  - 함수는 읽고 이해하기 쉬운 정도로 짧게 작성해야한다.
  - 조건문은 간단하게

- 한 가지만 해라
  - 지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행하면 한 가지일만 한다.
- 함수 당 추상화 수준은 하나로

  - 함수 내 추상화 수준을 섞으면 읽는 사람이 헷갈린다.
  - 개념과 세부사항이 섞이면 함수에 세부 사항이 추가된다.
  - 내려가기 규칙 : 한 함수 다음에는 추상화 수준이 한 단계 낮은 함수가 온다.

- 서술적인 이름을 사용하라.

  - 함수가 하는 일을 잘 표현할 수 있는 이름을 사용한다.
    ex, `testTableHtml` -> `setUpTeardownIncluderRender`
  - 코드 내에서 일관성있게 작성한다.

- 함수 인수

  - 함수에서 가장 이상적인 인수 개수는 0(무항) -> 1(단항) -> 2(이항)이다.
  - 삼 항은 가능한 피하는 편이 좋다.
  - 인수가 2-3개 필요하다면 객체 리터럴 사용을 고려해본다.변수를 묶어서 넘기려면 이름을 뭍여야하고 결국 개념을 표현하게 된다.
  - 함수 이름에 인수 키워드를 넣어 의도를 표현한다.

- 부수 효과를 일으키지 마라
- 명령과 조회를 분리하라!
  - 함수는 수행하거나 뭔가에 단하거나 둘 중 하나만 한다.
- 오류 코드보다 예외를 사용하라
- 반복하지 마라
  - 중복은 소프트웨어에서 모든 악의 근원이다. 중복되는 부분중 어느 한곳이라도 빠트리면 오류가 발생할 확률이 배가 된다.
- 구조적 프로그래밍
  - 모든 함수와 함수 내 모든 블록에 입구가 출구는 하나만 존재해야한다.
  - 하지만 이 원칙은 함수가 작으면 별로 이익을 제공하지 못한다.
