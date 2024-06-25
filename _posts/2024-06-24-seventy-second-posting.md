---
title: "[Baekjoon] 1325.효율적인 해킹 (node.js)"
categories: [baekjoon, algorithm]
tags: [algorithm, baekjoon, silver1, 그래프 이론, 그래프 탐색, DFS, BFS]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 일흔 두 번째 포스팅

안녕하세요! 일흔 두 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **백준 - 효율적인 해킹**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[백준] 효율적인 해킹 (문제 링크)**](https://www.acmicpc.net/problem/1325)

## 💨 **문제 설명**

해커 김지민은 잘 알려진 어느 회사를 해킹하려고 한다. 이 회사는 `N`개의 컴퓨터로 이루어져 있다. 김지민은 귀찮기 때문에, 한 번의 해킹으로 여러 개의 컴퓨터를 해킹 할 수 있는 컴퓨터를 해킹하려고 한다.

이 회사의 컴퓨터는 신뢰하는 관계와, 신뢰하지 않는 관계로 이루어져 있는데, A가 B를 신뢰하는 경우에는 B를 해킹하면, A도 해킹할 수 있다는 소리다.

이 회사의 컴퓨터의 신뢰하는 관계가 주어졌을 때, 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 출력하는 프로그램을 작성하시오.

## 💨 **입력**

첫째 줄에, `N`과 `M`이 들어온다. `N`은 10,000보다 작거나 같은 자연수, `M`은 100,000보다 작거나 같은 자연수이다. 둘째 줄부터 `M`개의 줄에 신뢰하는 관계가 A B와 같은 형식으로 들어오며, "A가 B를 신뢰한다"를 의미한다. 컴퓨터는 1번부터 `N`번까지 번호가 하나씩 매겨져 있다.

## 💨 **출력**

첫째 줄에, 김지민이 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 오름차순으로 출력한다.

## 💨 **입출력 예**

**※ 참고로 변수 설정 및 입력값 설정은 해결 방법에 따라 달라질 수 있음. 아래와 같이 배열 및 변수명은 본인이 커스텀한 것임.**

|  L  |  R  |               arr                | return |
| :-: | :-: | :------------------------------: | :----: |
|  5  |  4  | [[3, 1], [3, 2], [4, 3], [5, 3]] |  1 2   |

## 💨 **알고리즘 분류**

- [그래프 이론](https://www.acmicpc.net/problemset?sort=ac_desc&algo=7)
- [그래프 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=11)
- [너비 우선 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=126)
- [깊이 우선 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=127)

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. 각 컴퓨터를 노드로 보고, 신뢰 관계를 간선으로 보는 그래프를 만듦.
> 2. 주어진 신뢰 관계를 통해 그래프를 채움
> 3. 특정 노드에서 시작해 dfs를 수행하고 해킹 가능한 컴퓨터 수 반환함.
> 4. 최대 해킹 가능한 컴퓨터 수와 비교하여 값을 갱신함.
> 5. 결과를 출력값에 맞게 반환함.

```js
const fs = require("fs");
const filePath = process.platform === "linux" ? "/dev/stdin" : "example.txt";
const [input, ...arr] = fs.readFileSync(filePath).toString().trim().split("\n");
const [N, M] = input.split(" ").map(Number);

// 그래프를 만듦.
const graph = Array.from({ length: N + 1 }, () => []);

// 각 노드에 대한 간선 정보를 저장
for (let i = 0; i < M; i++) {
  const [front, back] = arr[i].split(" ");
  graph[+back].push(+front);
}

// 각 노드에 대한 DFS
function dfs(start) {
  const stack = [start];
  const visited = new Array(N + 1).fill(false);
  let count = 0; // 해킹 가능한 컴퓨터 수
  visited[start] = true; // 시작 노드 방문 처리

  while (stack.length) {
    const node = stack.pop();
    // 해당 노드와 연결된 노드들을 탐색
    for (let next of graph[node]) {
      if (visited[next]) continue;
      stack.push(next);
      visited[next] = true;
      count++;
    }
  }

  return count;
}

let max = -1;
let answer = [];

for (let i = 1; i <= N; i++) {
  let count = dfs(i);
  if (count > max) {
    max = count;
    answer = [i];
  } else if (count === max) {
    answer.push(i);
  }
}

console.log(answer.join(" "));
```

각 컴퓨터의 신뢰 관계를 그래프로 표현을 하고 각 노드에 대해 DFS를 수행하여 연결된 모든 노드를 탐색하는 것이 이 문제 풀이 방법이다. DFS를 통해 연결 요소를 찾고, 크기를 구하기 쉬울 것이다.

각자의 신뢰 관계를 파악하자면 `[front, back]`을 추출하여 `back`이 `front`를 신뢰한다고 할 때, `graph[back]` 배열에 `front`를 추가하면 각 컴퓨터가 신뢰하는 다른 컴퓨터들의 관계가 구성된다.

그 후, DFS 함수를 통해서 반복적으로 호출해야 한다. 해당 노드와 연결된 노드들을 탐색하는 과정을 반복한다.

마지막으로 `1`부터 `N`까지의 모든 노드에 대해서 dfs를 호출하여 `count`에 저장하고 최대 해킹 가능한 컴퓨터 수(`maxCount`)와 `count`를 비교하여 업데이트를 해준 후 `answer`에 추가해준다.

그리고 출력 요구사항에 맞게 출력해주면 된다.

---

# 3️⃣ 느낀점

이 문제를 풀면서 미친듯이 화가 났다. 로직 상으로는 다 맞는데 계속 시간 초과가 났다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/94f8c311-ff46-41c0-85bd-8c9770df7123) <br>

이미 구글에 게시된 타 블로그를 보고 로직을 수정해봤지만 그것마저 시간초과가 발생했다. 나는 프로그래머스에서 항상 풀던 방식대로 `solution`함수로 풀었었다. 하지만 이 함수를 없애고 해보니 제대로 작동했다. . . 이게 무슨.. 일인가 . . . . . . ~~그냥 말도 안되는 걸로 꼬투리 잡는 것 같아 화가 치밀어 올랐다.~~

또한, 파일 입력과정도 평소와는 다른 것이 문제가 됐나보다. 그것까지도 시간초과 요소로 만들거면 입력값 범위를 줄이던가 하면 좋겠다. 😡😡😡
