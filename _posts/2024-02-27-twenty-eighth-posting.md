---
title: "[Algorithm] 소수 찾기"
categories: [algorithm]
tags: [algorithm, programmers, exhaustive search, dfs]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 스물 여덟 번째 포스팅

안녕하세요! 스물 여덟 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 소수 찾기**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 소수 찾기 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

## 💨 **문제 설명**

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

## 💨 **제한사항**

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

## 💨 **입출력 예**

| numbers | return |
| :-----: | :----: |
|  "17"   |   3    |
|  "011"  |   2    |

## 💨 **입출력 예 설명**

**예제 #1** <br>
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

**예제 #2** <br>
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다. <br>

- 11과 011은 같은 숫자로 취급합니다.

---

# 2️⃣ 문제 풀이

## 🔥 문제 풀이

> 1. 숫자를 이어 붙여 소수인지 판단하기 위해서는 완전 탐색을 함.
> 2. 최단 경로를 따지는 게 아닌 모든 경로를 살펴 탐색 - **dfs 사용**
> 3. dfs 함수에서 판단할 때 소수이면 set에 추가해줌.
> 4. 배열이 아닌 set을 사용하는 이유는 중복을 없애기 위함.
> 5. 또 다른 곳을 탐색할 때를 위해 방문했던 곳은 다시 0으로 바꿈.
> 6. set의 크기 return

```js
function solution(numbers) {
  const visited = new Array(numbers.length).fill(0);
  const set = new Set();

  const dfs = (num, depth) => {
    if (isPrime(parseInt(num)) && depth > 0) {
      set.add(parseInt(num));
    }

    for (let i = 0; i < numbers.length; i++) {
      if (!visited[i]) {
        visited[i] = 1;
        dfs(num + numbers[i], depth + 1);
        visited[i] = 0;
      }
    }
  };

  dfs("", 0);

  return set.size;
}

function isPrime(number) {
  if (number < 2) return false;
  for (let i = 2; i <= Math.sqrt(number); i++) {
    if (number % i === 0) return false;
  }
  return true;
}
```

소수 판별법, 중복 제거, 완전 탐색을 모두 알아야 풀 수 있는 문제였다.

---

# 3️⃣ 느낀점

에라토스테네스의 체를 사용하여 소수 판별 메서드를 작성하고, 모든 경우의 수를 조사하는 완전 탐색을 통해 해결할 수 있었다. <br>
처음에는 배열에 삽입하는 것을 시도해 보았는데 통과가 계속 되지 않았다. clg를 찍어보니 당연하게도 중복값이 생겼고, `Set()`을 통해서 중복을 제거할 수 있었다. <br>
완전 탐색이 어렵기만 하고 거부감이 들었지만 어떻게 접근해야 할 지 잘 생각하고 나면 접근법은 대충 알 것 같다. ~~이게 뭔소리냐? 모르겠다. 열심히 해라 아닐까?~~ **그럼 안녕핑🐌**
