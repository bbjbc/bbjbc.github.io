---
title: "[Algorithm] 둘만의 암호"
categories: [algorithm]
tags: [algorithm, programmers, asciicode]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 스물 다섯 번째 포스팅

안녕하세요! 스물 다섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 둘만의 암호**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 둘만의 암호 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/155652)

## 💨 **문제 설명**

두 문자열 `s`와 `skip`, 그리고 자연수 `index`가 주어질 때, 다음 규칙에 따라 문자열을 만들려 합니다. 암호의 규칙은 다음과 같습니다.

- 문자열 `s`의 각 알파벳을 `index`만큼 뒤의 알파벳으로 바꿔줍니다.
- `index`만큼의 뒤의 알파벳이 `z`를 넘어갈 경우 다시 `a`로 돌아갑니다.
- `skip`에 있는 알파벳은 제외하고 건너뜁니다.

예를 들어 `s` = "aukks", `skip` = "wbqd", `index` = 5일 때, a에서 5만큼 뒤에 있는 알파벳은 f지만 [b, c, d, e, f]에서 'b'와 'd'는 `skip`에 포함되므로 세지 않습니다. 따라서 'b', 'd'를 제외하고 'a'에서 5만큼 뒤에 있는 알파벳은 [c, e, f, g, h] 순서에 의해 'h'가 됩니다. 나머지 "ukks" 또한 위 규칙대로 바꾸면 "appy"가 되며 결과는 "happy"가 됩니다.

두 문자열 `s`와 `skip`, 그리고 자연수 `index`가 매개변수로 주어질 때 위 규칙대로 `s`를 변환한 결과를 return하도록 solution 함수를 완성해주세요.

## 💨 **제한사항**

- 5 ≤ `s`의 길이 ≤ 50
- 1 ≤ `skip`의 길이 ≤ 10
- `s`와 `skip`은 알파벳 소문자로만 이루어져 있습니다.
  - `skip`에 포함되는 알파벳은 `s`에 포함되지 않습니다.
- 1 ≤ `index` ≤ 20

## 💨 **입출력 예**

|    s    |  skip  | index | result  |
| :-----: | :----: | ----- | ------- |
| "aukks" | "wbqd" | 5     | "happy" |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

- 본문 내용과 일치합니다.

---

# 2️⃣ 문제 풀이

## 🔥 문제를 풀기 위한 사전 지식

> 1. 문자 → 아스키코드 : [[**charCodeAt()**]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt) <br><br> ![charCodeAt](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/f4e2f679-7de4-4e58-9367-4f953aac884d) <br><br>
> 2. 아스키코드 → 문자 : [[**fromCharCode()**]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode) <br><br> ![fromCharCode](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/298db170-4501-42d7-80ff-f8f0b926597a) <br>
> 3. 알파벳 소문자 아스키코드는 97~122이며 총 26개이다.

<br>

## 🔥 문제 풀이

> 1. s와 skip을 각각 배열로 만들어서 각각의 문자를 아스키코드로 변환한다.
> 2. index의 크기만큼 순회하며 char 값을 소문자 범위를 넘어가지 않도록 올려준다.
> 3. 만약 skip에 포함된 문자라면 다음 문자로 넘어가도록 한다.
> 4. 넘어갈 때도 소문자 범위를 넘어가지 않도록 올려준다.
> 5. 그리고 다시 문자로 변환하여 result에 저장한다.

```java
function solution(s, skip, index) {
  let result = "";
  let arr = [];

  s = [...s].map((a) => a.charCodeAt(a));
  skip = [...skip].map((a) => a.charCodeAt(a));

  for (let char of s) {
    for (let i = 0; i < index; i++) {
      char = ((char - 97 + 1) % 26) + 97;

      for (let j = 0; j < skip.length; j++) {
        if (skip.includes(char)) {
          char = ((char - 97 + 1) % 26) + 97;
        }
      }
    }

    arr.push(char);
  }

  arr.forEach((a) => {
    result += String.fromCharCode(a);
  });

  return result;
}
```

<br>

## 🔥 어떤 사람의 미친 풀이

```java
function solution(s, skip, index) {
  const alphabet = [
    "a",
    "b",
    "c",
    "d",
    "e",
    "f",
    "g",
    "h",
    "i",
    "j",
    "k",
    "l",
    "m",
    "n",
    "o",
    "p",
    "q",
    "r",
    "s",
    "t",
    "u",
    "v",
    "w",
    "x",
    "y",
    "z",
  ].filter((c) => !skip.includes(c));
  return s
    .split("")
    .map((c) => alphabet[(alphabet.indexOf(c) + index) % alphabet.length])
    .join("");
}
```

알파벳 배열 길이가 크지 않아서 오히려 효율적이고 JavaScript의 아스키코드 메서드를 사용하지 않아도 되는 대단하고 멋진 코드이다.

---

# 3️⃣ 느낀점

진짜 아스키코드 관련 메서드를 찾아 돌아 다녔다. 막상 어떻게 풀까 풀까 하다가 어찌저찌 푼 것 같다. <br>
다른 사람들이 푼 코드도 관련 메서드로 풀었지만 위에 미친 사람의 풀이는 다시 봐도 대단하고 존경스럽고 감탄스럽다. ~~감탄떡볶이ㅋㅋ(먹고싶다)~~<br>
원래 저렇게 푸는 거일 수도 있다. ~~원래가 어딨어 뭐 풀고 싶은 대로 푸는거지 뭐 ㅋ;;~~ **그럼 안녕🙆**
