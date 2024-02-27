---
title: "[Algorithm] 다리를 지나는 트럭"
categories: [algorithm]
tags: [algorithm, programmers, stack]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 스물 일곱 번째 포스팅

안녕하세요! 스물 일곱 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 다리를 지나는 트럭**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 다리를 지나는 트럭 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/42583)

## 💨 **문제 설명**

트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

| 경과 시간 | 다리를 지난 트럭 | 다리를 건너는 트럭 | 대기 트럭 |
| :-------: | :--------------: | :----------------: | :-------: |
|     0     |        []        |         []         | [7,4,5,6] |
|    1~2    |        []        |        [7]         |  [4,5,6]  |
|     3     |       [7]        |        [4]         |   [5,6]   |
|     4     |       [7]        |       [4,5]        |    [6]    |
|     5     |      [7,4]       |        [5]         |    [6]    |
|    6~7    |     [7,4,5]      |        [6]         |    []     |
|     8     |    [7,4,5,6]     |         []         |    []     |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

## 💨 **제한사항**

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

## 💨 **입출력 예**

| bridge_length | weight |          truck_weights          | return |
| :-----------: | :----: | :-----------------------------: | :----: |
|       2       |   10   |            [7,4,5,6]            |   8    |
|      100      |  100   |              [10]               |  101   |
|      100      |  100   | [10,10,10,10,10,10,10,10,10,10] |  110   |

[**[출처]**](https://icpckorea.org/2016/ONLINE/problem.pdf)

![truck](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/626b8007-41f9-49ec-b7df-e38043eab689)<br>

---

# 2️⃣ 문제 풀이

## 🔥 문제 풀이

> 1. 무게 한계치에 못미친다 - passingBridge에 대기트럭.shift() 값 삽입
> 2. 무게 한계치보다 크다 - 임의의 0 삽입
> 3. shift() 연산 존재하니 대기 트럭의 길이가 0보다 커야함
> 4. time을 올려줌
> 5. 다리를 건너는 트럭이 존재하지 않을 때까지 반복함

```js
function solution(bridge_length, weight, truck_weights) {
  let time = 0;
  let bridgeWeight = 0; // 다리 위 트럭의 무게
  let passingBridge = new Array(bridge_length).fill(0); // 다리를 건너는 트럭 배열

  while (passingBridge.length > 0) {
    bridgeWeight -= passingBridge.shift();

    if (truck_weights.length > 0) {
      if (truck_weights[0] + bridgeWeight <= weight) {
        bridgeWeight += truck_weights[0];
        passingBridge.push(truck_weights.shift());
      } else {
        passingBridge.push(0);
      }
    }

    time++;
  }

  return time;
}
```

---

# 3️⃣ 느낀점

문제 설명에 나온 표가 있어서 문제를 쉽게 이해할 수 있었다. 설명이 가시적으로 보이니까 어떤 식으로 움직이는 지 쉽게 파악할 수 있었다. <br>
이 문제는 2016 대전 지역 ICPC에 출제된 문제인가보다. 프로그래머스에서 문제를 직접 만드는 줄 알았는데 이렇게 문제를 가져오기도 하는구나. <br>
아 카카오 문제들도 가져오는구나. ㅋ **그럼 안녕👏**
