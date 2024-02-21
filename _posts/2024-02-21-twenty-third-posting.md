---
title: "[Algorithm] 숫자 변환하기"
categories: [algorithm]
tags: [algorithm, programmers, bfs, dp]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 스물 세 번째 포스팅

안녕하세요! 스물 세 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

벌써 개강이 다가오네요😞 4학년이라 듣는 학점은 전보다 많이 적지만 ~ 그래도 또 다시 아침 일찍 기상할 생각하니 정말 힘드네요,,
모두모두 이번 학기도 화이팅 넘치고 성적 장학금 받는 학기이길 바랍니다🌟

오늘의 포스팅 내용은 **프로그래머스 - 숫자 변환하기**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 숫자 변환하기 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/154538)

## 💨 **문제 설명**

자연수 `x`를 `y`로 변환하려고 합니다. 사용할 수 있는 연산은 다음과 같습니다.

- `x`에 `n`을 더합니다
- `x`에 2를 곱합니다.
- `x`에 3을 곱합니다.
  자연수 `x`, `y`, `n`이 매개변수로 주어질 때, `x`를 `y`로 변환하기 위해 필요한 최소 연산 횟수를 return하도록 solution 함수를 완성해주세요. 이때 `x`를 `y`로 만들 수 없다면 -1을 return 해주세요.

## 💨 **제한사항**

- 1 ≤ `x` ≤ `y` ≤ 1,000,000
- 1 ≤ `n` < `y`

## 💨 **입출력 예**

|  x  |  y  |  n  | result |
| :-: | :-: | :-: | :----: |
| 10  | 40  |  5  |   2    |
| 10  | 40  | 30  |   1    |
|  2  |  5  |  4  |   -1   |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>
`x`에 2를 2번 곱하면 40이 되고 이때가 최소 횟수입니다.

**입출력 예 #2** <br>
`x`에 `n`인 30을 1번 더하면 40이 되고 이때가 최소 횟수입니다.

**입출력 예 #3** <br>
`x`를 `y`로 변환할 수 없기 때문에 -1을 return합니다.

---

# 2️⃣ 문제 풀이

## 👎 **BFS 풀이 - 시간 초과 (실패)**

> 1. 완전 탐색을 통해서 같아지면 count 반환
> 2. 방문 숫자 저장하는 Set()
> 3. bfs를 위한 배열 queue
> 4. rules를 거치면서 값이 없다면 추가해줌
> 5. number와 목표값 y가 같아지면 return

```js
// bfs 풀이 - TC 9, 10에서 시간 초과 발생
function solution(x, y, n) {
  if (x === y) return 0;
  let visited = new Set();
  let queue = [{ number: x, count: 0 }]; // 변수 x와 count

  while (queue.length > 0) {
    let { number, count } = queue.shift();
    if (number === y) return count;

    let rules = [number * 2, number * 3, number + n];

    for (let rule of rules) {
      if (rule <= y && !visited.has(rule)) {
        visited.add(rule);
        queue.push({ number: rule, count: count + 1 });
      }
    }
  }
  return -1;
}
```

위는 bfs로 푼 것인데 큐를 배열로 바꾸어 봐도 테스트케이스 9, 10, (+11) 까지 시간초과가 남.

<br>

## 👍 **DP 풀이 - 통과**

> 1. dp로 변경
> 2. dp 배열 infinity로 채워주고 base case 선언
> 3. x~y까지 infinity가 아닌 곳에 rule을 적용시켜 카운트 증가
> 4. 최종 dp[y]가 infinity면 만들 수 없는 것임.

```js
function solution(x, y, n) {
  if (x === y) return 0;
  let dp = new Array(y + 1).fill(Infinity);
  dp[x] = 0; // base case

  for (let i = x; i <= y; i++) {
    let rules = [i * 2, i * 3, i + n];
    if (dp[i] !== Infinity) {
      for (let rule of rules) {
        if (rule <= y) {
          dp[rule] = Math.min(dp[rule], dp[i] + 1);
        }
      }
    }
  }
  return dp[y] !== Infinity ? dp[y] : -1;
}
```

dp로 해결하면 무난하게 통과됨. 완전탐색 방법이 안되면 dp로 넘어가는 것이 맞는 것 같음.

# 3️⃣ 느낀점

코드로 보기엔 굉장히 쉬워보일 수 있겠지만 난 어려웠다. ~~불만 있으면 물로 뿌려라. 그래야 불이 꺼질테니.~~ <br>
당연히 bfs 풀이로 풀면 될 줄 알았다. 하지만 조금이라도 변경해서 통과하고 싶어서 발악을 해보았지만 절대 되지 않았다.
dp는 어떻게 해야 하지 고민하다가 크게 달라질 건 없고 중복 계산만 없어지면 되는거니까 그 점을 이용했다.<br>
아 어렵다. 어려워. 앞으로도 배운 점이 많았던 문제나 삘 꽂히는 문제는 포스팅을 하려고 한다. 이상 전달 끝.
