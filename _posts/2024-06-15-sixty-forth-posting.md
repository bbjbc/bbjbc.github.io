---
title: "[Algorithm] 단어 변환"
categories: [algorithm]
tags: [algorithm, programmers, Lv3, 깊이/너비 우선 탐색(DFS/BFS)]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 네 번째 포스팅

안녕하세요! 예순 네 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 단어 변환**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 단어 변환 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/43163)

## 💨 **문제 설명**

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

```md
한 번에 한 개의 알파벳만 바꿀 수 있습니다.
words에 있는 단어로만 변환할 수 있습니다.
```

예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

## 💨 **제한 사항**

- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

## 💨 **입출력 예**

| begin | target |                   words                    | return |
| :---: | :----: | :----------------------------------------: | :----: |
| "hit" | "cog"  | ["hot", "dot", "dog", "lot", "log", "cog"] |   4    |
| "hit" | "cog"  |    ["hot", "dot", "dog", "lot", "log"]     |   0    |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

문제에 나온 예와 같습니다.

**입출력 예 #2** <br>

target인 "cog"는 words 안에 없기 때문에 변환할 수 없습니다.

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. 깊이 우선 탐색으로 해결하였음.
> 2. 방문한 단어를 저장하는 visited 배열과 탐색할 단어를 저장하는 queue 배열을 사용함.
> 3. queue 배열에서 shift()로 하나씩 빼서 탐색하며, target과 같은 단어가 나오면 count를 반환함.
> 4. 단어를 비교할 때, 한 글자만 다른 단어를 찾아야 하므로 notEqual 변수를 사용함.
> 5. notEqual이 1이면 queue에 추가하고, visited에 추가함.
> 6. visited에 추가한 단어는 다시 탐색하지 않도록 함.
> 7. target과 같은 단어가 나오면 count를 반환함.
> 8. target이 없으면 0을 반환함.

```js
function solution(begin, target, words) {
  let answer = 0;
  let visited = [];
  let queue = [];

  if (!words.includes(target)) return 0;
  queue.push([begin, answer]);

  while (queue) {
    let [v, count] = queue.shift();
    if (v === target) return count;

    words.forEach((word) => {
      let notEqual = 0;
      if (visited.includes(word)) return;
      for (let i = 0; i < word.length; i++) {
        if (word[i] !== v[i]) notEqual++;
      }
      if (notEqual === 1) {
        queue.push([word, ++count]);
        visited.push(word);
      }
    });
  }

  return answer;
}

console.log(solution("hit", "cog", ["hot", "dot", "dog", "lot", "log", "cog"]));
```

주어진 문제는 시작 단어에서 목표 단어까지 변환하는 최소 단계 수를 찾는 문제이다. 이 문제를 해결하기 위해서는 `visited`를 통해 이미 방문했던 단어는 방문하지 않도록 하는 것이며, 너비 우선 탐색(`BFS`) 알고리즘을 통해 해결할 수 있다.

코드는 되게 직관적이라고 생각한다. `visited` 배열에는 이미 방문한 단어를 저장해 중복 방문을 방지하는 것이고, `queue`는 BFS를 수행하기 위한 큐이다.

목표한 단어가 단어 리스트에 없는 경우도 처리해줘야 한다. 제한 사항에 그런 것이 없기 때문.

큐가 빌 때까지 BFS 탐색을 계속해서 진행하며, 큐의 앞에서 요소를 `shift()`하여 현재 단어(`v`)와 현재 카운팅(`count`)을 뽑아온다. <br>
주어진 문제는 문자 하나만 바꾸어 변경하는 것이기 때문에 `notEqual`이 1일 경우만 처리해주면 된다.

---

# 3️⃣ 느낀점

되게 오랜만에 다시 감을 잡기 위해서 풀어보았다. 오랜만이라 조금 오래 걸리기도 했고 어색했지만 앞으로 다시 차근차근 감을 익힐 예정이며 꾸준히 풀어볼 것이다.

구현 문제를 비롯하여 여러 알고리즘을 접하여 알고리즘에 대한 부담감을 덜고 싶다. 화이팅팅털❗️💕
