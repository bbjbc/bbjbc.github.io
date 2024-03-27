---
title: "[Algorithm] Subarray Product Less Than K"
categories: [leetcode, algorithm]
tags: [algorithm, leetcode, medium, sliding window]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 쉰 여섯 번째 포스팅

안녕하세요! 쉰 여섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **LeetCode - Subarray Product Less Than K**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] Subarray Product Less Than K (문제 링크)**](https://leetcode.com/problems/subarray-product-less-than-k/description/?envType=daily-question&envId=2024-03-27)

## 💨 **문제 설명**

Given an array of integers `nums` and an integer `k`, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than `k`.

## 💨 **제한 사항**

- `1 <= nums.length <= 3 * 104`
- `1 <= nums[i] <= 1000`
- `0 <= k <= 106`

## 💨 **입출력 예**

**Example 1:**

- **Input:** nums = [10,5,2,6], k = 100
- **Output:** 8
- **Explanation:** The 8 subarrays that have product less than 100 are: <br> [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6] <br> Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.

**Example 2:**

- **Input:** nums = [1,2,3], k = 0
- **Output:** 0

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. 이중 포문을 통해서 해결
> 2. product 변수에 값을 곱해가면서 k보다 작은 경우 count를 증가시킴
> 3. product가 k보다 커지면 break
> 4. 시간 복잡도가 O(n<sup>2</sup>)이기 때문에 시간이 너무 오래 걸림

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

var numSubarrayProductLessThanK = function (nums, k) {
  let count = 0;

  for (let i = 0; i < nums.length; i++) {
    let product = 1;

    for (let j = i; j < nums.length; j++) {
      product *= nums[j];

      if (product < k) {
        count++;
      } else break;
    }
  }

  return count;
};
```

간단하게 생각해서 이중 포문으로 해결했다. 풀고 나니 시간복잡도가 O(n<sup>2</sup>) 로 나왔다. 이중 포문을 사용했으니 당연히 그럴 수 밖에 없었다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/57c8bf0a-5530-499a-b325-7e69b8405ff7) <br>

너무나도 크게 떨어져 있어서 다른 방법을 모색했다.

<br>

## 🔥 Two Pointers Algorithm

> 1. 시작점과 끝점을 0으로 초기화하고, 현재 부분 배열의 곱을 저장할 변수 선언.
> 2. 끝점을 오른쪽으로 이동시키면서 현재 부분 배열의 곱을 업데이트.
> 3. 만약 현재 부분 배열의 곱이 k보다 작다면, 부분 배열의 개수 증가시킴.
> 4. 아니라면, 시작점을 오른쪽으로 이동 시키면서 부분 배열의 길이를 줄여 나감.
> 5. 모든 부분 배열을 순회할 때까지 반복.

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */

var numSubarrayProductLessThanK = function (nums, k) {
  if (k <= 1) return 0;

  let count = 0;
  let start = 0;
  let product = 1;

  for (let end = 0; end < nums.length; end++) {
    product *= nums[end]; // 끝점을 오른쪽으로 이동하면서 현재 부분 배열의 곱 갱신

    while (product >= k) {
      // 현재 부분 배열의 곱이 k 이상일 때 시작점 이동
      product /= nums[start];
      start++;
    }

    count += end - start + 1; // 가능한 부분 배열의 수는 (끝점 - 시작점 + 1)
  }

  return count;
};
```

위와 같이 투포인터 알고리즘을 사용하면 시간 복잡도를 O(n)까지 줄일 수 있다.

투포인터 알고리즘은 연속적인 요소들을 처리할 때 유용하게 사용하면 된다. 특히나 효율성 측면에서 매우 좋기 때문에 사용하면 좋다.

투포인터 알고리즘을 사용하여 문제를 해결할 때는 두 개의 포인터를 사용한다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/04980d19-4d69-4cff-bd58-54e2a1bc363a) <br>

위와 같이 런타임이 크게 줄어든 것을 확인할 수 있다.

### 🐾 정의

투포인터 알고리즘은 주로 **배열 또는 리스트에서 두 개의 포인터를 사용하여 특정 조건을 충족하는 부분을 찾거나 원하는 값을 계산**하는 알고리즘 기법이다.

이 알고리즘은 대부분의 경우 시간복잡도를 효과적으로 줄일 수 있으며, 메모리 사용량도 상대적으로 작다.

### 🐾 동작 방식

> 1. **시작 상태 설정**: 보통 시작 지점과 끝 지점을 배열의 처음과 끝으로 초기화한다.
> 2. **포인터 이동**: 문제의 조건에 따라 두 포인터를 이동시킨다.
> 3. **조건 충족 검사**: 각 단계에서 두 포인터가 가리키는 부분이 문제의 조건을 만족하는지 확인한다.
> 4. **최적의 방법 탐색**: 문제에 따라 최적의 방법을 찾기 위해 두 포인터를 조절한다.

주로 부분 배열의 합, 곱 또는 조건에 맞는 부분 구간을 찾는 문제에 적용된다고 한다. 알고리즘의 성능을 높이기 위해서는 위 알고리즘을 적절하게 사용할 줄 알아야 한다❗️

---

# 3️⃣ 느낀점

친구가 리트코드 문제를 보내줘서 풀어보라 했다. 외국 사이트라 문제도 영어로 나오고 문제도 엄청 다양하다.

문제를 풀면 런타임과 메모리까지 다 보여준다. 그것을 통해서 오늘처럼 코드를 리팩토링할 수 있으면서 리팩토링 과정에서 나오는 알고리즘을 공부하면 좋을 것 같다.

투포인터는 이럴 때 사용한다는 것을 알았고 시간복잡도가 대거 감소됐다는 것이 놀라웠다. 화이탱❗️🐧
