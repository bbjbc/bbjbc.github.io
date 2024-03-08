---
title: "[Algorithm] 파일명 정렬"
categories: [algorithm]
tags: [algorithm, programmers, Lv2, 2018 KAKAO BLIND RECRUITMENT]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 마흔 번째 포스팅

안녕하세요! 마흔 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 마흔❗️

오늘의 포스팅 내용은 **프로그래머스 - 파일명 정렬**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 파일명 정렬 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/17686)

## 💨 **문제 설명**

세 차례의 코딩 테스트와 두 차례의 면접이라는 기나긴 블라인드 공채를 무사히 통과해 카카오에 입사한 무지는 파일 저장소 서버 관리를 맡게 되었다.

저장소 서버에는 프로그램의 과거 버전을 모두 담고 있어, 이름 순으로 정렬된 파일 목록은 보기가 불편했다. 파일을 이름 순으로 정렬하면 나중에 만들어진 ver-10.zip이 ver-9.zip보다 먼저 표시되기 때문이다.

버전 번호 외에도 숫자가 포함된 파일 목록은 여러 면에서 관리하기 불편했다. 예컨대 파일 목록이 ["img12.png", "img10.png", "img2.png", "img1.png"]일 경우, 일반적인 정렬은 ["img1.png", "img10.png", "img12.png", "img2.png"] 순이 되지만, 숫자 순으로 정렬된 ["img1.png", "img2.png", "img10.png", img12.png"] 순이 훨씬 자연스럽다.

무지는 단순한 문자 코드 순이 아닌, 파일명에 포함된 숫자를 반영한 정렬 기능을 저장소 관리 프로그램에 구현하기로 했다.

소스 파일 저장소에 저장된 파일명은 100 글자 이내로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.

파일명은 크게 HEAD, NUMBER, TAIL의 세 부분으로 구성된다.

- HEAD는 숫자가 아닌 문자로 이루어져 있으며, 최소한 한 글자 이상이다.
- NUMBER는 한 글자에서 최대 다섯 글자 사이의 연속된 숫자로 이루어져 있으며, 앞쪽에 0이 올 수 있다. `0`부터 `99999` 사이의 숫자로, `00000`이나 `0101` 등도 가능하다.
- TAIL은 그 나머지 부분으로, 여기에는 숫자가 다시 나타날 수도 있으며, 아무 글자도 없을 수 있다.

|      파일명      | HEAD | NUMBER |    TAIL     |
| :--------------: | :--: | :----: | :---------: |
|     foo9.txt     | foo  |   9    |    .txt     |
| foo010bar020.zip | foo  |  010   | bar020.zip  |
|       F-15       |  F-  |   15   | (빈 문자열) |

파일명을 세 부분으로 나눈 후, 다음 기준에 따라 파일명을 정렬한다.

- 파일명은 우선 HEAD 부분을 기준으로 사전 순으로 정렬한다. 이때, 문자열 비교 시 대소문자 구분을 하지 않는다. `MUZI`와 `muzi`, `MuZi`는 정렬 시에 같은 순서로 취급된다.
- 파일명의 HEAD 부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자 순으로 정렬한다. 9 < 10 < 0011 < 012 < 13 < 014 순으로 정렬된다. 숫자 앞의 0은 무시되며, 012와 12는 정렬 시에 같은 같은 값으로 처리된다.
- 두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다. `MUZI01.zip`과 `muzi1.png`가 입력으로 들어오면, 정렬 후에도 입력 시 주어진 두 파일의 순서가 바뀌어서는 안 된다.

무지를 도와 파일명 정렬 프로그램을 구현하라.

## 💨 **입력 형식**

입력으로 배열 `files`가 주어진다.

- `files`는 1000 개 이하의 파일명을 포함하는 문자열 배열이다.
- 각 파일명은 100 글자 이하 길이로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.
- 중복된 파일명은 없으나, 대소문자나 숫자 앞부분의 0 차이가 있는 경우는 함께 주어질 수 있다. (`muzi1.txt`, `MUZI1.txt`, `muzi001.txt`, `muzi1.TXT`는 함께 입력으로 주어질 수 있다.)

## 💨 **출력 형식**

위 기준에 따라 정렬된 배열을 출력한다.

## 💨 **입출력 예제**

- 입력
  ["img12.png", "img10.png", "img02.png", "img1.png", "IMG01.GIF", "img2.JPG"]
- 출력
  ["img1.png", "IMG01.GIF", "img02.png", "img2.JPG", "img10.png", "img12.png"]

- 입력
  ["F-5 Freedom Fighter", "B-50 Superfortress", "A-10 Thunderbolt II", "F-14 Tomcat"]
- 출력
  ["A-10 Thunderbolt II", "B-50 Superfortress", "F-5 Freedom Fighter", "F-14 Tomcat"]

[**[해설 보러가기]**](https://tech.kakao.com/2017/11/14/kakao-blind-recruitment-round-3/)

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. \D : 숫자가 아닌 문자
> 2. \d : 숫자
> 3. \D+ : 숫자가 아닌 문자가 1개 이상
> 4. \d+ : 숫자가 1개 이상
> 5. i : 대소문자 구분 없이 매칭

![filename](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/49f67cc0-b4de-4b98-80c2-666a38ceffe5) <br>

위 그림은 [**[REGEXPER]**](https://regexper.com/) 에서 정규표현식을 검색하면 그림으로 나타내준다.

```js
function solution(files) {
  return files.sort((a, b) => {
    const getHeadAndNumber = (str) => {
      const regex = /(\D+)(\d+)/i; // 문자+숫자 패턴
      // const match = str.match(regex) // [ 'img12', 'img', '12', index: 0, input: 'img12.png', groups: undefined ]
      const [, head, number] = str.match(regex);
      return [head.toLowerCase(), parseInt(number)];
    };

    const [headA, numberA] = getHeadAndNumber(a);
    const [headB, numberB] = getHeadAndNumber(b);

    // headA가 headB보다 사전순으로 앞에 있으면
    if (headA < headB) {
      return -1;
    }
    // headA가 headB보다 사전순으로 뒤에 있으면
    else if (headA > headB) {
      return 1;
    }
    // headA와 headB가 같으면 숫자 오름차순으로 정렬
    else {
      return numberA - numberB;
    }
  });
}
```

주어진 문제는 꽤나 까다로운 문제라고 생각한다. **정규표현식**의 사용과 **정렬**을 잘 알고 있는 지가 출제의 의도인 것 같다.

세 개의 부분으로 나누기 위해서는 문자, 숫자, 꼬리 부분으로 나누어야 한다. 글자 수도 제각각이며 정해진 틀이 없기 때문에 여러 가지의 테스트 케이스를 만족하기 위해서는 어렵다.

단순히, 문자 다음에 숫자가 등장하면 나누어야 하기 때문에 정규표현식을 통해서 `match()`메서드를 통해 `HEAD` 부분과 `NUMBER` 부분을 분리하는 것이 좋다.

[**[String.prototype.match()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/match) 참고하길 바란다.

물론, 나는 정규표현식을 잘 모른다. 그래서 찾아 보면서 했는데 JavaScript로 코딩테스트 본 분들은 정규표현식까지 꿰뚫고 있는건가.. 괜히 카카오 코테 3차까지 간 사람들이 아니겠지 ㅠ\_ㅠ

TAIL부분은 고려하지 않은 이유는 정렬 기준이나 비교에 영향을 미치지 않기 때문에 무시할 수 있다. 파일명의 정렬은 먼저 HEAD를 기준으로 정렬하고, HEAD가 동일한 경우에 NUMBER를 기준으로 정렬하므로 TAIL은 크게 상관이 없다.

```js
const regex = /(\D+)(\d+)/i;
const match = str.match(regex);

console.log(match); // [ 'img12', 'img', '12', index: 0, input: 'img12.png', groups: undefined ]

// logs [ 'img12', 'img', '12', index: 0, input: 'img12.png', groups: undefined ]
// 'img12'는 완전한 매치 상태임.
// 'img'는 '(\D+)' 부분에 의해 발견된 것임.
// '12' 는 '(\d+)'를 통해 매치된 마지막 값임.
// 'input' 요소는 입력된 원래 문자열을 나타냄.
```

`match()` 메서드를 사용하면 이렇게 여러 가지가 배열에서 나온다. 첫 번째 요소는 전체 문자열, 두 번째는 정규표현식의 앞 부분, 세 번째는 정규표현식의 뒷 부분으로 구분되어 나타난다.

```js
const regex = /(\D+)(\d+)/i;
const [, head, number] = str.match(regex);

console.log(match); // [ <1 empty item>, 'img', '10' ]
```

이런식으로 딱 필요한 값만 추출해 낼 수 있다. 첫 번째 요소를 비워두면 JavaScript에서 알아서 무시한다.

이렇게 해서 문자열 부분과 숫자 부분을 적절하게 뽑아낼 수 있다. 이제 조건에 맞게 정렬만 해주면 된다.

[**[Array.prototype.sort()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 어디서나 쓰이는 메서드이므로 숙지 바란다.

```js
function compare(a, b) {
  if (a is less than b by some ordering criterion) {
    return -1;
  }
  if (a is greater than b by the ordering criterion) {
    return 1;
  }
  // a must be equal to b
  return 0;
}
```

`compareFunction`이 제공되면 배열 요소는 compare 함수의 반환 값에 따라 정렬된다. a와 b가 비교되는 두 요소라면,

- `compareFunction(a, b)`이 0보다 작은 경우 a를 b보다 낮은 색인으로 정렬한다. 즉, a가 먼저온다.
- `compareFunction(a, b)`이 0을 반환하면 a와 b를 서로에 대해 변경하지 않고 모든 다른 요소에 대해 정렬한다. 참고 : ECMAscript 표준은 이러한 동작을 보장하지 않으므로 모든 브라우저(예 : Mozilla 버전은 적어도 2003 년 이후 버전 임)가 이를 존중하지는 않는다.
- `compareFunction(a, b)`이 0보다 큰 경우, b를 a보다 낮은 인덱스로 정렬한다.
- `compareFunction(a, b)`은 요소 a와 b의 특정 쌍이 두 개의 인수로 주어질 때 항상 동일한 값을 반환해야한다. 일치하지 않는 결과가 반환되면 정렬 순서는 정의되지 않는다.

이를 활용해서

```js
// headA가 headB보다 사전순으로 앞에 있으면
if (headA < headB) {
  return -1;
}
// headA가 headB보다 사전순으로 뒤에 있으면
else if (headA > headB) {
  return 1;
}
// headA와 headB가 같으면 숫자 오름차순으로 정렬
else {
  return numberA - numberB;
}
```

이런 식으로 비교를 통한 정렬이 가능하게 된다.

---

# 3️⃣ 느낀점

정렬에 대한 완벽한 이해와 정규표현식의 사용이 아름답게 조화를 이루는 문제이다. 정말 어려운 문제이다.

정규표현식도 자주 쓰는 것은 손과 눈에 익도록 해야할 것 같다. 주어진 조건처럼 특정 부분을 추출할 때나 패턴을 찾는 작업을 할 때에는 정규표현식이 단순하고 효과적이라고 생각한다.

정규표현식에 대한 포스팅도 한 번 써보도록 해야겠다.

정렬에 대한 경각심을 갖게 해준 문제였고 정규표현식의 앎도 소중하게 느낀 문제였다. <br>
**그럼 안녕핑🐌**
