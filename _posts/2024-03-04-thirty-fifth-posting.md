---
title: "[Algorithm] 124 나라의 숫자"
categories: [algorithm]
tags: [algorithm, programmers, Lv2]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 서른 다섯 번째 포스팅

안녕하세요! 서른 다섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 124 나라의 숫자**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 124 나라의 숫자 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/12899)

## 💨 **문제 설명**

124 나라가 있습니다. 124 나라에서는 10진법이 아닌 다음과 같은 자신들만의 규칙으로 수를 표현합니다.

1. 124 나라에는 자연수만 존재합니다.
2. 124 나라에는 모든 수를 표현할 때 1, 2, 4만 사용합니다.

예를 들어서 124 나라에서 사용하는 숫자는 다음과 같이 변환됩니다.

| 10진법 | 124 나라 | 10진법 | 124 나라 |
| :----: | :------: | :----: | :------: |
|   1    |    1     |   6    |    14    |
|   2    |    2     |   7    |    21    |
|   3    |    4     |   8    |    22    |
|   4    |    11    |   9    |    24    |
|   5    |    12    |   10   |    41    |

자연수 n이 매개변수로 주어질 때, n을 124 나라에서 사용하는 숫자로 바꾼 값을 return 하도록 solution 함수를 완성해 주세요.

## 💨 **제한사항**

- n은 50,000,000이하의 자연수 입니다.

## 💨 **입출력 예**

|  n  | result |
| :-: | :----: |
|  1  |   1    |
|  2  |   2    |
|  3  |   4    |
|  4  |   11   |

※ 공지 - 2022년 9월 5일 제한사항이 수정되었습니다.

---

# 2️⃣ 문제 풀이

## 🔥 문제 풀이

> 1. 일의 자리는 1 2 4 반복
> 2. 십의 자리는 1 2 4 한 사이클이 돌면 1 2 4 사이클이 붙음.
> 3. 문자열로 1, 2, 4를 일정하게 붙여주면 됨.

```js
function solution(n) {
  let answer = "";
  const world = ["4", "1", "2"]; // 인덱스를 생각하면 1번 자리에 1로 시작해야 함.

  while (n > 0) {
    answer = world[n % 3] + answer;
    n = Math.floor((n - 1) / 3);
  }

  return answer;
}
```

이 문제는 규칙성만 찾으면 된다.

| 10진법 | 124나라의 숫자 |
| :----: | :------------: |
|   1    |       1        |
|   2    |       2        |
|   3    |       4        |
|   4    |       11       |
|   5    |       12       |
|   6    |       14       |
|   7    |       21       |
|   8    |       22       |
|   9    |       24       |
|   10   |       41       |
|   11   |       42       |
|   12   |       44       |
|   13   |      111       |
|   14   |      112       |
|   15   |      114       |
|   16   |      121       |
|   17   |      122       |
|   18   |      124       |
|   19   |      141       |
|   20   |      142       |
|   21   |      144       |
|   22   |      211       |
|   23   |      212       |
|   24   |      214       |
|   25   |      221       |
|   26   |      222       |
|   27   |      224       |
|   28   |      241       |
|   29   |      242       |
|   30   |      244       |

규칙성을 파악하기 위해 대충 여기까지 노트에 적어보면서 풀었다.

일단 일의 자리는 1, 2, 4 가 계속 반복되므로 배열 안에 넣고 나머지에 따른 식을 통해 변수에 저장하면 됐다.
배열에서 "4"부터 시작하는 이유는 인덱스는 0번부터 시작하고, 숫자는 1부터 시작하기 때문이다.

3개 단위로 십의 자리 수가 같다는 것을 파악할 수 있다. 3개 단위로 몫은 결국 같다는 것이고 그 몫을 n에 저장하여 배열에서 이어 붙일 수를 찾아 내면 된다.

문제 자체는 미친듯이 단순했다. 솔직히 규칙 찾는 것도 단순했다. 그 규칙을 어떻게 적용하는 지가 어려운 것 같다.

## 🔥 어떤 천재의 풀이

```js
let $,
  solution = ($ = (n) => (n-- > 0 ? $((n / 3) ^ 0) + (1 << n % 3) : ""));
```

놀랍게도 저 풀이가 끝이다. 같은 언어가 맞나 싶은 정도의 풀이고 그냥 javascript를 통달한 사람인 것 같다. 멋있다. 멋있는건가. 멋있긴 하지.

![world124](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/b04dd5a1-89cf-4e73-85a4-d1121b036fd1) <br>

반응도 맛있다. 저런 반응 맛보려고 천재할 수도..

---

# 3️⃣ 느낀점

문제가 짧고 단순할수록 문제 푸는 것이 어려운 것 같다. 문제가 빨리 읽혀서 좋긴 한데 너무 추상적인 느낌이라 어려운 감이 있는 것 같다. 글은 잘 읽혀서 어떻게 풀 지 고민하는 시간이 많아지긴 한다. 문제를 이해하는 데 오래 걸리는 것 보다는 나은 것 같다.

이런 규칙만 찾는 문제는 규칙 찾아서 코드로 옮기기만 하면 된다. 근데 그게 좀 걸렸음. ㅋ <br> **그럼 안녕핑🐌**
