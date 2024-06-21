---
title: "[Algorithm] 프렌즈4블록"
categories: [algorithm]
tags: [algorithm, programmers, Lv2, 2018 KAKAO BLIND RECRUITMENT, 1차]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 여덟 번째 포스팅

안녕하세요! 예순 여덟 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 프렌즈4블록**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 프렌즈4블록 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/17679)

## 💨 **문제 설명**

블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 "프렌즈4블록".

같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/415065a6-82d0-4183-9e47-c354b08c1623) <br>

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/a675846d-27f2-4f5b-82e4-bac72503a324) <br>

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/f22e1ac5-460f-4de1-bcb5-2ebc2d7a3b27) <br>

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/32258674-228c-495c-ac53-30db45849f74) <br>

위 초기 배치를 문자로 표시하면 아래와 같다.

```md
TTTANT
RRFACC
RRRFCC
TRRRAA
TTMMMF
TMMTTJ
```

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

## 💨 **제한 사항**

- 입력으로 판의 높이 `m`, 폭 `n`과 판의 배치 정보 `board`가 들어온다.
- 2 ≦ `n, m` ≦ 30
- `board`는 길이 `n`인 문자열 `m`개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

## 💨 **입출력 예**

|  m  |  n  |                            board                             | answer |
| :-: | :-: | :----------------------------------------------------------: | :----: |
|  4  |  5  |             ["CCBDE", "AAADE", "AAABF", "CCBBF"]             |   14   |
|  6  |  6  | ["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"] |   15   |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

입출력 예제 1의 경우, 첫 번째에는 A 블록 6개가 지워지고, 두 번째에는 B 블록 4개와 C 블록 4개가 지워져, 모두 14개의 블록이 지워진다.

**입출력 예 #2** <br>

입출력 예제 2는 본문 설명에 있는 그림을 옮긴 것이다. 11개와 4개의 블록이 차례로 지워지며, 모두 15개의 블록이 지워진다.

[**해설 보러가기**](https://tech.kakao.com/posts/344)

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. 주어진 배열 문자열을 2차원 배열로 변환함.
> 2. 2차원 배열을 순회하면서 2x2로 같은 블록이 있는지 확인함.
> 3. 같은 블록이 있으면 해당 블록을 제거하고, 제거된 블록을 기준으로 아래 블록을 내림.
> 4. 2x2로 같은 블록이 없을 때까지 반복함.
> 5. 제거된 블록의 수를 반환함.
>
> - removed는 제거된 블록이 있는지 확인하는 변수임.
> - removeArr은 제거할 블록을 표시하는 배열임. → 제거할 블록은 true로 표시함.

```js
function solution(m, n, board) {
  let answer = 0;
  let arr = board.map((b) => b.split(""));

  while (true) {
    let removeArr = Array.from({ length: m }, () => Array(n).fill(false));
    let removed = false;

    for (let i = 0; i < m - 1; i++) {
      for (let j = 0; j < n - 1; j++) {
        if (
          arr[i][j] !== "" &&
          arr[i][j] === arr[i][j + 1] &&
          arr[i][j] === arr[i + 1][j] &&
          arr[i][j] === arr[i + 1][j + 1]
        ) {
          removeArr[i][j] = true;
          removeArr[i][j + 1] = true;
          removeArr[i + 1][j] = true;
          removeArr[i + 1][j + 1] = true;
          removed = true;
        }
      }
    }

    if (!removed) break;

    for (let i = 0; i < m; i++) {
      for (let j = 0; j < n; j++) {
        if (removeArr[i][j]) {
          answer++;
          // 아래 포문은 제거된 블록을 기준으로 블록을 아래로 내리는 로직임.
          for (let k = i; k > 0; k--) {
            arr[k][j] = arr[k - 1][j];
          }
          // 제일 위에 있는 블록은 빈 문자열로 채워줌.
          arr[0][j] = "";
        }
      }
    }
  }

  return answer;
}

console.log(solution(4, 5, ["CCBDE", "AAADE", "AAABF", "CCBBF"])); // 14
console.log(
  solution(6, 6, ["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"])
); // 15
```

주어진 문제는 간단하게 말하자면 주어진 `m x n` 크기의 보드에서 `2x2` 크기의 동일한 블록이 만나면 해당 블록이 사라지고, 위의 블록들이 아래로 떨어지게 되는 것이다. 이를 반복하여 최종적으로 제거된 블록의 수를 반환하는 문제이다.

먼저 주어진 `board`를 2차원 배열로 변환한 후 시작한다. `removeArr`을 통해서 제거할 블록을 표시할 수 있다.

`2x2` 블록을 찾고 제거하는 과정을 반복하기 위해서 `while` 루프를 사용한다.

그 후, 이중 반복문을 통해 `2x2` 블록을 찾고 해당하는 경우에는 `removeArr`에서 해당 위치를 `true`로 변경한다.

`removed`가 `false`인 경우에는 더 이상 제거할 블록이 없다는 것으로 `while`루프를 종료할 수 있게 된다.

제거할 블록이 없다는 것을 확인하면 블록을 아래로 이동시켜야 한다. `removeArr`에서 `true`로 표시된 블록을 제거하고, 위에 있는 행의 블록을 아래의 행으로 이동시키고 맨 위의 블록은 빈 문자열로 채우면 된다.

위 코드가 조금 난해해 보여 함수로 나누어 리팩토링을 해보았다.

```js
function solution(m, n, board) {
  let answer = 0;
  let arr = board.map((b) => b.split(""));

  while (true) {
    let removeArr = getRemoveArray(m, n, arr);
    let removed = removeBlocks(m, n, arr, removeArr);
    if (!removed) break;
    answer += removed;
  }

  return answer;
}

function getRemoveArray(m, n, arr) {
  let removeArr = Array.from({ length: m }, () => Array(n).fill(false));

  for (let i = 0; i < m - 1; i++) {
    for (let j = 0; j < n - 1; j++) {
      if (
        arr[i][j] !== "" &&
        arr[i][j] === arr[i][j + 1] &&
        arr[i][j] === arr[i + 1][j] &&
        arr[i][j] === arr[i + 1][j + 1]
      ) {
        removeMarking(removeArr, i, j);
      }
    }
  }

  return removeArr;
}

function removeMarking(removeArr, i, j) {
  removeArr[i][j] = true;
  removeArr[i][j + 1] = true;
  removeArr[i + 1][j] = true;
  removeArr[i + 1][j + 1] = true;
}

function removeBlocks(m, n, arr, removeArr) {
  let removeCount = 0;

  for (let i = 0; i < m; i++) {
    for (let j = 0; j < n; j++) {
      if (removeArr[i][j]) {
        removeCount++;
        for (let k = i; k > 0; k--) {
          arr[k][j] = arr[k - 1][j];
        }
        arr[0][j] = "";
      }
    }
  }

  return removeCount;
}
```

위와 같이 나누게 되면 가독성이 더욱 좋고 코드를 한 눈에 파악할 수 있게 된다.

---

# 3️⃣ 느낀점

처음에 문제를 보고 한참동안 바라보기만 했다. `2x2`의 블록을 어떻게 처리하면 좋을 지 생각을 해보았다. 배열의 `현재 위치`, `우측`, `아래`, `대각선 아래`를 없애면 된다고 생각했고 터져야 할 블록들을 표시할 배열이 있어야 한다고 생각했다.

이것이 문제가 끝이 아니었다. 블록이 터지고 나서 행을 아래로 내려야 하는 로직을 작성해야 했다. 이중 반복문을 통해서 `삭제여부를 판단하는 배열`을 통해 `카운팅`을 해주고 내려주었다. 그리고 나서 터진 블록의 잔해를 `빈 문자열`로 바꾸어 주었다.

처음에 어떻게 해결할 지 한글로 적어두고 시작하는 것이 뭔가 수월한 것 같다. 그것을 통해서 코드화를 하면 되는 것 같다. ~~당연한 말 아님?;~~

**이상❗️**
