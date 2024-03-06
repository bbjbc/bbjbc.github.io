---
title: "[Algorithm] 더 맵게"
categories: [algorithm]
tags: [algorithm, programmers, Lv2, heap]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 서른 일곱 번째 포스팅

안녕하세요! 서른 일곱 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 더 맵게**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 더 맵게 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/42626)

## 💨 **문제 설명**

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

```md
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 \* 2)
```

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다. <br>
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

## 💨 **제한사항**

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

## 💨 **입출력 예**

|       scoville       |  K  | return |
| :------------------: | :-: | :----: |
| [1, 2, 3, 9, 10, 12] |  7  |   2    |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
2. 새로운 음식의 스코빌 지수 = 1 + (2 \* 2) = 5
3. 가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

**입출력 예 #2** <br>

1. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
2. 새로운 음식의 스코빌 지수 = 3 + (5 \* 2) = 13
3. 가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

※ 공지 - 2022년 12월 23일 테스트 케이스가 추가되었습니다. 기존에 제출한 코드가 통과하지 못할 수도 있습니다. <br>
※ 공지 - 2023년 03월 23일 테스트 케이스가 추가되었습니다. 기존에 제출한 코드가 통과하지 못할 수도 있습니다.

---

# 2️⃣ 문제 풀이

## 🔥 올바른 문제 풀이

> 1. 최소 힙을 통해 문제를 구현해야 함.
> 2. 모든 음식의 스코빌 지수를 K 이상으로 만들지 못하면 -1 리턴.
> 3. 요굿 사항 대로 최소 힙에서 제일 작은 수와 두번 째로 작은 수를 `poll()`
> 4. 힙에 추가하고 힙을 통한 버블업 → 정렬
> 5. 카운트 올려주고 리턴.

```js
function solution(scoville, K) {
  let count = 0;
  const heap = new MinHeap(scoville);

  while (heap.peek() < K) {
    if (heap.size() === 1) return -1;

    const first = heap.poll();
    const second = heap.poll();
    const mix = first + second * 2;
    heap.add(mix);
    count++;
  }

  return count;
}

class MinHeap {
  constructor(arr = []) {
    this.heap = [];
    arr.forEach((item) => this.add(item));
  }

  add(value) {
    this.heap.push(value);
    this.bubbleUp();
  }

  poll() {
    const min = this.heap[0];
    const last = this.heap.pop();
    if (this.heap.length > 0) {
      this.heap[0] = last;
      this.bubbleDown();
    }
    return min;
  }

  peek() {
    return this.heap[0];
  }

  size() {
    return this.heap.length;
  }

  swap(a, b) {
    [this.heap[a], this.heap[b]] = [this.heap[b], this.heap[a]];
  }

  bubbleUp() {
    let currentIndex = this.heap.length - 1;
    while (currentIndex > 0) {
      const parentIndex = Math.floor((currentIndex - 1) / 2);
      if (this.heap[currentIndex] >= this.heap[parentIndex]) break;
      this.swap(currentIndex, parentIndex);
      currentIndex = parentIndex;
    }
  }

  bubbleDown() {
    let currentIndex = 0;
    while (true) {
      const leftChildIndex = currentIndex * 2 + 1;
      const rightChildIndex = currentIndex * 2 + 2;
      let smallestChildIndex = leftChildIndex;

      if (
        rightChildIndex < this.heap.length &&
        this.heap[rightChildIndex] < this.heap[leftChildIndex]
      ) {
        smallestChildIndex = rightChildIndex;
      }

      if (
        leftChildIndex >= this.heap.length ||
        this.heap[currentIndex] <= this.heap[smallestChildIndex]
      ) {
        break;
      }

      this.swap(currentIndex, smallestChildIndex);
      currentIndex = smallestChildIndex;
    }
  }
}
```

**이 문제 역시 예전에 손을 대고 나서 한참을 손을 놨던 문제이다.** 그 이유는 시간 초과 때문이었다. 물론, 통과하지 못했던 코드도 존재한다.

## 🔥 시간 초과 문제 풀이

```js
function solution(scoville, K) {
  let count = 0;
  scoville.sort((a, b) => a - b);

  while (scoville[0] < K) {
    if (scoville.length === 1) return -1;

    let first = scoville.shift();
    let second = scoville.shift();

    scoville.push(first + second * 2);
    scoville.sort((a, b) => a - b);
    count++;
  }
  return count;
}
```

처음 나의 코드이다. 문제만 보고 쉬워 보였지만 굉장히 까다로웠다. 위의 방법으로 하면 모든 테스트 케이스는 통과하지만 효율성 측면에서 시간 초과가 발생한다. 계속되는 `shift()` 연산과 `sort()`로 인해 시간을 굉장히 많이 잡아 먹는다.

올바른 코드와의 차이점이라고 하면 `shift()`연산과 `sort()`메서드를 메인 함수에서 사용하지 않는다는 것이다. **최소 힙 정렬**을 통해 정렬을 하니 시간복잡도가 `O(N logN)` 이 되기 때문에 통과하게 된다. ~~사실 문제 카테고리가 `힙`으로 되어 있어서 힙으로 풀어야 통과하는구나 생각함.~~

최소 힙의 완벽한 자료구조를 꿰뚫고 있어야 풀 수 있는 문제였다.

물론 나는 힙의 자료구조를 정확히 알지 못했으며 질문하기나 구글링을 해봐도 대부분의 사람들의 문제 풀이가 똑같거나 같았다. 엄청 잘하는 사람이 아니었으면 다들 힙 구조를 터득하고 제출을 한 모양이다.

## 🔥 최소 힙 사전 지식

1. 최소 힙(Min Heap): 부모 노드의 값이 자식 노드의 값보다 작거나 같은 완전 이진 트리이다.
2. 최소 힙을 이용하는 경우 '값이 낮은 데이터가 먼저 삭제'됨
3. 최소 힙을 사용하는 이유는 오름차순 정렬을 통해 문제를 해결하기 위해서이다.
4. 왼쪽 자식의 index = 부모의 index\*2 + 1
5. 오른쪽 자식의 index = (부모의 index\*2) + 2
6. 부모의 index = Math.floor((자식의 index - 1) / 2)
7. 힙은 배열을 사용해 표현한 트리와 같은 자료 구조이다.
8. bubbleUp - 노드가 삽입됐을 때 힙의 구조가 올바르게 될 때까지 항목들을 반복적으로 교환하면서 노드를 위로 이동시킨다.
9. bubbleDown - 노드를 삭제할 때 루트 노드를 삭제하고 맨 마지막에 삽입했던 노드를 위로 올린 후 힙의 구조가 올바르게 될 때까지 항목들을 반복적으로 교환하면서 노드가 아래로 내려간다.
10. poll - 힙에서 `pop()`을 하여 노드를 삭제하는 것이다. 노드를 삭제하면 `bubbleDown`을 통해 다시 정렬이 된다.
11. add - 힙에 `push()`를 하여 노드를 추가하는 것이다. 노드가 삽입되었으니 올바르게 정렬이 되도록 `bubbleUp`을 진행한다.

이러한 여러 가지의 지식이 필요하다.

## 🔥 틀린 문제 풀이

```js
function solution(scoville, K) {
  let count = 0;

  // 최소 힙 로직
  const minHeap = (arr, currentIndex) => {
    let parentIndex;
    while (currentIndex > 0) {
      parentIndex = Math.floor((currentIndex - 1) / 2);
      if (arr[currentIndex] >= arr[parentIndex]) break;
      [arr[currentIndex], arr[parentIndex]] = [
        arr[parentIndex],
        arr[currentIndex],
      ];
      currentIndex = parentIndex;
    }
  };

  for (let i = Math.floor(scoville.length / 2) - 1; i >= 0; i--) {
    minHeap(scoville, i);
  }

  while (scoville[0] < K && scoville.length > 1) {
    const first = scoville[0];
    const second = scoville[1];
    const mix = first + second * 2;
    scoville.splice(0, 2);

    let newIndex = scoville.length;
    scoville.push(mix);
    minHeap(scoville, newIndex);
    count++;
  }

  if (scoville[0] < K && scoville.length === 1) return -1;
  return count;
}
```

그래서 최소 힙 로직에 맞게 작성해 보았지만, 통과하지 못하는 테스트 케이스도 존재하며 `minHeap()` 메서드도 올바르게 작동하지 않나 보다.

또한 `splice()`를 통해 노드를 삭제하는 것도 시간 초과의 원인일 것이다.

---

# 3️⃣ 느낀점

자료구조를 모두 코드에 써 내려가 적용 시켜야 하는 문제였다. 이것이 왜 Level 2이며 통과자가 많은 지 모르겠다. 아 다른 언어에는 힙을 제공하는 메서드가 있어서 그럴 수도 있겠다.

질문하기와 구글을 살펴 봐도 class를 통한 자료구조 풀이가 대부분 똑같이 나와 있다. javascript 사용자들이 풀기엔 버거웠던 문제였나보다. 나만 그럴 수도 있고 ㅋ

암튼 어려웠고 오래오래 손 놓고 있던 문제였다. 이번에도 도전했지만 자료구조를 모두 구현해야 하는 탓에 내 풀이로는 풀 수가 없었다.

코드를 계속 읽고 익히며 어떻게 작동하는 지 파악하도록 해야겠다. **그럼 안녕핑🐌**
