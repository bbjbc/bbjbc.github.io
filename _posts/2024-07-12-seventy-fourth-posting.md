---
title: "[Baekjoon] 1260.DFS와 BFS (node.js)"
categories: [baekjoon, algorithm]
tags:
  [
    algorithm,
    baekjoon,
    silver2,
    dfs,
    bfs,
    그래프 이론,
    그래프 탐색,
    너비 우선 탐색,
    깊이 우선 탐색,
  ]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 일흔 네 번째 포스팅

안녕하세요! 일흔 네 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **백준 - DFS와 BFS**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[백준] DFS와 BFS (문제 링크)**](https://www.acmicpc.net/problem/1260)

## 💨 **문제 설명**

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 `N`번까지이다.

## 💨 **입력**

첫째 줄에 정점의 개수 `N`(1 ≤ N ≤ 1,000), 간선의 개수 `M`(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 `V`가 주어진다. 다음 `M`개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.

## 💨 **출력**

첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. `V`부터 방문된 점을 순서대로 출력하면 된다.

## 💨 **입출력 예**

**※ 참고로 변수 설정 및 입력값 설정은 해결 방법에 따라 달라질 수 있음. 아래와 같이 배열 및 변수명은 본인이 커스텀한 것임.**

|  N   |  M  |  V   |                  arr                   |              return              |
| :--: | :-: | :--: | :------------------------------------: | :------------------------------: |
|  4   |  5  |  1   | [1, 2], [1, 3], [1, 4], [2, 4], [3, 4] |    [1, 2, 4, 3], [1, 2, 3, 4]    |
|  5   |  5  |  3   | [5, 4], [5, 2], [1, 2], [3, 4], [3, 1] | [3, 1, 2, 5, 4], [3, 1, 4, 2, 5] |
| 1000 |  1  | 1000 |      [1000, 1, 1000], [999, 1000]      |     [1000, 999], [1000, 999]     |

## 💨 **알고리즘 분류**

- [그래프 이론](https://www.acmicpc.net/problemset?sort=ac_desc&algo=7)
- [그래프 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=11)
- [너비 우선 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=126)
- [깊이 우선 탐색](https://www.acmicpc.net/problemset?sort=ac_desc&algo=127)

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. 먼저, 그래프를 만들어줌. → 각 노드에 대한 간선 정보를 저장함.
> 2. dfs와 bfs 함수를 만들어줌.
> 3. 숫자가 작은 노드부터 방문하므로, 간선 정보를 오름차순으로 정렬함.
> 4. dfs 함수를 통해 시작 노드부터 탐색하고, bfs 함수를 통해 시작 노드부터 탐색함.
> 5. dfs와 bfs 함수를 호출하고, 결과값을 출력 요구사항에 맞게 출력함.

```js
const fs = require("fs");
const filePath = process.platform === "linux" ? "/dev/stdin" : "example.txt";
const input = fs.readFileSync(filePath).toString().trim().split("\n");
const [N, M, V] = input[0].split(" ").map(Number);
const arr = input.slice(1);

const graph = Array.from({ length: N + 1 }, () => []);

for (let i = 0; i < M; i++) {
  const [a, b] = arr[i].split(" ").map(Number);
  graph[a].push(b);
  graph[b].push(a);
}

for (let i = 1; i <= N; i++) {
  graph[i].sort((a, b) => a - b);
}

function dfs(start) {
  const visited = Array(N + 1).fill(false);
  const result = [];

  function _dfs(v) {
    if (visited[v]) return;
    visited[v] = true;
    result.push(v);

    for (let next of graph[v]) {
      if (!visited[next]) {
        _dfs(next);
      }
    }
  }

  _dfs(start);
  return result;
}

function bfs(start) {
  const visited = Array(N + 1).fill(false);
  const result = [];
  const queue = [start];
  visited[start] = true;

  while (queue.length) {
    const node = queue.shift();
    result.push(node);

    for (let next of graph[node]) {
      if (!visited[next]) {
        visited[next] = true;
        queue.push(next);
      }
    }
  }

  return result;
}

console.log(dfs(V).join(" "));
console.log(bfs(V).join(" "));
```

주어진 문제는 DFS와 BFS를 사용할 수 있다면 금방 해결할 수 있을 것이다. 두 가지 탐색 방법을 활용해서 주어진 그래프에서 특정 노드를 시작점으로 탐색한 결과를 출력하면 된다.

각 간선 정보를 읽어서 `graph` 배열에 추가하여 간선 정보를 저장한다. 그러고나서 각 노드의 인접 노드를 오름차순으로 정렬한다.

그리고 나서 `_dfs` 함수를 통해 현재 노드를 방문 처리하고, 인접 노드를 재귀적으로 방문하면 된다.

또한, BFS도 BFS 로직에 맞게 방문 처리하면 된다.

마지막으로 출력 결과에 맞게 출력하면 된다.

---

# 3️⃣ 느낀점

DFS와 BFS를 깨우치기 좋은 문제인 것 같다.

이상털😌💫
