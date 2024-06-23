---
title: "[Baekjoon] 13702.이상한 술집"
categories: [baekjoon, algorithm]
tags: [algorithm, baekjoon, silver2, 이분탐색, Binary Search, 매개 변수 탐색]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 일흔 번째 포스팅

안녕하세요! 일흔 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 일흔💫

오늘의 포스팅 내용은 **백준 - 이상한 술집**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[백준] 이상한 술집 (문제 링크)**](https://www.acmicpc.net/problem/13702)

## 💨 **문제 설명**

프로그래밍 대회 전날, 은상과 친구들은 이상한 술집에 모였다. 이 술집에서 막걸리를 시키면 주전자의 용량은 똑같았으나 안에 들어 있는 막걸리 용량은 랜덤이다. 즉 한 번 주문에 막걸리 용량이 802ml 이기도 1002ml가 나오기도 한다. 은상은 막걸리 `N` 주전자를 주문하고, 자신을 포함한 친구들 `K`명에게 막걸리를 똑같은 양으로 나눠주려고 한다.

그런데 은상과 친구들은 다른 주전자의 막걸리가 섞이는 것이 싫어서, 분배 후 주전자에 막걸리가 조금 남아 있다면 그냥 막걸리를 버리기로 한다. (즉, 한 번 주문한 막걸리에 남은 것을 모아서 친구들에게 다시 주는 경우는 없다. 예를 들어 5명이 3 주전자를 주문하여 1002, 802, 705 ml의 막걸리가 각 주전자에 담겨져 나왔고, 이것을 401ml로 동등하게 나눴을 경우 각각 주전자에서 200ml, 0m, 304ml 만큼은 버린다.) 이럴 때 K명에게 최대한의 많은 양의 막걸리를 분배할 수 있는 용량 ml는 무엇인지 출력해주세요.

## 💨 **입력**

첫째 줄에는 은상이가 주문한 막걸리 주전자의 개수 `N`, 그리고 은상이를 포함한 친구들의 수 `K`가 주어진다. 둘째 줄부터 `N개의 줄에 차례로 주전자의 용량`이 주어진다. N은 10000이하의 정수이고, K는 1,000,000이하의 정수이다. 막걸리의 용량은 2<sup>31</sup> -1 보다 작거나 같은 자연수 또는 0이다. 단, 항상 `N ≤ K` 이다. 즉, 주전자의 개수가 사람 수보다 많을 수는 없다.

## 💨 **입출력 예**

**※ 참고로 변수 설정 및 입력값 설정은 해결 방법에 따라 달라질 수 있음. 아래와 같이 배열 및 변수명은 본인이 커스텀한 것임.**

첫째 줄에 K명에게 나눠줄 수 있는 최대의 막걸리 용량 ml 를 출력한다.

|  N  |  K  |      capacities      | return |
| :-: | :-: | :------------------: | :----: |
|  2  |  3  |      [702, 429]      |  351   |
|  4  | 11  | [427, 541, 774, 822] |  205   |

## 💨 **힌트**

2번째 예제에서 205ml로 나눌 경우 2,2,3,4 가 된다. 하지만 206ml라고 하면 각각 2, 2, 3, 3 으로 나눌 수 있기 때문에 나누는 용량을 조금 줄여야한다.

## 💨 **알고리즘 분류**

- [이분 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=12)
- [매개 변수 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=170)

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. mid를 통해 K명에게 나눌 수 있는지 판단.
> 2. 각 주전자의 용량을 mid로 나누어, 몇 명에게 나눌 수 있는지 count에 저장함.
> 3. count가 K보다 크거나 같으면, answer에 mid를 저장하고, left를 mid + 1로 변경함. → 더 큰 용량을 시도
> 4. 아니라면 right를 mid - 1로 변경함. → 더 작은 용량을 시도
> 5. left가 right보다 커질 때까지 반복함.
> 6. answer를 반환함.

```js
function solution(N, K, capacties) {
  let answer = 0;
  let left = 1;
  let right = Math.max(...capacties);

  while (left <= right) {
    let count = 0;
    let mid = Math.floor((left + right) / 2);

    for (let i = 0; i < N; i++) {
      count += Math.floor(capacties[i] / mid);
    }

    if (count >= K) {
      answer = mid;
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return answer;
}

const fs = require("fs");
const lines = fs.readFileSync("/dev/stdin").toString().trim().split("\n");
const [N, K] = lines[0].split(" ").map(Number);
const capacities = lines.slice(1).map(Number);

const result = solution(N, K, capacities);
console.log(result);
```

어떻게 해결할 지 헤매고 있다가 `알고리즘` 칸에 `이진 탐색`을 보았다. 주어진 문제의 막걸리 주전자의 용량이 2<sup>31</sup> -1 까지 가능하기 때문에 단순하게 하나씩 검사하다가는 `런타임 에러`를 맞이할 것이다. 이진 탐색을 사용하면 효율적인 탐색과 `O(log N)`의 시간복잡도로 해결할 수 있다고 한다.

주전자 용량이 `0`이 될 순 없으니 `left`는 `1`로 설정하고 `right`는 주어진 주전자 용량 중 최대 값으로 설정한다.

`mid`를 통해서 `K`명에게 나눌 수 있는지 계산하여 `count`에 넣어준다.

이 `count`가 `K`명 이상이라면 용량을 올려주는 시도를 해본다. 아니라면 용량을 낮춰준다.

`mid` 값을 사용하는 이유는 효율적인 탐색을 통해 최적값을 빠르게 찾기 위해서이다.

---

# 3️⃣ 느낀점

처음으로 자바스크립트로 백준을 풀어보았다. 확실히 정말 복잡하다. 프로그래머스로만 풀다가 이걸로 풀려고 하니까 정말 귀찮다. . . `fs` 모듈을 통해서 풀고 또한 입력값도 직접 넣어야 한다니 정말정말 귀차니즘이다 우어으어으어😒

하지만, 문제가 정말 다양한 건 좋으니,,, 백준도 애용해보도록 노력해보도록 해보도록 할 것이다..! 화이팅털털털보아저씨✨🔥
