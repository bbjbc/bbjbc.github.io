---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 1일차"
categories: [udemy-sniperfactory, react]
tags:
  [
    유데미,
    udemy,
    웅진씽크빅,
    스나이퍼팩토리,
    인사이드아웃,
    미래내일일경험,
    프로젝트캠프,
    부트캠프,
    React,
    리액트프로젝트,
    프론트엔드개발자양성과정,
    개발자교육과정,
  ]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 일흔 다섯 번째 포스팅

안녕하세요! 일흔 다섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 1일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ OT

스나이퍼팩토리와 유데미가 공동 주최한 프로젝트 캠프에 참여하게 됐다. 무엇보다도 리액트 프로젝트 경험을 쌓고 싶어 지원했다.

OT 시간에는 프로그램 설명 및 팀프로젝트를 위한 기업이 소개되었다.

![image](https://github.com/user-attachments/assets/474a4308-208a-4ba5-a286-b5d77039eba5) <br />

`kt`, `두꺼비세상`, `펄핏`, `테라파이` 총 4개의 기업이 참여하여 각 기업의 프로젝트 소개를 들었다. 그리고나서 오후 12시에 선착순으로 참여하고 싶은 기업 프로젝트를 선택하는 구글 폼을 제출하였다.

가장 끌리는 것은 `kt`였다. 무엇보다도 백엔드 데이터와, api를 풍부하게 제공해준다는 말에 혹한 것 같다.

또한, `kt`에서 하려고 하는 프로젝트가 [**KT Wiz 야구 정보 제공 웹사이트 제작**](https://we-are-kt-wiz.vercel.app/)이었는데 이것은 최근에 이미 개인 프로젝트를 통해 개발한 경험이 있다.

하지만 데이터의 부족으로 더 많은 기능을 구현하지 못한 것이 아쉬움에 남았다. 그래서 KT 기업 팀프로젝트에 1순위로 지원했다... 결과는 내일 나오는데 제발 KT 하고 싶다 ~

배치된다면 빵빵한 데이터를 통해 더 많은 화면과 UI/UX 개선에 최선을 다할 것이다.

![image](https://github.com/user-attachments/assets/8c5a0676-31fa-42c4-8707-072fa4e28b3b) <br />

---

# 2️⃣ JavaScript ES6 Core

오전 시간에 필요 제출 서류를 제출하고 기업 설명을 들으니 빠르게 끝이 났다.

점심을 먹은 후 오후부터는 '수코딩'님의 강의가 시작되었다. 아래는 강의를 들으며 정리한 내용이다. 급하게 정리하느라 올바르지 못한 내용이 있을 수 있으니 보시는 분은 지적 바랍니다.

## 🔗 let and const

- 재할당이 필요하면 `let`
- 재할당이 필요없으면 `const`
- let→변수/const→상수 → 잘못 배운거임.

> **`let` 사용 예시**

```js
let num = 10;
num = 20;
```

> **`const` 사용 예시**

```js
const name = "boongranii";
// name = "boongraniiiii" // error
```

<br />

## 🔗 함수

### ➰ 함수 선언문

```js
function printHello() {
  console.log("Hello");
}
```

### ➰ 함수 표현식

함수 표현식 → 표현식? → 값으로 평가될 수 있는 식임

```js
// 10 + 20 -> 30
const printHello = function () {
  console.log("Hello");
};

printHello();
```

### ➰ 화살표 함수

```js
const printHello = () => {
  console.log("Hello");
};
```

```js
function func(cb) {
  cb();
}

func(() => {
  console.log("Hello222");
});

func(function () {
  console.log("Hello333");
});

const arrowFunc = function () {
  console.log("Hello444");
};

func(arrowFunc);
```

위 4개의 함수는 다 같은 맥락이다.

```js
const add = function (a, b) {
  return a + b;
};

const add2 = (a, b) => a + b;
```

위처럼 화살표 함수를 쓸 수 있음!

<br />

## 🔗 비구조화 할당

```js
const fruits = ["apple", "banana", "cherry"];
console.log(fruits[0]);
console.log(fruits[1]);
console.log(fruits[2]);
```

이렇게 하면 너무 복잡.

```js
const [a, b, c] = ["apple", "banana", "cherry"];
console.log(a); // apple
console.log(b); // banana
console.log(c); // cherry

const { name, age, gender } = {
  name: "병찬",
  age: 25,
  gender: "male",
};
console.log(user.name);
console.log(user.age);
console.log(user.gender);
```

비구조화 할당할 때 중요한 것은 배열이 들어가는 인덱스 자리가 중요하다.

`user` 예시를 보면 객체의 비구조화 할당에서 중요한 점은 객체 할당할 때 키 값과 동일해야 한다. (순서는 상관없음. 이름 일치 여부에 따라 가능함) <br />
또한, 비구조화 할당에서 필요한 것만 사용해도 된다 ~

<br />

## 🔗 spread 연산자 (...)

```js
const arr1 = [1, 2, 3];
const arr2 = arr1;

arr1.push(4);
console.log(arr1); // [1, 2, 3, 4]
console.log(arr2); // [1, 2, 3, 4]
console.log(arr1 === arr2); // true
```

배열은 `참조 자료형`에 속하기 때문에 저렇게 `arr1`과 `arr2`가 같은 값을 보이는 것임.

> 🎶 **참고** <br />
> 기본자료형에는 `문자`, `숫자`, `논리형`, `undefined`, `null`, `symbol`이 있음. <br />
> 참조 자료형에는 `객체`, `배열`, `함수`가 있음.

배열의 할당된 값의 정의된 주소값이 javascript 어딘가에 정의가 되어있음.

```js
const arr2 = arr1;
```

여기에서는 같은 주소값을 바라보고 있기 때문에 `true`를 출력해내는 것임. 복사한 배열이 원본 배열과 같이 변경되는 형태로 복사되는 것을 `얕은 복사(shallow copy)`라고 한다.

```js
const arr2 = [...arr1];

console.log(arr1 === arr2); // false
```

이렇게 해서 `깊은 복사`를 해줄 수 있다. 이제 주소값은 달라진다. 리터럴 표기법으로 새로운 배열을 만들어 주었기 때문에 달라진다. 새로운 배열을 만들고 그 안에 배열을 바꿔준 셈임. (단지, 스프레드 연산자 때문에 깊은 복사가 된 것이 아니다.)

```js
const num1 = [1, 2];
const num2 = [3, 4];
const combinedArr = [num1[0], num1[1], num2[0], num2[1]];
console.log(combinedArr); // [1, 2, 3, 4]
```

두 배열을 합치는 방법은 가장 단순하게 위와 같이 하면 됨. 하지만 너무 길고 답답하잖아.

```js
const num1 = [1, 2];
const num2 = [3, 4];
const combinedArr = [...num1, ...num2];
console.log(combinedArr); // [1, 2, 3, 4]
```

이렇게 해주면 간단하게 병합을 할 수 있다. → 병합 패턴

이 모든 것이 배열에 적용되고 객체에도 마찬가지로 적용된다.

```js
const user1 = { name: "병찬" };
const user2 = user1;
console.log(user1); // { name: '병찬' }
console.log(user2); // { name: '병찬' }
```

객체가 정의되어 있는 주소 값을 `user1`의 값을 `user2` 값에 준다는 것이다. 그러므로 똑같은 결과가 나오는 것이다. 기본적으로 참조 자료형은 얕은 복사가 되니까 !

```js
const user1 = { name: "병찬" };
const user2 = { ...user1 };

user1.name = "봉만";
console.log(user1); // { name: '봉만' }
console.log(user2); // { name: '병찬' }
```

배열과 동일하게 객체에도 스프레드 연산자가 쓰인다.

```js
const obj1 = { name: "병찬" };
const obj2 = { age: 25 };

const combined1 = { name: obj1.name, age: obj2.age };
const combined2 = { ...obj1, ...obj2 };

console.log(combined1); // { name: '병찬', age: 25 }
console.log(combined2); // { name: '병찬', age: 25 }
```

객체도 배열과 마찬가지로 스프레드 연산자를 통해 병합이 가능하다.

객체의 value 값이 동일할 경우에는 후자에 쓰이는 것이 전자를 덮어 씌워버리게 된다.

```js
function add(...rest) {
  return rest.reduce((acc, curr) => acc + curr, 0);
}
const result = add(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
console.log(result); // 55
```

함수의 매개변수를 통해서도 위와 같이 사용할 수도 있다.

---

# 3️⃣ TypeScript로 떠나보자

JavaScript는 동적 언어이다. 런타임 시에 타입이 결정이 된다는 말이다.

```js
function getSize(arr) {
  console.log(arr.length);
}
getSize([1, 2, 3]); // 3
getSize(10); // undefined
```

JavaScript는 실행 해보지 않는 이상 에러가 있는지 없는지 모르는거라는 거야. → 런타임에 에러가 발생하기 때문이지. (실제 코드를 돌려봐야 에러인지 아닌지 알 수 있으니까!) → 개발자도 파악하지 못할 가능성이 있다. → 이것이 JavaScript의 단점 → 이 동적인 언어를 정적으로 만들기 위해 **TypeScript 등장!**

TypeScript는 JavaScript의 에러 방지를 막기 위해 등장한 것이다. 런타임이 아닌 **컴파일**을 하는 과정에서 에러가 발생하기 때문에 개발자 경험에도 우세하다고 볼 수 있다.

JavaScript를 조금 더 안정성 있게 사용하고자 한 것이 TypeScript이다. **JavaScript의 값에 Type을 지정하는 언어가 TypeScript인 것이다.**

결국 TypeScript는 JavaScript를 포함하고 있다는 것이다. 그렇기 때문에 TypeScript를 먼저 접하는 사람은 JavaScript에 대하여 정확히 잡혀 있지 않을 수도 있다는 것이다.

<br />

## 🔗 TypeScript 실행을 위한 발판

```js
npm init -y
```

를 통해서 패키지 파일을 초기화 시켜준다.

```js
npm install typescript
```

위를 터미널에 입력하여 typescript를 설치해준다.

`dependencies`와 `devDependencies`는 NodeJs에서 패키지 관리를 할 때 사용하는 package.json 파일 내의 두 가지 주요 섹션이다.

`dependencies` → NodeJs에서 관리하는 의존이다. 애플리케이션이 동작하는데 필요한 패키지들이 포함된다. 즉, 애플리케이션이 배포되거나 운영 환경에서 실행될 때 필수적인 라이브러리들이 이곳에 지정된다. (ex. express)

`devDependencies` → 개발모드에서만 사용하면 되는 것들을 몰아 넣는 것이다. 테스트, 빌드 도구, 코드 린터와 같은 개발 도구들이 주로 여기에 포함된다. 배포된 애플리케이션이 실행될 때는 필요하지 않은 패키지들이며, 일반적으로 개발 환경에서만 사용된다.

간단히 말해서 `dependencies`는 프로덕션 환경에서 필요한 패키지들을, `devDependencies`는 개발 환경에서만 필요한 패키지들을 포함하는 것이다.

어떻게 구분하면 되는지는 공식사이트에 검색하여 설치하는 것이 가장 정확하다....

```ts
let uname: string = "Boongranii";
console.log(uname);
```

이러한 코드를 `tsc index.ts`를 터미널에 치면 `index.js` 파일에 컴파일 되어 나타나게 된다.

```js
var uname = "Boongranii";
console.log(uname);
```

CLIENT → 웹브라우저에서 `html`, `css`, `js`, `어셈블리어` 등이 작동한다. `scss`, `sass`, `ts`, `pug` 등과 같은 것들은 웹브라우저에서 컴파일(Compile) 과정을 거치게 된다.

하나의 확장자를 웹 브라우저가 읽어내도록 하는 것을 **컴파일** 과정이라고 하고, 이것이 가능하게 하는 것이 **컴파일러**이다.

```js
tsc index.ts
```

를 입력하지 않고 run 버튼을 통해 TypeScript를 러닝하기 위해서는 `settings.json` 파일에 들어간다.

```json
"code-runner.executorMap": {
  "typescript": "node_modules/.bin/ts-node"
},
```

위 코드를 추가하면 된다. 만약 윈도우에서 글자가 깨진다면 아래와 같이 수정하면 된다.

```json
"code-runner.executorMap": {
  "typescript": "node -r ts-node/register"
},
```

참고로 위를 진행하기 전에 아래를 설치해야 한다.

```js
npm install -D ts-node
```

이제 편리하게 ts 파일에서도 버튼 하나만으로 실행되는 것을 확인할 수 있다.

<br />

## 🔗 Type 지정 방법

```js
let/const 변수명: 타입 = 값;
```

이런 형태로 선언 가능하다.

```ts
let uname: string = "홍길동"
let num: number = 10
let isTrue: boolean: true
let nul: null = null
let und: undefined = undefined
let sym: symbol = Symbol("key")
```

이런 식으로 여러 가지로 타입 선언이 가능하다. 하지만 위처럼 변수에 타입을 지정했다면 변수의 타입을 바꾸려하면 밑줄이 발생하게 된다. TypeScript는 정적 언어이기 때문에 미리 에러의 원인을 파악 가능하다!

```ts
let user: { name: string; age: number; isTrue: boolean } = {
  name: "boongranii",
  age: 25,
  isTrue: true,
};
```

위 예제는 객체에서 타입을 선언하는 방법이다.

```ts
let arr: number[] = [1, 2, 3];
arr.push(10); // number를 삽입하는건 올바름.
arr.push("a"); // 불가능함. type error 발생

let arr2: string[] = ["a", "b", "c"];

let arr3: boolean[] = [true, false];
let arr4: null[] = [null, null];
let arr5: { name: string; age: number } = [
  { name: "병찬", age: 25 },
  { name: "봉만", age: 25 },
];

let arr6: [number, string, boolean, null, { name: string; age: number }] = [
  1,
  "a",
  true,
  null,
  { name: "병찬", age: 25 },
];

// 객체 안에 배열이 존재하면 이와 같이 선언하면 된다.
let obj1: { name: string; numberArr: number[] } = {
  name: "boongranii",
  numberArr: [1, 2, 3],
};
```

이런식으로 배열은 배열이지만 그 안의 요소에 따라 배열 앞에 타입을 선언해주면 된다.

만약 배열의 타입이 섞여 있다면 `튜플`을 통해서 타입을 지정해주면 된다. → 인덱스 별로 타입을 지정해 주는 방식!

결론, 요소에 따라서 어떤 타입인지 지정해주면 된다!

<br />

## 🔗 함수 Type 지정 방법

매개변수와 반환 값의 타입을 지정해주면 된다.

```ts
function add(n1: number, n2: number): number {
  return n1 + n2;
}
add(10, 20);
```

매개변수에도 타입을 지정하며 반환값에도 타입을 지정해야 한다.

**함수 표현식을 선언하는 타입은 2가지가 있다.**

1. 함수 선언문처럼 매개변수와 반환값의 타입을 지정하는 방법
2. 변수에 타입을 지정하는 방법

```js
const add: (n1:number, n2:number) => (n1, n2) => n1 + n2;
const add2 = (n1: number, n2: number): number => n1 + n2
const add3: (n1: number, n2: number) => number = function (n1, n2) {
  return n1 + n2;
};
add(10, 20);

// 함수 선언문 → 아무것도 반환하지 않을 때는 void를 사용
(): void {
  console.log("Hello");
}

const printHello = (): void => {
  console.log("Hello");
};

const printHello: () => void = () => {
  console.log("Hello");
};
```

위 예제는 변수에 타입을 지정하는 방법이다.

두 방법 중 편한 방법을 사용하면 된다.

**여러 예제가 있었지만 지금까지 위에서 했던 것들 복습이었다.** 단순히 객체면 객체 주고 타입 선언해주고, 배열이나 함수면 타입에 맞게 선언을 해주면 끝이다.

<br />

## 🔗 함수 Type 연습문제

다음은 위 내용을 배우고 나서 해결했던 연습문제였다. 간단했다.

- **두 숫자를 더하고 결과를 반환하는 함수**

```js
function add(a: number, b: number): number {
  return a + b;
}
```

- **문자열을 받아서 문자열을 반환하는 함수**

```js
function result(str: string): string {
  return str;
}
```

- **불리언 값을 인자로 받아서 아무것도 반환하지 않는 함수**

```js
function result(value: boolean): void {
  console.log(value);
}
```

- **문자열에서 가장 긴 단어를 반환하는 함수**

```js
function findLongestWord(words: string): string {
  const word = words.split(" ");
  let longestWord = "";

  for (const w of word) {
    if (w.length > longest.length) {
      longestWord = w;
    }
  }

  return longestWord;
}
```

- **100부터 999까지 암스트롱 수를 구하시오.** (암스트롱의 수는 세 자리의 정수 중에서 각 자리의 수를 세 세제곱의 합과 자신이 같은 수임)

```js
function getArmstrongNumbers(): number[] {
  const armstrongNumber: number[] = [];

  for (let i = 100; i <= 999; i++) {
    const digits = i.toString().split("").map(Number);
    const sum = digits.reduce((acc, digit) => acc + Math.pow(digit, 3), 0);

    if (sum === i) {
      armstrongNumber.push(i);
    }
  }

  return armstrongNumber;
}

console.log(getArmstrongNumbers());
```

<br />

## 🔗 타입 오퍼레이터

### ➰ Union Type

```js
let strAndNum: number | string = 10;
strAndNum = "Hello";

let user: {
  name: string,
  money: number | null,
} = {
  name: "Bong",
  money: 0, // null
};

const getElements = (arr: number[] | string[]) => {
  return arr[0];
};

console.log(getElements([1, 2, 3])); // 1
console.log(getElements(["a", "b", "c"])); // 'a'
```

JavaScript에서 `or(||)` 연산자는 피연산자 중에서 하나만 참이면 참을 반환하는 것이다. <br />
`and(&&)` 연산자는 피연산자 모두 참이어야 참을 반환하는 것이다.

위 예제처럼 유니언 타입을 사용하면 둘 중에 하나만 만족하면 된다는 것이다.

### ➰ Intersection Type

```js
const user: { name: string } & { age: number } = {
  name: "Bong",
  age: 25,
};
```

이렇게 `&`을 통해서 모두의 타입을 만족해야 한다.

```js
const user: {
  name: string,
  age: number,
  gender: string,
  printInfo: () => void,
} = {
  name: "Bong",
  age: 25,
  gender: "male",
  printInfo() {
    console.log(`Name: ${this.name}, Age: ${this.age}, Gender: ${this.gender}`);
  },
};
```

위 예제보다 많은 객체 안에 값이 있으면 타입 선언하는 것이 너무 길어진다. 그래서 사용하는 것이 `interface`이다.

```js
interface IUser {
  name: string;
  age: number;
  gender: string;
  printInfo: () => void;
}
```

위와 같이 우리만의 타입을 지정해두면

```js
const user: IUser = {
  name: "Bong",
  age: 25,
  gender: "male",
  printInfo() {
    console.log(`Name: ${this.name}, Age: ${this.age}, Gender: ${this.gender}`);
  },
};
```

이렇게 가독성 있고 더욱 짧아진 코드를 볼 수 있게 된다. 또 다른 장점은 재사용성도 높아질 수 있다는 것이다. 결국 유지보수에 용이하다 ~ 인터페이스를 잘 사용하자 !

하지만 `interface`는 객체를 지정할 때만 사용하는 것은 올바르지 않은 말이다. 단순하게 나만의 타입 룰을 만들 때 사용한다고 생각하면 되는 것이다.

---

# 4️⃣ 느낀점

처음으로 이런 프로그램을 수강하였다. 처음으로 수강해서 그런지 9-18 정말 빡셌다. 하지만 정해진 시간에 좋은 강사님의 수업을 수강하며 내가 정확히 몰랐던 TypeScript의 문법을 배우니 흥미로웠다.

앞으로도 TypeScript에 관한 강의가 많을텐데 항상 기초가 중요한 것 같다. 기초를 더 열심히 다지고 배워서 내 것으로 만들어야겠다.

![image](https://github.com/user-attachments/assets/5ccaca63-6b0c-4458-89d9-448abe33d6e8) <br />

저기 단상 맨 앞에 노랭이가 나다. 정말 귀엽다. 병아리 같다. ㅋ😁

남은 2주 화이팅 쿄쿄 💜😊

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
