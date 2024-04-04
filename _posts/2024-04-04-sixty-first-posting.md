---
title: "[Algorithm] Word Search"
categories: [leetcode, algorithm]
tags: [algorithm, leetcode, medium, dfs, backtracking, matrix]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 한 번째 포스팅

안녕하세요! 예순 한 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **LeetCode - Word Search**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[Leetcode] Word Search (문제 링크)**](https://leetcode.com/problems/word-search/description/?envType=daily-question&envId=2024-04-03)

## 💨 **문제 설명**

Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

## 💨 **제한 사항**

- _m_ = _board.length_
- _n_ = _board[i].length_
- 1 <= _m, n_ <= 6
- 1 <= _word.length_ <= 15
- `board` and `word` consists of only lowercase and uppercase English letters.

## 💨 **입출력 예**

**Example 1:**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/4922a624-ea5b-4479-9486-9a4cd2111f00) <br>

- **Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
- **Output:** true

**Example 2:**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/38f7baa5-b6b6-4af0-a93e-72ad23f28aff) <br>

- **Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
- **Output:** true

**Example 3:**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/c7090b3a-05b5-4167-9599-9d34f6a72f11) <br>

- **Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
- **Output:** false

---

# 2️⃣ 문제 풀이

## 🔥 문제 해결 풀이

> 1. board의 길이와 너비를 n, m에 저장
> 2. dx, dy를 통해 상하좌우를 탐색하기 위한 배열 생성
> 3. visited를 통해 방문 여부를 저장
> 4. dfs를 통해 탐색 시작
> 5. idx가 word의 길이와 같으면 true 반환
> 6. x, y가 board의 범위를 벗어나거나 방문했거나 board[x][y]가 word[idx]와 다르면 false 반환
> 7. 방문 표시를 하고 상하좌우를 탐색
> 8. dfs를 통해 탐색한 결과가 true이면 true 반환
> 9. 방문 표시를 해제하고 false 반환
> 10. 모든 경우를 탐색한 후 false 반환

```js
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */

var exist = function (board, word) {
  const n = board.length;
  const m = board[0].length;
  const dx = [-1, 1, 0, 0];
  const dy = [0, 0, -1, 1];
  const visited = Array.from({ length: n }, () => Array(m).fill(false));

  const dfs = (x, y, idx, visited) => {
    if (idx === word.length) return true;
    if (
      x < 0 ||
      y < 0 ||
      x >= n ||
      y >= m ||
      visited[x][y] ||
      board[x][y] !== word[idx]
    )
      return false;

    visited[x][y] = true;
    for (let i = 0; i < 4; i++) {
      const nx = x + dx[i];
      const ny = y + dy[i];
      if (dfs(nx, ny, idx + 1, visited)) return true;
    }
    visited[x][y] = false;
    return false;
  };

  for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
      if (dfs(i, j, 0, visited)) return true;
    }
  }
  return false;
};
```

문제는 주어진 배열과 단어를 받아서 단어가 보드 안에 존재하는지 여부를 리턴하는 문제이다.

보드의 각 칸을 시작으로 해서 DFS를 수행하며 보드의 모든 칸을 돌면서 탐색을 한다. 현재 위치에서 상하좌우 이동하면서 단어의 각 문자를 찾는데, 이동 가능한 범위를 벗어나거나 이미 방문한 위치라면 탐색을 멈추고 이전 상태로 돌아간다.

해당 위치에서 단어의 다음 문자가 존재하는지 확인하고 존재하면 그 위치로 이동하여 DFS 탐색을 하게 된다.

`visited` 배열을 통해서 방문 위치를 표시하고, 탐색을 진행 후에는 그 위치를 방문하지 않도록 해서 불필요한 탐색을 줄일 수 있다.

```js
for (let i = 0; i < 4; i++) {
  const nx = x + dx[i];
  const ny = y + dy[i];
  visited[x][y] = true;
  if (dfs(nx, ny, idx + 1)) return true;
  visited[x][y] = false;
}

return false;
```

처음에는 위처럼 반복문 안에 방문 여부를 처리하고 되돌리는 것을 넣었다. 이것이 시간을 꽤나 잡고 있는 것 같아서 방문 여부를 `dfs` 함수의 파라미터로 넘기고 호출 종료 후에 방문 여부를 되돌리는 식으로 바꿨다.

`dfs`에 진입할 때 방문 여부를 체크하고 탐색 종료 후에 되돌리는 것이 아니라, 결국에는 방문 여부를 체크 후에 백트래킹을 수행한 것이다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/b0987bc2-b789-4c63-90ee-4992025c07f2) <br>

**이상. 억셉.**

---

# 3️⃣ 느낀점

그리드 탐색과 dfs를 통한 탐색은 프로그래머스에서 몇 번 해결한 후 조금 익숙해진 문제였다.

이 문제도 마찬가지로 그런 방식으로 해결했으며 공식이 된 듯한 느낌이다.

릿코드의 좋은 점이라고 하면 런타임이 나오고 다른 사람들의 런타임까지 렌더링 해주는 것이 좋은 것 같다. 어느 부분에서 내가 느린지 코드를 보며 파악할 수 있고 다른 사람을 이기기 위해서 불필요한 연산을 줄이려고 리팩토링을 해볼 수 있다.

낯설지만 적응하면 되겠지 몽.👍
