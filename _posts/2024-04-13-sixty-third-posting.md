---
title: "[Algorithm] Trapping Rain Water"
categories: [leetcode, algorithm]
tags: [algorithm, leetcode, hard, array, two pointers, dp, monotonic stack]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 세 번째 포스팅

안녕하세요! 예순 세 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **LeetCode - Trapping Rain Water**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[Leetcode] Trapping Rain Water (문제 링크)**](https://leetcode.com/problems/trapping-rain-water/description/?envType=daily-question&envId=2024-04-12)

## 💨 **문제 설명**

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

## 💨 **제한 사항**

- _n == height.length_
- 1 <= _n_ <= 2 \* 10<sup>4</sup>
- 0 <= _height[i]_ <= 10<sup>5</sup>

## 💨 **입출력 예**

**Example 1:**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/32f6d1db-7981-4613-96a1-368d1e4fe764) <br>

- **Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]
- **Output:** 6
- **Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Example 2:**

- **Input:** height = [4,2,0,3,2,5]
- **Output:** 9

---

# 2️⃣ 문제 풀이

## 🔥 문제 해결 풀이

> 1. two pointers를 활용하여 해결함
> 2. 높이 배열에서 왼쪽과 오른쪽에 포인터를 두고 시작.
> 3. 두 포인터가 가리키는 높이 중 작은 높이를 택함. → 물이 차는 높이는 작은 높이에 의해 결정되기 때문.
> 4. 높이가 작은 쪽의 포인터를 이동시키면서 물이 차는 양을 계산함.
> 5. 양쪽 포인터가 만날 때까지 반복함.

```js
/**
 * @param {number[]} height
 * @return {number}
 */

var trap = function (height) {
  let left = 0;
  let right = height.length - 1;
  let temp = 0;
  let temp2 = 0;
  let count = 0;

  while (left < right) {
    if (height[left] < height[right]) {
      if (height[left] >= temp) {
        temp = height[left];
      } else {
        count += temp - height[left];
      }
      left++;
    } else {
      if (height[right] >= temp2) {
        temp2 = height[right];
      } else {
        count += temp2 - height[right];
      }
      right--;
    }
  }

  return count;
};
```

문제 자체는 이해가 되게 쉽게 되었다. 그림으로 설명되어 있어서 그런지 파악하기는 쉬웠다. 어떻게 풀어야 할 지 고민을 했다.

사실 `Topics`를 보고 투포인터로 풀어야겠다고 파악했다. 근데 문제를 보고도 좌우에서 포인터를 사용해야 할 것이라고 생각하긴 했다.ㅋ

왜냐하면 양 끝점이 있고 사이에 있는 블럭 안에 물이 담기는 최대 물의 양을 구하는 것이기 때문에 두 개의 포인터를 사용해서 양쪽으로 범위를 좁혀가며 계산을 해야할 것이라고 생각을 해보았다.

`left`와 `right`의 두 포인터를 사용해서 양 끝에서 시작한다. 작은 쪽의 높이를 선택해야만 물이 담기는 높이를 파악할 수 있다.

높이가 낮은 쪽을 택하고 해당 쪽의 높이가 기준이 되니까 물의 양을 계산할 수 있을 것이고 반복문을 통해 `left`와 `right`를 좁혀 가다보면 최대의 물의 양이 나오게 된다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/8df819a5-6b2d-41c4-b7bc-f2b880919f86) <br>

**이상. 억셉.**

---

# 3️⃣ 느낀점

투포인터를 접하고 나서는 코드를 보면 직관적이지 않다고 생각했다. 빙빙 돌려서 시간 복잡도와 효율성을 줄이기 위해서 해결하는 느낌이 들었다.

근데 뭐 어쩌겠어. 효율성 좋은 풀이가 어디서나 최고일텐데. 4월 화이팅하자,,, 내 체력아 버텨줘😐
