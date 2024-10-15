---
title: "[패스트캠퍼스] 김민태의 프론트엔드 데브캠프 2기 세 번째 섹션: JavaScript"
categories: [dev-camp, react]
tags: [FE, 부트캠프, 패스트캠퍼스, 패스트캠퍼스데브캠프, 프론트엔드, 김민태]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 아흔 두 번째 포스팅

안녕하세요! 아흔 두 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 아흔💫

오늘의 포스팅 내용은 **[패스트캠퍼스] 김민태의 프론트엔드 데브캠프 2기 세 번째 섹션: JavaScript**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ JavaScript 들여다보기

JavaScript는 웹 개발의 핵심 언어로 다양한 기능과 개념을 포함하고 있다.

자, JavaScript의 기본 개념을 알아보도록 하자.

## 🔥 변수와 상수

JavaScript에서 데이터를 저장하는 방법에는 3가지가 있다.

```js
const PI = 3.14159; // 상수
let count = 0; // 변수
var oldStyle = "oldboy"; // 옛날 방식 (권장 X)
```

- `const`는 상수를 선언한다. 한 번 할당하면 값을 변경할 수 없다.
- `let`은 변수를 선언한다. 값을 재할당할 수 있다.
- `var`는 오래된 방식으로 현재는 사용을 권하지 않으며 거의 사용하지 않는다.

위 3가지는 모두 예약어로 JavaScript에서 특별한 의미를 가지며 사용된다.

## 🔥 세미콜론 사용

JavaScript에서는 각 명령문 끝에 세미콜론(`;`)을 사용한다.

```js
let x = 5;
let y = 10;
let sum = x + y;
```

최근에는 세미콜론을 생략하는 경우가 많아졌다. 팀프로젝트를 하며 코드 컨벤션을 맞추기 위해 세미콜론을 빼고 코딩하는 방법도 존재한다.

하지만 명시적으로 사용하는 것이 안전하다. 브라우저가 자동으로 세미콜론을 삽입할 때 예기치 않은 문제가 발생할 수 있기 때문이다.

## 🔥 함수

함수는 재사용이 가능한 코드 블록이다.

```js
function calculateArea(width, height) {
  return width * height;
}

let area = calculateArea(5, 3); // 결과: 15
```

`function` 키워드를 사용한다. 화살표 함수는 추후 설명하도록 하겠다.

함수 내부에서 선언된 변수는 함수 외부에서 접근할 수 없다. (스코프) 또한, 함수 자체도 값으로 취급되어 `area`에 값을 할당할 수 있는 것이다.

## 🔥 화살표 함수

이는 ES6에서 도입되어 더욱 간결한 문법을 제공한다.

```js
const greet = (name) => `안녕하세요, ${name}님!`;

console.log(greet("병찬")); // 출력: 안녕하세요, 병찬님!
```

함수 이름이 없다. 즉, 익명 함수이다. 또한, 위 예시처럼 인자가 하나일 때는 괄호를 생략할 수 있다.

함수 본문이 한 줄일 때는 중괄호와 `return` 키워드가 생략이 된다. 따라서, 위처럼 엄청나게 간결하게 함수를 작성할 수 있다.

## 🔥 즉시 실행 함수

```js
(function () {
  console.log("이 함수는 즉시 실행됩니다!");
})();
```

위 함수는 한 번만 실행되어야 하는 초기화 코드에 사용된다.

## 🔥 이벤트 처리

웹 페이지에서 사용자 상호작용을 처리할 때 이벤트 리스너를 사용한다.

```js
const button = document.querySelector("#myButton");
button.addEventListener("click", function () {
  alert("버튼이 클릭되었습니다!");
});
```

위 코드는 DOM(Document Object Model)을 사용하여 HTML 요소를 선택하고 이벤트를 처리한다.

## 🔥 비교 연산자

아래는 주로 사용되는 비교 연산자들이다.

- `===`: 엄격한 동등 비교 (값과 타입이 모두 같아야 한다.)
- `!==`: 엄격한 부등 비교
- `>`, `<`, `>=`, `<=`: 크기 비교

## 🔥 반복문

```js
// for 루프
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// while 루프
let count = 0;
while (count < 3) {
  console.log(`Count: ${count}`);
  count++;
}

// do-while 루프
let x = 0;
do {
  console.log(x);
  x++;
} while (x < 3);
```

각 반복문은 특정 상황에 맞게 사용하면 된다.

---

# 2️⃣ JavaScript 데이터 타입

JavaScript에서 데이터 타입은 크게 원시 타입(Primitive types)와 참조 타입(Reference types)로 나눌 수 있다.

## 👝 원시 타입 (Primitive Types)

원시 타입은 불변성을 가지고 값 자체가 메모리에 저장된다.

> 🍪 **숫자(Number)**

JavaScript는 정수와 실수를 구분하지 않고 모두 Number 타입으로 취급한다.

```js
let integer = 42;
let float = 3.14159;
let scientific = 2.998e8; // 과학적 표기법
let binary = 0b1010; // 2진수 (10)
let octal = 0o744; // 8진수 (484)
let hexadecimal = 0xff; // 16진수 (255)
```

위와 같은 예시가 있고 특수한 숫자값인 `Infinity`, `NaN` 등이 존재한다.

> 🍪 **문자열(String)**

문자열은 작은따옴표(`'`), 큰따옴표(`"`), 또는 백틱(`'`)으로 둘러싸인 텍스트이다.

```js
let singleQuote = "Hello";
let doubleQuote = "World";
let backticks = `Hello, ${singleQuote}!`; // 템플릿 리터럴
```

템플릿 리터럴을 사용하면 문자열 안에 표현식을 삽입할 수 있고, 여러 줄의 문자열을 쉽게 작성할 수 있다.

```js
let multiLine = `
  이 곳에는
  여러 줄을
  이렇게
  작성 가능해요
  ㅎㅎ
`;
```

> 🍪 **불리언(Boolean)**

불리언 타입은 `true` 또는 `false` 두 가지 값만 가질 수 있다.

```js
let isTrue = true;
let isFalse = false;
```

> 🍪 **Undefined**

변수가 선언되었지만 값이 할당되지 않은 상태를 나타낸다.

```js
let undefinedVar;
console.log(undefinedVar); // undefined
```

> 🍪 **Null**

의도적으로 '비어있음' 또는 '알 수 없음'을 나타내는 값이다.

```js
let nullVar = null;
```

> 🍪 **Symbol**

고유하고 변경 불가능한 원시 값이다. 주로 객체 프로퍼티의 키로 사용된다.

```js
let sym1 = Symbol("id");
let sym2 = Symbol("id");
console.log(sym1 === sym2); // false
```

## 👝 참조 타입 (Reference Types)

JavaScript에서 원시 타입을 제외한 나머지는 참조타입이라 할 수 있다.

참조 타입은 메모리의 주소를 참조하며 크기가 동적으로 변할 수 있다.

> 🍪 **객체(Object)**

키-값 쌍의 컬렉션이다. 중괄호 `{}`로 정의한다.

```js
let person = {
  name: "병찬",
  age: 25,
  isStudent: true,
};

console.log(person.name); // "병찬"
console.log(person["age"]); // 25
```

> 🍪 **배열(Array)**

순서가 있는 데이터의 집합이다. 대괄호 `[]`로 정의한다.

```js
let fruits = ["apple", "banana", "orange"];
console.log(fruits[1]); // "banana"
console.log(fruits.length); // 3
```

또한, 배열은 다양한 타입의 요소를 포함할 수 있다.

```js
let mixed = [1, "two", { name: "three" }, [4, 5]];
```

> 🍪 **함수(Function)**

JavaScript에서 함수도 객체의 한 종류이다.

```js
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("병찬")); // "Hello, 병찬!"
```

## 👝 타입 확인

`typeof` 연산자를 사용하여 변수의 타입을 확인할 수 있다.

```js
console.log(typeof 42); // "number"
console.log(typeof "hello"); // "string"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof {}); // "object"
console.log(typeof []); // "object"
console.log(typeof function () {}); // "function"
```

## 👝 타입 변환

JavaScript는 동적 타입 언어이므로, 필요에 따라 자동으로 타입을 변환한다. 이를 암시적 형변환이라 한다.

```js
console.log("5" + 3); // "53" (문자열 연결)
console.log("5" - 3); // 2 (숫자로 변환 후 뺄셈)
```

위와 달리 명시적으로 타입을 변환할 수도 있다.

```js
let num = Number("5"); // 문자열을 숫자로
let str = String(42); // 숫자를 문자열로
let bool = Boolean(1); // 1은 true로 변환됨
```

---

# 3️⃣ 마무리

JavaScript의 기본적이고 거시적으로 학습하였다. 이를 토대로 다음 팀프로젝트에서 JavaScript를 사용하여 잘 진행할 수 있을 것이다.

팀 프로젝트를 통해 팀원들과 협업하며 서로의 지식을 공유하고, 실제 문제를 해결해 나가는 과정에서 더 큰 성장이 있을 것이다. 화이팅💪
