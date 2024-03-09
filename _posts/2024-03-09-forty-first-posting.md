---
title: "[JavaScript] 정규 표현식의 이해"
categories: [javascript]
tags: [javascript, regular expression]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 마흔 한 번째 포스팅

안녕하세요! 마흔 한 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **자바스크립트 - 정규 표현식**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 정규표현식 (Regular Expression)

## 😏 정규표현식이란?

JavaScript 정규표현식(Regular Expression)은 문자열에서 특정 패턴을 찾거나 일치 시키기 위한 도구이다. 예를 들면, 휴대폰 번호나 이메일 주소의 형식을 확인하거나 특정 단어를 찾는 등의 작업에 사용된다.

[**[RegExp]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp) 공식 문서에도 친절하게 설명이 되어 있으니 내 글도 읽고 공식 문서도 참조 바란다.

외계어 같은 문자들의 조합으로 암호 형태를 띄고 있는 정규표현식은 구현하기가 쉽지 않고, 간단한 형식인 만큼 함축된 의미를 해석하기에 가독성이 좋지 않은 문제점도 존재한다.

하지만, [**[Lv.2 - 파일명 정렬]**](https://bbjbc.github.io/algorithm/fortieth-posting/) 이 문제에서 보았듯이 정규표현식을 사용하면 문제를 깔꿀마스럽게 해결 가능하다.

---

# 2️⃣ 정규표현식의 사용 방법

## 🚩 리터럴 표기법

리터럴 표기법은 표현식을 평가할 때 정규 표현식을 컴파일한다고 한다. 정규 표현식의 리터럴은 아래처럼 표현한다.

![regex](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/6e03390f-de29-40f6-8e51-81d5f58a8165) <br>

위처럼 귀염뽀짝하게 하나 만들어 보았다.

```js
const regex = /(D+)(d+)/i;
```

> 😮 **리터럴이란?** <br>
> 변수의 값이 변하지 않는 데이터 그 자체를 의미한다. <br>
> 슬래시와 슬래시 사이에 검색할 문자열 패턴을 넣고, 슬래시가 끝나는 순서에 필요에 따라 플래그를 추가할 수 있다. <br>
> 리터럴 표기법과 생성자로써 `RegExp` 객체를 생성할 수 있다.

```js
/ab+c/i;
new RegExp(/ab+c/, "i"); // 리터럴
new RegExp("ab+c", "i"); // 생성자
```

<br>

## 🚩 플래그 (flag)

플래그는 정규 표현식에서 필수 사항은 아니다. 즉, 옵션이다. 순서와 상관없이 하나 이상의 플래그를 설정 가능하다. <br>
플래그를 사용하지 않는 경우라면 문자열 내 검색 대상이 1개 이상이더라도 첫 번째 조건 대상만을 검색하고 종료하게 된다.

> 😲 **자주 사용하는 플래그** <br> **g(global)** : 문자열 내의 모든 패턴을 검색한다. <br> **i(ignore case)** : 대소문자를 구별하지 않고 검색한다. <br> **m(multi line)** : 문자열의 행이 바뀌더라도 검색을 계속한다.

```js
const str = "Hello World! hello everyone!";
const regex = /hello/i; // 대소문자를 구별하지 않음.
const result = str.match(regex);

console.log(result); // ["Hello", index: 0, input: "Hello World! hello everyone!", groups: undefined]
```

<br>

## 🚩 메타 문자

정규 표현식의 메타 문자는 패턴을 나타내는 데 사용되는 특별한 문자이다. 이러한 메타 문자를 사용하여 문자열 내에서 특정 패턴을 찾거나 조작이 가능하다.

메타 문자는 정말 다양하고 많기 때문에 모든 메타 문자를 외울 필요는 없다. 찾아서 사용하면 되긴 하는데 코딩 테스트를 볼 때에는 어떻게 해야 하나 모르겠다.

여튼 정규 표현식을 보고도 어려워서 잘 사용할 줄 알아야 한다.

### 📍 메타 문자 : 매칭 패턴

|    패턴    |                               의미                               |
| :--------: | :--------------------------------------------------------------: |
|   a-zA-Z   |                   영어알파벳(-으로 범위 지정)                    |
| ㄱ-ㅎ가-힣 |                    한글 문자(-으로 범위 지정)                    |
|    0-9     |                      숫자(-으로 범위 지정)                       |
|     .      | 모든 문자열(숫자, 한글, 영어, 특수기호, 공백 모두! 단, 줄바꿈 x) |
|     \d     |                               숫자                               |
|     \D     |                          숫자가 아닌 것                          |
|     \w     |                영어 알파벳, 숫자, 언더스코어(\_)                 |
|     \W     |                          \w 가 아닌 것                           |
|     \s     |                            space 공백                            |
|     \S     |                       space 공백이 아닌 것                       |
| \특수기호  |                             특수기호                             |

```js
const text = "Today is March 8, 2024.";

const regex = /\d+/g; // 하나 이상의 숫자를 찾는 정규표현식
const result = text.match(regex);

console.log(result); // ["8", "2024"]
```

### 📍 메타 문자 : 검색 패턴

|   기호   |                        의미                         |
| :------: | :-------------------------------------------------: |
|    []    |              괄호 안의 문자들 중 하나               |
| [^문자]  |             괄호 안의 문자를 제외한 것              |
| ^문자열  |            특정 문자열로 시작(괄호 없음)            |
| 문자열$  |                 특정 문자열로 끝남                  |
|    ()    | 그룹 검색 및 분류(match 메서드에서 그룹별로 묶어줌) |
| (?:패턴) |                  그룹 검색(분류x)                   |
|    \b    |                   단어의 처음/끝                    |
|    \B    |                단어의 처음/끝이 아님                |

```js
const text = "Hello, World! Hi, World!";

const regex = /^H/; // "H"로 시작하는 문자열을 찾는 정규 표현식임.
const result = text.match(regex);

console.log(result); // ["Hello", "Hi"]
```

### 📍 메타 문자 : 횟수 패턴

|    기호    |               의미               |
| :--------: | :------------------------------: |
|     ?      |     최대 한 번(없음 or 1개)      |
|     \*     |   (없거나 있음): 여러 개 포함    |
|     +      |     최소 1개(1개 or 여러 개)     |
|    {n}     |               n개                |
|   {Min,}   |         최소 Min개 이상          |
| {Min, Max} | 최소 Min개 이상, 최대 Max개 이하 |

```js
const text = "The number is 123456.";

const regex = /\d{3}/; // 숫자가 3개 연속으로 나오는 패턴을 찾는 정규 표현식임.
const result = text.match(regex);

console.log(result); // ["123"]
```

<br>

## 🚩 패턴

매칭하여 검색하고 싶은 문자열을 지정하는 것이다.<br>
기존처럼 문자열의 따옴표를 포함해서 선언하면 따옴표까지 검색하기 때문에 따옴표는 생략한다.

정규 표현식 패턴을 작성할 때는 일반 문자와 특수 문자를 사용할 수 있는데, 일반 문자는 **리터럴 문자**, 특수 문자는 **메타 문자**로 표현한다.

> **리터럴 문자 (정규 문자)** : 일반 문자, \0, \n, \t, \v, \f, \r, \xhh, \uhhhh, \cX <br>
> **메타 문자 (정규 표현식의 구문 문자)** : ^ $ \ . \* + ? ( ) [ ] { } |\_

---

# 3️⃣ 자주 사용하는 정규 표현식

> **이메일 주소 검증**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/406bb3bb-db52-4699-be49-d212263a1db8) <br>

```js
const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
```

> **URL 검증**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/fefe16aa-318a-43d7-8da4-dae0d47ccc46) <br>

```js
const urlPattern =
  /^(http(s)?:\/\/)?(www\.)?[a-zA-Z0-9_-]+(\.[a-zA-Z]{2,}){1,}/;
```

> **전화번호 검증**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/a711f7fd-c7d5-43f6-8e13-361b45bf7cd8) <br>

```js
const phonePattern = /^\d{3}-\d{3,4}-\d{4}$/;
```

> **비밀번호 강도 검증**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d805df15-19e0-4f07-994f-e839fb635c1a) <br>

```js
const passwordPattern =
  /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;
```

> **이름 검증(한글 이름만)**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/fe66f143-8d5e-4342-a38c-72b771b263ed) <br>

```js
const namePattern = /^[ㄱ-ㅎ가-힣]+$/;
```

> **날짜 형식 검증(YYYY-MM-DD)**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/3c908af6-d216-4e62-821e-2abf45dd5000) <br>

```js
const datePattern = /^\d{4}-\d{2}-\d{2}$/;
```

> **주민등록번호 검증(YYMMDD-NNNNNNN 형식)**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/bb952eac-41ec-4893-a2ad-73b51aa4624b) <br>

```js
const ssnPattern = /^\d{6}-\d{7}$/;
```

> **IP 주소 검증**

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d02bb5e5-0dad-4af7-95af-31c4f97588fd) <br>

```js
const ipPattern =
  /^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;
```

**와우, 정말 힘들군. 정말 외계어스럽군. 참나 ~**

---

# 4️⃣ 정규 표현식의 다양한 메서드

## 📜 test()

[**[RegExp.prototype.test()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) 참고 바란다.

문자열이 정규 표현식과 일치하는 지 여부를 확인한다. 일치한다면 `true`를 반환하고, 그렇지 않으면 `false`를 반환한다.

```js
const text = "Hello, World!";
const regex = /hello/;

const isMatch = regex.test(text); // false
```

## 📜 exec()

[**[RegExp.prototype.exec()]**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) 참고 바란다.

문자열에서 정규 표현식과 일치하는 부분을 검색하고 해당 정보를 배열로 반환한다. 이 메서드는 정규 표현식에 `g` 플래그가 설정되어 있을 때 반복적으로 사용 가능하다.

```js
const text = "The numbers are 123 and 456.";
const regex = /\d+/g;
let match;

while ((match = regex.exec(text)) !== null) {
  console.log(match);
}

// logs:
// [
//   '123',
//   index: 16,
//   input: 'The numbers are 123 and 456.',
//   groups: undefined
// ]
// [
//   '456',
//   index: 24,
//   input: 'The numbers are 123 and 456.',
//   groups: undefined
// ]
```

## 📜 match()

[**[String.prototype.match()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/match) 참고 바란다.

문자열에서 정규 표현식과 일치하는 부분을 찾아 배열로 반환한다.

```js
const text = "The number is 1234.";
const regex = /\d+/;

const result = text.match(regex);
console.log(result);
// [ '1234', index: 14, input: 'The number is 1234.', groups: undefined ]
```

## 📜 search()

[**[String.prototype.search()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/search) 참고 바란다.

문자열에서 정규 표현식과 일치하는 부분의 인덱스를 반환한다. 일치하는 부분이 없으면 -1을 반환한다.

```js
const text = "Hello, world!";
const regex = /world/;

const index = text.search(regex); // 7
```

## 📜 replace()

[**[String.prototype.replace()]**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace) 참고 바란다.

문자열에서 정규표현식과 일치하는 부분을 지정된 문자열로 대체한다.

```js
const text = "Hello, world!";
const regex = /world/;

const newText = text.replace(regex, "everyone"); // "Hello, everyone!"
```

## 📜 split()

[**[String.prototype.split()]**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) 참고 바란다.

문자열을 정규표현식에 따라 분할하여 배열로 반환한다.

```js
const text = "Hello World!";
const regex = /\s+/;

const parts = text.split(regex); // ["Hello", "World!"]
```

---

# 5️⃣ 마무리

자 정규 표현식에 대해 알아보았다. 거의 몰랐었지만 포스팅을 끄적이면서 어떻게 사용하는 것이며 어느 때에 어느 메타 문자를 사용하는 지도 감을 잡았다. ~~며칠 뒤면 까먹겠지.~~

그래도 언젠가 찾아 보면서 한다 해도 감이 바로 잡혀 금방 써 먹을 수 있을 것 같다.

정규 표현식은 JavaScript에서 마법 같은 도구이다. 엄청 단순하게 해주는 마법 도구이다. 문자열을 조작하고 원하는 데이터를 쉽게 추출할 수 있고, 생각하지 못했던 정확한 패턴을 찾아낼 수 있다. 물론 그것을 하기 위해서는 개발자의 솜씨가 필요하겠지만..

긴 글 읽어 주셔서 감사하고 참고한 사이트가 많아 레퍼런스를 남긴다. 똑똑한 분들 정말 대단하고 감사합니다❗️

그럼 빠이💫

---

📑 **레퍼런스**

- [**[REGEXPER]**](https://regexper.com/)
- [**[MDN]**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [**[ryuryu.log]**](https://velog.io/@purplew/Javascript-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D)
- [**[poiemaweb]**](https://poiemaweb.com/js-regexp)
- [**[정규 표현식]**](https://ko.wikipedia.org/wiki/%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D)
- [**[HAMA 블로그]**](https://hamait.tistory.com/342)
