---
title: "[Algorithm] 무인도 여행"
categories: [algorithm]
tags: [algorithm, programmers, Lv2]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 마흔 여덟 번째 포스팅

안녕하세요! 마흔 여덟 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 무인도 여행**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 무인도 여행 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/154540)

## 💨 **문제 설명**

메리는 여름을 맞아 무인도로 여행을 가기 위해 지도를 보고 있습니다. 지도에는 바다와 무인도들에 대한 정보가 표시돼 있습니다. 지도는 1 x 1크기의 사각형들로 이루어진 직사각형 격자 형태이며, 격자의 각 칸에는 'X' 또는 1에서 9 사이의 자연수가 적혀있습니다. 지도의 'X'는 바다를 나타내며, 숫자는 무인도를 나타냅니다. 이때, 상, 하, 좌, 우로 연결되는 땅들은 하나의 무인도를 이룹니다. 지도의 각 칸에 적힌 숫자는 식량을 나타내는데, 상, 하, 좌, 우로 연결되는 칸에 적힌 숫자를 모두 합한 값은 해당 무인도에서 최대 며칠동안 머물 수 있는지를 나타냅니다. 어떤 섬으로 놀러 갈지 못 정한 메리는 우선 각 섬에서 최대 며칠씩 머물 수 있는지 알아본 후 놀러갈 섬을 결정하려 합니다.

지도를 나타내는 문자열 배열 `maps`가 매개변수로 주어질 때, 각 섬에서 최대 며칠씩 머무를 수 있는지 배열에 오름차순으로 담아 return 하는 solution 함수를 완성해주세요. 만약 지낼 수 있는 무인도가 없다면 -1을 배열에 담아 return 해주세요.

## 💨 **제한 사항**

- 3 ≤ `maps`의 길이 ≤ 100
  - 3 ≤ `maps[i]`의 길이 ≤ 100
  - `maps[i]`는 'X' 또는 1 과 9 사이의 자연수로 이루어진 문자열입니다.
  - 지도는 직사각형 형태입니다.

## 💨 **입출력 예**

|                maps                |   result   |
| :--------------------------------: | :--------: |
| ["X591X","X1X5X","X231X", "1XXX1"] | [1, 1, 27] |
|        ["XXX","XXX","XXX"]         |    [-1]    |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

위 문자열은 다음과 같은 지도를 나타냅니다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/11cc26b1-a261-413a-a8a3-b85206af42a3) <br>

연결된 땅들의 값을 합치면 다음과 같으며

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/8ec6873a-152d-4223-a5c2-168c621e0bf5) <br>

이를 오름차순으로 정렬하면 [1, 1, 27]이 됩니다.

**입출력 예 #2** <br>

위 문자열은 다음과 같은 지도를 나타냅니다.

**입출력 예 #3** <br>

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/605fc2a9-d6a6-4de8-8e1e-3c2c5e51e7d0) <br>

섬이 존재하지 않기 때문에 -1을 배열에 담아 반환합니다.

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. 그리드 탐색을 사용하여 탐색하면서 dfs를 사용하여 count를 구함.
> 2. 'X'인 경우는 탐색하지 않음.(무시)
> 3. dfs를 사용하여 탐색하면서 count를 구함.
> 4. 탐색한 count를 배열에 넣고 정렬하여 반환.

```js
function solution(maps) {
  const countArr = [];
  const dx = [-1, 1, 0, 0];
  const dy = [0, 0, -1, 1];
  const arr = maps.map((m) => m.split(""));
  const n = arr.length;
  const m = arr[0].length;
  const visited = Array.from(Array(n), () => Array(m).fill(false));

  function isValid(x, y) {
    return x >= 0 && y >= 0 && x < n && y < m;
  }

  function dfs(x, y) {
    let count = parseInt(arr[x][y]);
    visited[x][y] = true;
    for (let k = 0; k < 4; k++) {
      const nx = x + dx[k];
      const ny = y + dy[k];
      if (isValid(nx, ny) && !visited[nx][ny] && arr[nx][ny] !== "X") {
        count += dfs(nx, ny);
      }
    }
    return count;
  }

  for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
      if (!visited[i][j] && arr[i][j] !== "X") {
        const countDay = dfs(i, j);
        countArr.push(countDay);
      }
    }
  }

  return countArr.length !== 0 ? countArr.sort((a, b) => a - b) : [-1];
}

console.log(solution(["X591X", "X1X5X", "X231X", "1XXX1"]));
```

최근에 푼 문제들이 이차원 배열에서 해당 위치에서 주변 지역을 탐색하는 문제를 풀어서 그런지 익숙해진 느낌이 들었다.

딱 문제를 읽고 나서 어떻게 풀어야 할 지 감이 왔고 바로 술술 코드를 작성할 수 있었다.

위 코드는 DFS 알고리즘을 사용한 그리드 탐색이다. 셀이 연결된 이웃 셀을 검사하여 탐색을 수행하였고,방문 위치는 `visited`를 통해 체크하였다.

`X`에 다다르면 그 위치는 무시하면 된다.

그리드 탐색을 수행하면서 DFS를 적용하며 연결된 모든 위치를 탐색 가능하다.

아 그리고 또 알게 된 것이 있다. [**[Array.from()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 이것이다.

DFS를 위해서 2차원 배열을 저렇게 생성할 수도 있다. 자세한 내용은 공식 문서(위 링크)를 참조 바란다.

---

# 3️⃣ 느낀점

최근에 그리드탐색과 관련한 문제를 풀다 보니 이 문제도 순식간에 이해할 수 있었다.

아직은 아니지만 그래도 DFS와 가까워진 것 같기도 하다. 그래도 어렵긴 하다.

고등학교 수학마냥 진짜 반복해서 풀면 코드가 수학 공식마냥 술술 적힐 것 같다. 물론 난 아직 멀긴 했다. ㅋ 아좌좌좌자좌좌 아자아자분식 ㅋ💕
