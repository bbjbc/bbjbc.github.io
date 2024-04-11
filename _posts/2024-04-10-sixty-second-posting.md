---
title: "[Algorithm] Reveal Cards In Increasing Order"
categories: [leetcode, algorithm]
tags: [algorithm, leetcode, medium, queue, simulation, sorting]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 두 번째 포스팅

안녕하세요! 예순 두 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **LeetCode - Reveal Cards In Increasing Order**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[Leetcode] Reveal Cards In Increasing Order (문제 링크)**](https://leetcode.com/problems/reveal-cards-in-increasing-order/description/?envType=daily-question&envId=2024-04-10)

## 💨 **문제 설명**

You are given an integer array `deck`. There is a deck of cards where every card has a unique integer. The integer on the i<sup>th</sup> card is `deck[i]`.

You can order the deck in any order you want. Initially, all the cards start face down (unrevealed) in one deck.

You will do the following steps repeatedly until all cards are revealed:

1. Take the top card of the deck, reveal it, and take it out of the deck.
2. If there are still cards in the deck then put the next top card of the deck at the bottom of the deck.
3. If there are still unrevealed cards, go back to step 1. Otherwise, stop.

Return an ordering of the deck that would reveal the cards in increasing order.

**Note** that the first entry in the answer is considered to be the top of the deck.

## 💨 **제한 사항**

- 1 <= _deck.length_ <= 1000
- 1 <= _deck[i]_ <= 10<sup>6</sup>
- All the values of `deck` are **unique**.

## 💨 **입출력 예**

**Example 1:**

- **Input:** deck = [17,13,11,2,3,5,7]
- **Output:** [2,13,3,11,5,17,7]
- **Explanation:**
  - We get the deck in the order [17,13,11,2,3,5,7] (this order does not matter), and reorder it.
  - After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
  - We reveal 2, and move 13 to the bottom. The deck is now [3,11,5,17,7,13].
  - We reveal 3, and move 11 to the bottom. The deck is now [5,17,7,13,11].
  - We reveal 7, and move 13 to the bottom. The deck is now [11,17,13].
  - We reveal 5, and move 17 to the bottom. The deck is now [7,13,11,17].
  - We reveal 11, and move 17 to the bottom. The deck is now [13,17].
  - We reveal 13, and move 17 to the bottom. The deck is now [17].
  - We reveal 17.
  - Since all the cards revealed are in increasing order, the answer is correct.

**Example 2:**

- **Input:** deck = [1,1000]
- **Output:** [1,1000]

---

# 2️⃣ 문제 풀이

## 🔥 문제 해결 풀이

> 1. 덱을 오름차순으로 정렬함.
> 2. 배열을 만들어서 덱의 가장 큰 수부터 넣어줌. → 정렬한 덱에서 맨 뒤 숫자 unshift()하면 됨.
> 3. 배열의 맨 앞 숫자를 pop()해서 빼줌.
> 4. 마지막에 배열의 맨 앞 숫자를 push()해서 넣어줌.

```js
/**
 * @param {number[]} deck
 * @return {number[]}
 */

var deckRevealedIncreasing = function (deck) {
  deck.sort((a, b) => a - b);
  const result = [];

  for (let i = deck.length - 1; i >= 0; i--) {
    result.unshift(deck[i]);
    result.unshift(result.pop());
  }

  result.push(result.shift());

  return result;
};
```

문제 해결 방법 자체는 간단하다. 하지만 문제가 이해가 잘 되지 않았다.

나는 **Explanation**을 보고 문제를 해결했다. 설명란을 해결하기 위해서는 거꾸로 해결하면 되겠다고 생각했다.

먼저 오름차순으로 정렬한 뒤 배열의 끝에서 반복문을 통해 `unshift()`를 통해 빼내서 결과 배열에 넣어주었다. 그리고 다시 `pop()`을 하고 `unshift()`를 해주면 된다.

결국, 계속 왔다갔다 하는 것이다. 되게 간단하다. 역순으로 따라가도록 하면 문제는 금방 풀릴 것이다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/e7c39dea-a274-474d-a869-5b19b031854a) <br>

**이상. 억셉.**

---

# 3️⃣ 느낀점

릿코드 특: 문제를 너무 복잡하고 장황하게 써놓음. 사람 헷갈리게;;

문제 이해가 잘 안되는 사람들은 설명란을 계속 살펴보다 보면 규칙이 보일 것이다. 이상 빠이😐
