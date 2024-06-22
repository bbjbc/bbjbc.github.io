---
title: "[Algorithm] 네트워크"
categories: [algorithm]
tags: [algorithm, programmers, Lv3, 깊이/너비 우선 탐색(DFS/BFS)]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 아홉 번째 포스팅

안녕하세요! 예순 아홉 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 네트워크**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 네트워크 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/43162)

## 💨 **문제 설명**

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

## 💨 **제한 사항**

- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 `n-1`인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

## 💨 **입출력 예**

|  n  |             computers             | return |
| :-: | :-------------------------------: | :----: |
|  3  | [[1, 1, 0], [1, 1, 0], [0, 0, 1]] |   2    |
|  3  | [[1, 1, 0], [1, 1, 1], [0, 1, 1]] |   1    |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

아래와 같이 2개의 네트워크가 있습니다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/c5e5745b-39e2-431f-9ddb-be3e74d93d27) <br>

**입출력 예 #2** <br>

아래와 같이 1개의 네트워크가 있습니다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d8860e80-f979-4ec6-832d-3e83519e2033) <br>

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. dfs를 이용한 풀이임.
> 2. computers 배열을 순회하면서 연결된 컴퓨터를 찾아서 방문 처리함. → 1로 표시된 컴퓨터는 연결되어 있음.
> 3. 방문하지 않은 컴퓨터가 있으면 dfs를 호출하고, answer를 증가시킴.
> 4. 모든 컴퓨터를 방문하면 answer를 반환함.

```js
function solution(n, computers) {
  let answer = 0;
  let visited = Array.from({ length: n }, () => false);

  const dfs = (start) => {
    visited[start] = true;

    for (let i = 0; i < n; i++) {
      if (computers[start][i] === 1 && !visited[i]) {
        dfs(i);
      }
    }
  };

  for (let i = 0; i < n; i++) {
    if (!visited[i]) {
      dfs(i);
      answer++;
    }
  }

  return answer;
}

console.log(
  solution(3, [
    [1, 1, 0],
    [1, 1, 0],
    [0, 0, 1],
  ])
); // 2
```

주어진 문제는 간단하게 dfs를 구현하는 되는 문제이다. 물론 bfs를 활용하여 문제를 해결할 수 있다. 하지만 나는 dfs로 풀어보았다.

`visted`를 통해서 방문 여부를 처리해주고, 네트워크가 연결되어있으면 `1`로 표현해주는 것이니 연결 여부를 확인을 해주면 된다. 이렇게 재귀적으로 dfs를 통해 모든 컴퓨터를 방문한다.

아직 방문하지 않는 노드가 존재하면 dfs를 통해 재귀적으로 돌린 후 `answer`값을 증가시켜 주면 된다.

```js
function solution(n, computers) {
  let answer = 0;
  let visited = Array.from({ length: n }, () => false);

  const bfs = (start) => {
    let queue = [start];
    visited[start] = true;

    while (queue.length > 0) {
      let node = queue.shift();

      for (let i = 0; i < computers.length; i++) {
        if (computers[node][i] === 1 && !visited[i]) {
          queue.push(i);
          visited[i] = true;
        }
      }
    }
  };

  for (let i = 0; i < n; i++) {
    if (!visited[i]) {
      bfs(i);
      answer++;
    }
  }

  return answer;
}
```

위는 bfs로 풀어본 것이다. 혹시나 차이가 있을까 하고 돌려봤는데 별 차이가 없었다.

|                                                   DFS                                                    |                                                   BFS                                                    |
| :------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------: |
| ![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d27a03e7-5d9a-4e21-97b2-4035fd140092) | ![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/572aa23a-7b64-4458-b00f-9535173ed3c6) |

제한 사항이 그렇게 크지 않아서 그런 것 같기도 하고.. 어쨌든 이 문제의 풀이 중 더 간단한 것은 dfs라고 생각한다..

---

# 3️⃣ 느낀점

오랜만에 dfs, bfs 감각을 익히니 재밌었다.

어떤 알고리즘이 효율적인지 궁금해서 찾아보았다.

- **DFS가 더 적합한 경우**
  - 재귀적 문제 해결이 필요한 경우
  - 메모리가 중요한 경우
  - 특정 깊이까지의 경로를 탐색하는 경우
- **BFS가 더 적합한 경우**
  - 최단 경로를 찾는 문제
  - 레벨 별 탐색이 필요한 문제
  - 너비 우선 탐색이 필요한 문제

잘 숙지해두고 잘 써먹자. **이상❗️**
