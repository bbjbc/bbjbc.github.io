---
title: "[Algorithm] Combination Sum II"
categories: [leetcode, algorithm]
tags: [algorithm, leetcode, medium, dfs, backtracking]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 쉰 아홉 번째 포스팅

안녕하세요! 쉰 아홉 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **LeetCode - Combination Sum II**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[Leetcode] Combination Sum II (문제 링크)**](https://leetcode.com/problems/combination-sum-ii/description/)

## 💨 **문제 설명**

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note**: The solution set must not contain duplicate combinations.

## 💨 **제한 사항**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

## 💨 **입출력 예**

**Example 1:**

- **Input:** candidates = [10,1,2,7,6,1,5], target = 8
- **Output:** [[1,1,6], [1,2,5], [1,7], [2,6]]

**Example 2:**

- **Input:** candidates = [2,5,2,1,2], target = 5
- **Output:** [[1,2,2], [5]]

---

# 2️⃣ 문제 풀이

## 🔥 문제 해결 풀이

> 1. 중복된 값을 제거하기 위해 visited 사용
> 2. sum이 target과 같으면 result에 arr를 push.
> 3. sum이 target보다 크면 return
> 4. visited[i]가 true이거나 candidates[i] === candidates[i - 1] && !visited[i - 1]이면 continue
> 5. 4번을 하는 이유 → 중복된 값이 나오면 이전 값이 이미 사용되었는지 확인하기 위해서.
> 6. visited[i]를 true로 바꾸고 arr에 candidates[i]를 push한다.
> 7. dfs를 호출할 때 i + 1로 호출한다.
> 8. dfs를 호출한 후에는 visited[i]를 false로 바꿈 → 다음 dfs에서 사용할 수 있도록 (백트래킹)

```js
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */

var combinationSum2 = function (candidates, target) {
  const result = [];
  const arr = [];
  const visited = Array.from({ length: candidates.length }, () => false);

  candidates.sort((a, b) => a - b);

  const dfs = (start, sum) => {
    if (sum === target) {
      result.push([...arr]);
      return;
    }

    if (sum > target) return;

    for (let i = start; i < candidates.length; i++) {
      if (
        visited[i] ||
        (candidates[i] === candidates[i - 1] && !visited[i - 1])
      )
        continue;

      visited[i] = true;
      arr.push(candidates[i]);

      dfs(i + 1, sum + candidates[i]);

      arr.pop();
      visited[i] = false;
    }
  };

  dfs(0, 0);
  return result;
};
```

백트래킹 문제이다. 먼저 오름차순으로 정렬을 한 후에 DFS를 사용하여 탐색을 진행했다. 정렬을 하는 이유는 다음 값을 고를때엔 같은값을 두번 선택하지 않도록 해야 하기 때문이다.

문제에서 중복된 조합은 제외하라고 했으니, `dfs` 함수에서 조건문을 통해서 중복된 조합을 거를 수 있었다.

현재 값이 이전 값과 같고 이전 값이 방문되지 않았다면 중복된 조합이므로 무시할 수 있다.

걸러진 조합을 배열 안에 넣고 `dfs`를 통해 index와 저장된 값을 갱신한 후 백트래킹을 위해서 `pop()`을 수행한다. 이전 단계로 돌아가기 위해서 마지막에 추가했던 값을 배열에서 제거하여 다음 단계에서 올바른 조합을 찾기 위해 이전 상태로 되돌아간다.

`visited[i]=false`는 백트래킹을 통해 방문한 상태를 다시 되돌리는 것이다. 해당 요소를 방문한 상태에서 이전으로 돌아가기 위해서는 다시 방문하지 않은 상태로 돌려놔야 한다.

이렇게 하면 중복은 허용하지 않으며 합이 target이 되는 모든 조합을 찾아낼 수 있다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/937ba3e7-f2a7-47a7-bfee-b37791d5cdd6) <br>

**이상. 억셉.**

---

# 3️⃣ 느낀점

어떻게 해결할 지 고민하다가 dp로 풀어야할지 dfs로 풀어야할지 하다가 조금 더 익숙한 dfs로 해결했다.

경우의 수를 구하는 거였으면 dp로 풀었을 수도? 모르겠다.

난이도가 가늠이 안된다. 딱 3종류의 난이도만 존재하는데 셋 다 어려운 것 같다.

나를 죽이지 못하는 고통은 나를 더욱 강하게 만든다. 아모띠 최고👍
