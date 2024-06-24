---
title: "[Baekjoon] 1105.팔"
categories: [baekjoon, algorithm]
tags: [algorithm, baekjoon, silver1, 수학, 그리디 알고리즘]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 일흔 한 번째 포스팅

안녕하세요! 일흔 한 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **백준 - 팔**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[백준] 팔 (문제 링크)**](https://www.acmicpc.net/problem/1105)

## 💨 **문제 설명**

`L`과 `R`이 주어진다. 이때, `L`보다 크거나 같고, `R`보다 작거나 같은 자연수 중에 8이 가장 적게 들어있는 수에 들어있는 8의 개수를 구하는 프로그램을 작성하시오.

## 💨 **입력**

첫째 줄에 `L`과 `R`이 주어진다. `L`은 2,000,000,000보다 작거나 같은 자연수이고, `R`은 `L`보다 크거나 같고, 2,000,000,000보다 작거나 같은 자연수이다.

## 💨 **출력**

첫째 줄에 `L`보다 크거나 같고, `R`보다 작거나 같은 자연수 중에 8이 가장 적게 들어있는 수에 들어있는 8의 개수를 구하는 프로그램을 작성하시오.

## 💨 **입출력 예**

**※ 참고로 변수 설정 및 입력값 설정은 해결 방법에 따라 달라질 수 있음. 아래와 같이 배열 및 변수명은 본인이 커스텀한 것임.**

|  L   |  R   | return |
| :--: | :--: | :----: |
|  1   |  10  |   0    |
|  88  |  88  |   2    |
| 800  | 899  |   1    |
| 8808 | 8880 |   2    |

## 💨 **알고리즘 분류**

- [수학](https://www.acmicpc.net/problemset?sort=ac_desc&algo=124)
- [그리디 알고리즘](https://www.acmicpc.net/problemset?sort=ac_desc&algo=33)

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. 자리수별로 8의 개수를 세는 함수를 제작
> 2. L부터 R까지 반복문을 돌면서 8의 개수를 세고, 최소 개수를 찾아 반환
> 3. 최소 개수가 0이면 반복문을 종료

```js
function solution(L, R) {
  function countEights(num) {
    let count = 0;
    while (num > 0) {
      if (num % 10 === 8) count++;
      num = Math.floor(num / 10);
    }

    return count;
  }

  let minCount = Infinity;

  for (let num = L; num <= R; num++) {
    let eightsCounting = countEights(num);

    if (minCount > eightsCounting) {
      minCount = eightsCounting;
    }
  }

  return minCount;
}

const fs = require("fs");
const [L, R] = fs
  .readFileSync("example.txt")
  .toString()
  .trim()
  .split(" ")
  .map(Number);

const result = solution(L, R);
console.log(result);
```

주어진 문제는 간단하지만 왜 `실버1`에 위치해 있는지는 모르겠다.

`L`, `R` 범위 안에 있는 숫자들 중 각 자릿수에서 숫자 `8`의 개수를 세는 함수를 만든다.

주어진 범위 내에서 숫자를 순회하면서 각 숫자의 `8`의 개수를 구한다. `minCount`와 비교하여 작은 값으로 갱신을 해준 후 반복문이 종료되면 최종적으로 `minCount`를 반환해주면 된다.

근데 제출했는데 시원시원하게 채점된 것은 아니다. 시간이 조금 걸렸지만 통과가 되었다!

---

# 3️⃣ 느낀점

백준에서의 자바스크립트는 정말 생소한 것 같다. 이 문제 말고 [**다음과 같은 문제**(**31287.장난감 강아지**)](https://www.acmicpc.net/problem/31287))를 풀다가 정말 안풀려서 지피티든 블로그를 찾아 보려고 했지만 `node.js`로 게시글을 포스팅한 사람이 거의 없다. 그래서 결국 이 문제는 버려버림. 더러운 문제 아오!!! 혹시나 누구라도 저 문제를 `node.js`로 푼다면 도움을 주면 정말 감사하겠습니다🙏

여튼, 이런 것을 보고 조금이나마 내 포스팅으로 기여를 해보고 싶다는 생각이 들었다.. ㅋ <br>
화이팅털털털보아저씨✨🔥
