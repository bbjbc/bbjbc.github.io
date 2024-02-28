---
title: "[Algorithm] 키패드 누르기"
categories: [algorithm]
tags: [algorithm, programmers, kakao]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 서른 번째 포스팅

안녕하세요! 서른 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 서른~

오늘의 포스팅 내용은 **프로그래머스 - 키패드 누르기**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 키패드 누르기 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/67256)

## 💨 **문제 설명**

스마트폰 전화 키패드의 각 칸에 다음과 같이 숫자들이 적혀 있습니다. <br>

![kakao_phone](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/f7e6e4c9-36e9-4bc6-a0ee-1b2c0663b488) <br>

이 전화 키패드에서 왼손과 오른손의 엄지손가락만을 이용해서 숫자만을 입력하려고 합니다.
맨 처음 왼손 엄지손가락은 `*` 키패드에 오른손 엄지손가락은 `#` 키패드 위치에서 시작하며, 엄지손가락을 사용하는 규칙은 다음과 같습니다.

1. 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.
2. 왼쪽 열의 3개의 숫자 `1`, `4`, `7`을 입력할 때는 왼손 엄지손가락을 사용합니다.
3. 오른쪽 열의 3개의 숫자 `3`, `6`, `9`를 입력할 때는 오른손 엄지손가락을 사용합니다.
4. 가운데 열의 4개의 숫자 `2`, `5`, `8`, `0`을 입력할 때는 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용합니다.
   - 4-1. 만약 두 엄지손가락의 거리가 같다면, 오른손잡이는 오른손 엄지손가락, 왼손잡이는 왼손 엄지손가락을 사용합니다.

순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.

## 💨 **제한사항**

- numbers 배열의 크기는 1 이상 1,000 이하입니다.
- numbers 배열 원소의 값은 0 이상 9 이하인 정수입니다.
- hand는 `"left"` 또는 `"right"` 입니다.
  - `"left"`는 왼손잡이, `"right"`는 오른손잡이를 의미합니다.
- 왼손 엄지손가락을 사용한 경우는 `L`, 오른손 엄지손가락을 사용한 경우는 `R`을 순서대로 이어붙여 문자열 형태로 return 해주세요.

## 💨 **입출력 예**

|              numbers              |  hand   |    result     |
| :-------------------------------: | :-----: | :-----------: |
| [1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5] | "right" | "LRLLLRLLRRL" |
| [7, 0, 8, 2, 8, 3, 1, 5, 7, 6, 2] | "left"  | "LRLLRRLLLRR" |
|  [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]   | "right" | "LLRLLRLLRL"  |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>
순서대로 눌러야 할 번호가 [1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5]이고, 오른손잡이입니다.

| 왼손 위치 | 오른손 위치 | 눌러야 할 숫자 | 사용한 손 |                               설명                               |
| :-------: | :---------: | :------------: | :-------: | :--------------------------------------------------------------: |
|    \*     |      #      |       1        |     L     |                      1은 왼손으로 누릅니다.                      |
|     1     |      #      |       3        |     R     |                     3은 오른손으로 누릅니다.                     |
|     1     |      3      |       4        |     L     |                      4는 왼손으로 누릅니다.                      |
|     4     |      3      |       5        |     L     |   왼손 거리는 1, 오른손 거리는 2이므로 왼손으로 5를 누릅니다.    |
|     5     |      3      |       8        |     L     |   왼손 거리는 1, 오른손 거리는 3이므로 왼손으로 8을 누릅니다.    |
|     8     |      3      |       2        |     R     |  왼손 거리는 2, 오른손 거리는 1이므로 오른손으로 2를 누릅니다.   |
|     8     |      2      |       1        |     L     |                      1은 왼손으로 누릅니다.                      |
|     1     |      2      |       4        |     L     |                      4는 왼손으로 누릅니다.                      |
|     4     |      2      |       5        |     R     | 왼손 거리와 오른손 거리가 1로 같으므로, 오른손으로 5를 누릅니다. |
|     4     |      5      |       9        |     R     |                     9는 오른손으로 누릅니다.                     |
|     4     |      9      |       5        |     L     |   왼손 거리는 1, 오른손 거리는 2이므로 왼손으로 5를 누릅니다.    |
|     5     |      9      |       -        |     -     |                                                                  |

따라서 `"LRLLLRLLRRL"`를 return 합니다.

**입출력 예 #2** <br>
왼손잡이가 [7, 0, 8, 2, 8, 3, 1, 5, 7, 6, 2]를 순서대로 누르면 사용한 손은 `"LRLLRRLLLRR"`이 됩니다.

**입출력 예 #3** <br>
오른손잡이가 [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]를 순서대로 누르면 사용한 손은 `"LLRLLRLLRL"`이 됩니다.

---

# 2️⃣ 문제 풀이

## 🔥 문제 풀이

> 1. 왼손과 오른손의 초기 위치 값과, 키패드를 보고 좌표로 설정한다.
> 2. 가운데 열 터치를 위한 거리 계산 함수를 만든다.
> 3. 주어진 numbers 배열을 순회하며 왼쪽 열, 오른쪽 열을 나누어 방향 문자열을 배열에 삽입.
> 4. 가운데 열에서는 거리에 따라 방향 문자열을 계산함.
> 5. 오른쪽과의 거리가 클 경우, 왼쪽과의 거리가 클 경우, 거리가 같을 경우로 나누어 작업.
> 6. 삽입된 배열을 문자열로 변환 후 return

```js
function solution(numbers, hand) {
  const result = [];
  let leftHandPosition = [3, 0];
  let rightHandPosition = [3, 2];
  const keypad = {
    1: [0, 0],
    2: [0, 1],
    3: [0, 2],
    4: [1, 0],
    5: [1, 1],
    6: [1, 2],
    7: [2, 0],
    8: [2, 1],
    9: [2, 2],
    0: [3, 1],
  };

  const getDistance = (a, b) => {
    return Math.abs(a[1] - b[1]) + Math.abs(a[0] - b[0]);
  };

  numbers.forEach((number) => {
    if (number === 1 || number === 4 || number === 7) {
      result.push("L");
      leftHandPosition = keypad[number];
    } else if (number === 3 || number === 6 || number === 9) {
      result.push("R");
      rightHandPosition = keypad[number];
    } else {
      const leftGetDistance = getDistance(keypad[number], leftHandPosition);
      const rightGetDistance = getDistance(keypad[number], rightHandPosition);

      if (leftGetDistance < rightGetDistance) {
        result.push("L");
        leftHandPosition = keypad[number];
      } else if (leftGetDistance > rightGetDistance) {
        result.push("R");
        rightHandPosition = keypad[number];
      } else {
        if (hand === "right") {
          result.push("R");
          rightHandPosition = keypad[number];
        } else {
          result.push("L");
          leftHandPosition = keypad[number];
        }
      }
    }
  });

  return result.join("");
}
```

키패드 배열을 올바르게 설정하고, 거리 계산을 제대로 해야하는 것 같다.

처음에는 아래와 같이 풀었다.

```js
function solution(numbers, hand) {
  const result = [];
  let leftHandPosition = [3, 0];
  let rightHandPosition = [3, 2];

  const getDistance = (number, currentHandPosition) => {
    const keypad = [
      [3, 1],
      [0, 0],
      [0, 1],
      [0, 2],
      [1, 0],
      [1, 1],
      [1, 2],
      [2, 0],
      [2, 1],
      [2, 2],
    ];
    const currentNumber = keypad[number];
    return (
      Math.abs(currentNumber[1] - currentHandPosition[1]) +
      Math.abs(currentNumber[0] - currentHandPosition[0])
    );
  };

  numbers.forEach((number) => {
    if (number === 1 || number === 4 || number === 7) {
      result.push("L");
      leftHandPosition = [Math.floor(number / 3), 0];
    } else if (number === 3 || number === 6 || number === 9) {
      result.push("R");
      rightHandPosition = [Math.floor(number / 3) - 1, 2];
    } else {
      const leftGetDistance = getDistance(number, leftHandPosition);
      const rightGetDistance = getDistance(number, rightHandPosition);

      if (leftGetDistance < rightGetDistance) {
        result.push("L");
        leftHandPosition = [Math.floor(number / 3), 1];
      } else if (leftGetDistance > rightGetDistance) {
        result.push("R");
        rightHandPosition = [Math.floor(number / 3), 1];
      } else {
        if (hand === "right") {
          result.push("R");
          rightHandPosition = [Math.floor(number / 3), 1];
        } else {
          result.push("L");
          leftHandPosition = [Math.floor(number / 3), 1];
        }
      }
    }
  });
  return result.join("");
}
```

왜 통과가 안 되는지는 모르겠지만 확실히, 위처럼 키패드의 좌표를 기준으로 거리를 계산하고 직관적이며 가독성도 뛰어난 것 같다.

---

# 3️⃣ 느낀점

일단 휴대폰 키패드 모양대로 <br>

```js
// 키패드 배열
// [0,0] [0,1] [0,2]
// [1,0] [1,1] [1,2]
// [2,0] [2,1] [2,2]
// [3,0] [3,1] [3,2]
// 현재 왼손의 위치 - [3,0]
// 현재 오른손의 위치 - [3,2]
```

배열을 설정하는 것이 중요한 것 같다. <br>
또한, 거리 계산할 때 점과 점 사이의 거리 공식으로 계산하는 것이 아닌, [**[맨해튼 거리 공식]**](https://ko.wikipedia.org/wiki/%EB%A7%A8%ED%95%B4%ED%8A%BC_%EA%B1%B0%EB%A6%AC) 으로 거리를 계산해야 한다. 키패드 사이의 길이가 1이며 대각선의 길이는 존재하지 않기 때문에 이 공식을 사용하는 것이 중요하다. <br>
키패드 배열과 거리 계산만 잘 써주면 무난하게 풀 수 있는 문제였다. ~~하지만, 그것을 생각하는 데 오래걸림핑;;~~ 그리고 아래 방법에서 문제점을 찾느라 한참 걸린 것 같다. 왜 안된 건지는 모르겠다. 혹시라도 이 글을 본 독자가 있다면 필자의 2번 풀이가 왜 안풀린 지 댓글로 달아주면 감사할 것이다. **그럼 안녕핑🐌**
