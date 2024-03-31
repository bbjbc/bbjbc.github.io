---
title: "[Algorithm] Subarrays with K Different Integers"
categories: [leetcode, algorithm]
tags: [algorithm, leetcode, hard, sliding window]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 쉰 여덟 번째 포스팅

안녕하세요! 쉰 여덟 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **LeetCode - Subarrays with K Different Integers**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[Leetcode] Subarrays with K Different Integers (문제 링크)**](https://leetcode.com/problems/subarrays-with-k-different-integers/description/?envType=daily-question&envId=2024-03-30)

## 💨 **문제 설명**

Given an integer array `nums` and an integer `k`, return the number of **good subarrays** of nums.

A **good array** is an array where the number of different integers in that array is exactly `k`.

- For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.

A **subarray** is a **contiguous** part of an array.

## 💨 **제한 사항**

- `1 <= nums.length <= 2 * 104`
- `1 <= nums[i], k <= nums.length`

## 💨 **입출력 예**

**Example 1:**

- **Input:** nums = [1,2,1,2,3], k = 2
- **Output:** 7
- **Explanation:** Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]

**Example 2:**

- **Input:** nums = [1,2,1,3,4], k = 3
- **Output:** 3
- **Explanation:** Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].

---

# 2️⃣ 문제 풀이

## 🔥 시간 초과 풀이

> 1. 이중 포문을 통해서 해결
> 2. Set을 이용하여 서로 다른 정수의 개수를 추적함.
> 3. 서로 다른 정수의 개수가 k개가 되는 경우를 찾으면 count를 증가시킴.
> 4. 서로 다른 정수의 개수가 k개가 넘어가는 경우에는 더 이상 탐색을 진행하지 않음.

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

var subarraysWithKDistinct = function (nums, k) {
  let count = 0;

  for (let i = 0; i < nums.length; i++) {
    let numSet = new Set();

    for (let j = i; j < nums.length; j++) {
      numSet.add(nums[j]);

      if (numSet.size === k) {
        count++;
      } else if (numSet.size > k) {
        break;
      }
    }
  }

  return count;
};
```

간단하게 생각해서 이중 포문으로 해결했다. 근데 난이도가 hard인데 바로 풀릴 리가 없었다. 리트코드의 어려움인 것 같다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/97ec723e-a052-4903-a174-127c07a42cd2) <br>

일부 테스트 케이스에서는 시간 초과가 발생한다. 이중 포문이므로 당연히 시간복잡도가 클 수 밖에 없었다. 그래서 Sliding window 방법으로 해보았다.

<br>

## 🔥 시간 초과 풀이 2

> 1. 서로 다른 숫자의 개수가 최대 k개인 부분 배열의 수를 계산하는 함수 most를 정의
> 2. most 함수는 윈도우 내의 서로 다른 숫자의 개수가 k개가 되는 경우에는 윈도우의 왼쪽을 이동하고, 서로 다른 숫자의 개수가 k개가 넘어가는 경우에는 윈도우의 오른쪽을 이동
> 3. 정확히 k개인 부분 배열의 수를 계산하는 것이므로, most(nums, k) - most(nums, k - 1)을 반환

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

var subarraysWithKDistinct = function (nums, k) {
  function most(nums, k) {
    const freq = {};
    let left = 0;
    let count = 0;

    for (let right = 0; right < nums.length; right++) {
      freq[nums[right]] = (freq[nums[right]] || 0) + 1;

      while (Object.keys(freq).length > k) {
        freq[nums[left]]--;
        if (freq[nums[left]] === 0) {
          delete freq[nums[left]];
        }
        left++;
      }

      count += right - left + 1;
    }

    return count;
  }

  // most(nums, k) → 서로 다른 정수가 최대 k개인 부분 배열의 수를 반환
  // most(nums, k - 1) → 서로 다른 정수가 최대 k - 1개인 부분 배열의 수를 반환
  // 따라서, most(nums, k) - most(nums, k - 1) → 서로 다른 정수가 exactly k개인 부분 배열의 수를 반환
  return most(nums, k) - most(nums, k - 1);
};
```

위 풀이마저 시간 초과가 발생한다. 리트코드는 단순한 제약 조건을 주지 않는다. 엄청나게 말도 안되는 제약조건과 테스트 케이스가 존재하기 때문에 시간 복잡도를 무조건 고려해야 한다.

시간 복잡도를 고려한 Sliding window 방법이다.

현재 윈도우에 포함된 각 숫자의 빈도를 `freq` 객체에 저장한다. `Object.keys(freq).length`는 현재 윈도우에 포함된 서로 다른 숫자의 개수를 나타낸다.

현재 윈도우에 포함된 서로 다른 숫자의 개수가 `k`를 초과하는지 확인한다. 만약 초과한다면, `left`에 있는 숫자의 빈도를 줄이고 해당 숫자가 윈도우에 없을 경우에 객체에서 삭제한다.

그 후, `left`를 오른쪽으로 한 칸 이동 시킨다.

이 과정을 반복해서 윈도우의 크기를 조절할 수 있다.

<br>

## 🔥 드디어 통과 풀이

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

var subarraysWithKDistinct = function (nums, k) {
  function most(nums, k) {
    const freq = {};
    let left = 0;
    let count = 0;
    let distinctCount = 0;

    for (let right = 0; right < nums.length; right++) {
      if (!freq[nums[right]]) {
        distinctCount++;
      }
      freq[nums[right]] = (freq[nums[right]] || 0) + 1;

      while (distinctCount > k) {
        freq[nums[left]]--;
        if (freq[nums[left]] === 0) {
          distinctCount--;
        }
        left++;
      }

      count += right - left + 1;
    }

    return count;
  }

  return most(nums, k) - most(nums, k - 1);
};
```

2번 풀이에서 시간이 오래 걸리는 부분을 바꿨다.

`Object.keys(freq).length`를 호출하는 부분에서 매번 `freq` 객체의 키 배열을 생성하는 것이 비효율적일 것이다. 또한 매번 delete 연산을 수행해서 객체를 갱신하기 때문에 시간이 더 소요되는 것일 것이다.

`Object.keys(freq).length` 대신에 윈도우 내의 서로 다른 숫자의 개수를 추적하는 `distinctCount` 변수를 사용했다.

이 변수를 사용해서 `freq` 객체를 삭제하거나 수정하는 대신 윈도우 내의 숫자 개수를 직접 갱신하였다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/962efb8c-e11a-4ce6-83d3-e106028df63e) <br>

대장정 끝에 **억셉**.

---

# 3️⃣ 느낀점

릿코드 너무 어렵다. 제약조건의 범위가 너무 광범위하다 보니까 일반적인 풀이로 통과가 되지 않는다.

그래서 시간 복잡도를 줄일 수 있는 알고리즘들을 사용해서 해결해야 하는 것 같다. 정말정말 어렵다.

오늘의 문제로 나오는 문제를 풀고 있는데 최근에는 `Sliding window`를 사용해야 하는 문제만 나온다. 한 알고리즘을 끝까지 이해 시키려는 릿코드의 전략인가.

슬라이딩 윈도우를 알게 되었지만 아직 친하지 않은 어사이다. 친해지도록 노력할게. 미안. 안녕🙌
