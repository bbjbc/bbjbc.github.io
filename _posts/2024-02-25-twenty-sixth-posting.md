---
title: "[Algorithm] 2 x n 타일링"
categories: [algorithm]
tags: [algorithm, programmers, dp]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 스물 여섯 번째 포스팅

안녕하세요! 스물 여섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 2 x n 타일링**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 2 x n 타일링 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/12900)

## 💨 **문제 설명**

가로 길이가 2이고 세로의 길이가 1인 직사각형모양의 타일이 있습니다. 이 직사각형 타일을 이용하여 세로의 길이가 2이고 가로의 길이가 n인 바닥을 가득 채우려고 합니다. 타일을 채울 때는 다음과 같이 2가지 방법이 있습니다.

- 타일을 가로로 배치 하는 경우
- 타일을 세로로 배치 하는 경우

예를들어서 n이 7인 직사각형은 다음과 같이 채울 수 있습니다. <br>
![타일링](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/8398acc2-4728-4a83-bd89-a418d0fca944)<br>

직사각형의 가로의 길이 n이 매개변수로 주어질 때, 이 직사각형을 채우는 방법의 수를 return 하는 solution 함수를 완성해주세요.

## 💨 **제한사항**

- 가로의 길이 n은 60,000이하의 자연수 입니다.
- 경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return해주세요.

## 💨 **입출력 예**

|  n  | result |
| :-: | :----: |
|  4  |   5    |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

- 다음과 같이 5가지 방법이 있다.

![타일링1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/4f4a197d-ff0c-49f9-b091-f75202aa8f31)<br>

---

# 2️⃣ 문제 풀이

## 🔥 문제 풀이

> 1. dp 배열을 -1로 채워준다.
> 2. base case 설정한다.
> 3. dp를 사용하여 경우의 수를 구한다. (경우의 수가 많아질 수 있으므로, 문제 요구 사항 대로 1,000,000,007로 나눈 나머지를 저장함)
> 4. n번째 return

```java
function solution(n) {
  const dp = new Array(n + 1).fill(-1);
  dp[1] = 1;
  dp[2] = 2;
  for (let i = 3; i <= n; i++) {
    dp[i] = (dp[i - 1] + dp[i - 2]) % 1000000007;
  }
  return dp[n];
}
```

base case가 주어지거나 쉽게 구할 수 있고, 경우의 수를 구하는 문제이면 dp를 사용하면 되는 듯하다.

<br>

## 🔥 의아했던 부분

```java
const dp = new Array(n + 1).fill(-1);
```

이 부분을 처음엔 `Infinity`로 채워 보았다. 이러면 무한대 값으로 배열이 초기화되는데 이후 계산이 올바르게 이루어지지 않을 수 있다고 한다.

또한, `0`으로도 채워 보았다. 값을 계산하지 않고 바로 0으로 채워지기 때문에 이후 계산이 이루어지지 않아 올바른 결과를 얻을 수 없다고 하며 시간 초과의 원인이라고 한다.

`-1`로 채우는 것은 아직 값을 계산하지 않았음을 나타낸다. 그러므로 이후 값을 계산하여 배열에 저장이 가능하다. dp 배열 초기화는 보통 `-1`로 해주는 것이 자연스러우며 안전한 선택일 것이다.

---

# 3️⃣ 느낀점

이 문제는 테스트 케이스를 통과하기는 쉬웠지만 효율성 측면에서 다들 애를 먹고 있는 모양이다. 어떤 사람은 console.log()를 포함하니 통과 되었다고 하며 clg문이 성능 측면에서 우수한 지 질문하는 분도 계셨다. 효율성 측면에서 통과가 어렵고 까다로운 문제였던 것 같다. <br>
물론 나도 배열의 초기값을 이상하게 채워서 시간 초과가 발생하긴 했지만 이것 저것 넣어보니 운이 좋게 통과된 듯 하다. <br>

실제로 찾아보니 `console.log()`를 남용하면 성능에 부정적인 영향을 미칠 수 있다고 한다. 출력 오버헤드, 출력 버퍼링, 불필요 정보 노출 등으로 부정적인 영향을 끼친다고 한다. 디버깅이나 개발 중에만 필요한 경우 사용하라고 하는데 clg문을 포함한 분이 통과가 어떻게 됐는 지는 정확히 모르겠다. ~~운빨 문제였나? 잘 모르겠다.~~ **그럼 안녕🌈**
