---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 2일차"
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

# 일흔 여섯 번째 포스팅

안녕하세요! 일흔 여섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 2일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ TypeScript로 떠나보자

어제 내주신 TypeScript로 간단한 함수 구현하기 과제를 시작으로 2일차가 시작되었다. JavaScript에 대한 메서드를 조금 알기만 한다면 풀 수 있는 문제였다.

```ts
const findLongestWord = (str: string): string =>
  str.split(" ").sort((a, b) => b.length - a.length)[0];

console.log(findLongestWord("Hello Worlddd")); // Worlddd
console.log(findLongestWord("asda ad sad asd asd asddsadfsd")); // asddsadfsd
```

나는 위와 같이 풀지 않았지만 위처럼 `sort()` 메서드를 사용해서 간단하게 해결할 수 있었다.

## 🔗 인터페이스

어제 마무리 시간에 인터페이스에 대해서 조금 알아보았다. 조금 더 알아보러 가자.

### ➰ 인터페이스 병합

```ts
interface IUser {
  name: string;
  age: number;
}

interface IUser {
  email: string;
}

const user: IUser = {
  name: "Bong",
  age: 20,
  // email 속성이 누락됨 → 오류 발생
};

interface IAdd {
  (n1: number, n2: number): number;
}

const add: IAdd = (n1, n2) => n1 + n2;
```

TypeScript에서 동일한 이름의 인터페이스가 여러 번 선언되면 이를 자동으로 병합한다. 그래서 각 인터페이스에 정의된 모든 속성이 하나의 인터페이스로 합쳐지게 된다.

위 예시를 보면 `IUser`라는 인터페이스가 두 번 선언되었을 때, 첫 번째 선언에는 `name`과 `age`를 포함하고, 두 번째 선언에는 `email` 포함됐다. 이 경우에 TypeScript는 이 두 인터페이스를 하나로 합쳐서 `name`, `age`, `email` 세 가지 속성을 모두 갖는 `IUser` 인터페이스를 만들어준다.

즉, 병합 덕분에 `IUser`라는 인터페이스는 `name`, `age`, `email` 세 가지 속성을 전부 갖게 되었기 때문에 `user` 객체에 `email` 속성이 빠져있을 때 오류가 발생하게 된다.

### ➰ readonly

```ts
interface IUser {
  name: string;
  // readonly name: string; → 수정 불가능
  // 수정 시 오류 아래 콘솔 단계에서 오류가 발생한다.
  age: number;
}

const user: IUser = {
  name: "Bong",
  age: 25,
};

user.name = "Boongranii";
console.log(user); // { name: 'Boongranii', age: 25}
```

TypeScript에서는 객체의 특정 속성을 수정하지 못하도록 막을 수 있는 방법으로 `readonly` 키워드를 사용한다. 이 키워드를 속성 앞에 붙이면, 해당 속성은 객체가 생성된 이후에 더 이상 수정이 불가능하다.

`readonly` 키워드는 객체의 특정 속성을 불변으로 만들기 때문에 실수로 값을 변경하는 것을 방지할 수 있다. 이 기능은 객체가 생성된 이후 해당 속성이 절대 변경되지 않아야 하는 경우에 유용하다.

### ➰ 상속

```ts
interface IUser {
  name: string;
  age?: number;
}

interface Job extends IUser {
  jobName: string;
}

const userJob: Job = {
  name: "Bong",
  jobName: "developer",
};
```

TypeScript에서는 인터페이스를 상속하여 원래의 인터페이스의 속성을 확장할 수 있다. 이렇게 하면 코드의 재사용성도 높일 수 있고, 구조적으로 체계적인 타입을 정의 가능하다.

`extends` 키워드를 사용해서 `IUser` 인터페이스를 상속한다. 이렇게 하면 `Job` 인터페이스는 `IUser`의 모든 속성을 그대로 상속받게 되며, 여기에 추가로 `jobName`이라는 새로운 속성을 추가 정의 가능하다.

`userJob` 객체는 `Job` 인터페이스를 기반으로 만들어졌으며, `name`과 `jobName` 속성을 포함하고, 선택적인 `age` 속성은 생략할 수 있다.

이렇게 인터페이스 상속을 통해 재사용성을 높이고 새로운 속성을 유연하게 추가 가능하다는 장점이 있다.

<br />

## 🔗 타입

### ➰ 타입 추론

```ts
let num = 10;
num = 20;

let str = "A";
str = "B";
```

TypeScript는 변수의 초기값을 기반으로 타입을 자동으로 추론하는 기능이 있다. 이 기능 덕분에 타입을 명시적으로 선언하지 않아도, 변수의 타입을 자동으로 결정한다. VSCode에서 커서를 `num`에 올려보면 `number`가 나타나는 것을 확인할 수 있다.

타입을 명시적으로 선언하는 것과 타입을 추론에 맡기는 것은 각각 장단점이 있으며, 둘 중 어떤 방법을 사용할지는 취향과 상황에 따라 다를 수 있다.

간단한 코드나 직관적인 경우에는 타입 추론을 활용하는 것이 좋고, 복잡한 코드나 협업 환경에서는 타입을 명시적으로 선언해서 가독성을 높이는 것이 유리할 수도 있다.

### ➰ 타입 별칭

```ts
type User = {
  name: string;
  age: number;
};
```

`interface`에서 `type`으로 바꾸고 값으로 할당만 해주면 별로 차이가 없어보인다.

```ts
interface IUser {
  name: string;
  age: number;
}

const user: IUser = {
  name: "Bong",
  age: 25,
};

type TUser = {
  name: string;
  age: number;
};

const user2: TUser = {
  name: "Bong",
  age: 25,
};
```

> **interface 호버 시**

![image](https://github.com/user-attachments/assets/27dafe42-98d4-498d-b1a2-e3c293d1a723)

> **type 호버 시**

![image](https://github.com/user-attachments/assets/960499a9-298e-4b6f-9ca5-86827b528a85)

인터페이스에서 배운 문법 개념이 똑같이 들어간다. 하지만 차이는 무조건 존재한다. `type`은 우선 병합이 되지 않는다.

```ts
type TUser = {
  [key: string]: string | number;
};

type TUser = {
  isStudent: boolean;
};
```

![image](https://github.com/user-attachments/assets/626586a9-ab08-42d0-82e2-cc360589e09a) <br />

인터페이스와는 다르게 중복이 된다고 오류가 뜬다. 그러므로 자동 병합이 되지 않는다. 또한, `상속(extends)`도 되지 않는다. 병합이나 상속을 구현하기 위해서는 `인터섹션(&)` 연산자를 통해서 가능하다.

```ts
type TUserAndJob = TUser & { job: string };
```

위와 같이 `인터섹션` 연산자를 통해 병합을 구현할 수 있다..

|    특징     |                 인터페이스(Interface)                 |                       타입 별칭(Type Alias)                       |
| :---------: | :---------------------------------------------------: | :---------------------------------------------------------------: |
|    목적     | 겍체의 구조(속성 및 메서드)를 정의하는 데 주로 사용됨 |      다양한(기본 타입, 유니온, 튜플 등)을 정의하는 데 사용됨      |
| 확장 가능성 |  `extends` 키워드를 사용하여 타 인터페이스 상속 가능  |   기존 타입을 확장할 수 없지만, 인터섹션(`&`)을 통해 조합 가능    |
|    병합     |        동일한 이름의 인터페이스는 자동 병합됨         |           동일한 이름의 타입은 병합되지 않고 오류 발생            |
|  사용 범위  |          객체 구조를 정의하는 데 주로 사용됨          | 객체 구조 외에 다양한 타입(기본 타입, 유니온 타입 등)을 정의 가능 |
|   유연성    |         주로 구조적인 형태의 타입 정의에 적합         |                더 복잡하고 다양한 타입 조합에 적합                |

### ➰ ENUM

```ts
enum Role {
  Admin, // 0
  User, // 1
  Guest, // 2
}

interface IUser {
  name: string;
  role: Role;
}

const user: IUser = {
  name: "Bong",
  role: Role.Admin,
};
```

`enum`은 TypeScript에서 관련된 값들의 집합을 이름으로 정의할 때 사용하는 특별한 데이터 타입이다. `enum`을 사용하면 코드의 가독성을 높이고, 특정 값들의 집합을 명확히 표현 가능하다.

끝에 보이는 것처럼 `Role.Admin`은 `user` 객체를 생성할 때, `role` 속성에 `Role` 열거형에서 정의된 `Admin` 값을 할당한다.

위 `enum` 방법도 있지만 `type`에서 유니언 타입을 사용해도 무관하다. 개인 취향이라고 생각한다.

<br />

## 🔗 제네릭

```ts
function getSize<T>(arr: T[]) {
  return arr.length;
}

console.log(getSize<number>([1, 2, 3])); // 3
```

제네릭은 함수, 클래스, 인터페이스, 또는 타입을 정의할 때, 다양한 타입에 대해 재사용 가능한 코드를 작성할 수 있도록 도와주는 기능이다. 이를 사용하면 구체적 타입을 함수나 클래스가 호출될 때 결정할 수 있게 되어, 보다 유연하고 타입 안전한 코드를 작성 가능하다!

`getSize`는 제네릭 타입 `T`를 사용해서 다양한 타입의 배열을 처리할 수 있게 한다. 여기서 `T`는 임의의 타입을 나타내며, 원하는 문자로 바꿔도 된다.

`getSize(<number>([1,2,3]))` 호출 시, `<number>`를 제네릭 타입으로 지정해서 `T`가 `number`로 대체된다. 따라서 `arr`은 `number[]` 타입이 되며 함수는 배열의 길이를 반환하게 된다.

제네릭의 본질은 꺽쇠(`<>`) 안에 있는 타입을 원하는 타입으로 치환하는 것이라고 볼 수 있다.

꺽쇠(`<>`)의 위치는 함수에서는 제네릭 타입 매개변수를 꺽쇠(`<>`) 안에 넣어 함수 이름 뒤에 위치시키고, 타입에서는 제네릭을 타입 이름 뒤에 붙여 사용한다.

```ts
function getSize<T>(arr: T[]): number {
  return arr.length;
}
```

위는 함수에서의 제네릭 타입의 위치이다.

```ts
type TApiResponse<T> = {
  data: T;
  status: number;
};
```

위는 타입에서의 제네릭 타입의 위치이다.

결국 둘 다 이름 뒤에 위치시키면 된다.

```ts
type TApiResponse<T> = {
  data: T; // 제네릭 T를 사용해 data의 타입을 동적으로 설정
  status: number;
};

const userResponse: TApiResponse<{ id: number; name: string }> = {
  data: { id: 1, name: "Bong" }, // data의 타입이 제네릭 T로 대체됨
  status: 200,
};

const todoResponse: TApiResponse<{
  id: number;
  text: string;
  completed: boolean;
}> = {
  data: { id: 2, text: "라라라", completed: true }, // data의 타입이 제네릭 T로 대체됨
  status: 200,
};
```

이렇게 사용하면 몇 백개의 api가 있어도 각자의 `interface`나 `type`을 여러 개 사용하지 않아도 범용적으로 사용할 수 있게 된다.

```js
function filterArray<T>(arr: T[], condition: (item: T) => boolean): T[] {
  return arr.filter((item) => condition(item));
}

const numberArray = [1, 2, 3, 4, 5];
const isEven = (num: number) => num % 2 === 0;
const evenNumbers = filterArray(numberArray, isEven);
console.log(evenNumbers);
```

이런식으로 다양한 데이터 형식을 처리하면서도 일관된 구조를 유지할 수 있도록 한다.

> 🎶 **제네릭의 장점** <br />
>
> 1. **범용성**: 동일한 제네릭 타입을 사용하면 다양한 데이터 구조에 대해 일관된 방식으로 타입을 정의 가능하다.
> 2. **재사용성**: 여러 API 응답 타입에 대해 공통의 구조를 정의하면서도, 각 API의 응답 데이터에 맞게 제네릭 타입을 동적으로 설정할 수 있다.
> 3. **유연성**: API의 응답 데이터 구조가 바뀔 때, 제네릭 타입 매개변수만 수정하면 되므로, 코드 수정이 간편하다.

---

# 2️⃣ 리액트 시작하기

## 🔧 NPM, NPX, YARN이 몽데요

1. **NPM (Node Package Manage)**
   - NodeJs의 패키지 매니저로, JavaScript의 패키지와 모듈을 관리한다. 패키지를 설치하고, 업데이트하며, 종속성을 관리하는 데 사용한다.
   - 프로젝트 디렉토리 내의 `package.json` 파일을 통해 의존성을 정의하고, `npm install` 명령어로 패키지를 설치한다.
   - 싱글스레드를 사용한다.
2. **NPX (Node Package Executor)**
   - 패키지를 직접 실행할 수 있도록 돕는다. (Node Package 실행도구)
   - 패키지를 전역으로 설치하지 않고도 실행 가능하며, 특히 CLI 도구를 사용할 때 유용하다.
   - `npx create-react-app my-app` 명령어로 CRA를 실행해 새로운 React 애플리케이션을 만들 수 있다.
   - npx tsc → 로컬 패키지를 사용해서 실행 해줌(1순위)/글로벌에 있는 것을 자동으로 참조해서 실행 해줌(2순위)/네트워크에 접속해서 설치(3순위)
3. **YARN**
   - NPM의 대안으로 Facebook에서 개발한 패키지 매니저이다. 패키지 설치 속도를 개선하고, 의존성 버전을 더 안정적으로 관리하는 데 초점을 맞춘다.
   - `yarn install` 명령어로 패키지를 설치하며 `yarn.lock` 파일을 사용해 의존성 버전을 유지한다.
   - 멀티스레드로 속도가 빠르고 유연하다.
4. **PNPM (Performant Node Package Manager)**
   - 고속 패키지 매니저로 NPM과 YARN의 대안이다.
   - 효율적인 캐시 사용으로 빠른 설치가 가능하다.
   - 의존성을 격리하여 충돌 가능성을 감소시킨다.
5. **BUN**
   - 빠른 빌드와 개발 서버를 제공하여 속도를 최적화 해준다.
   - JavaScript/TypeScript 빌드 및 번들링에 대한 통합 솔루션을 제공하여 자체 빌드 도구가 존재한다.

<br />

## 🔧 패키지 버전 읽는 방법

**패키지 버전은 `MAJOR.MINOR.PATCH`를 따른다.** 필요에 따라 추가적인 프리릴리즈나 빌드 메타데이터가 포함될 수 있다.

> 🏉 **버전 형식**

- **주버전(MAJOR)**: 주요 변경 사항을 나타낸다. API의 호환성이 깨지는 변경이 있거나 대규모 변경이 있을 때 증가한다.
- **부버전(MINOR)**: 새로운 기능이 추가되지만, 이전 버전과의 호환성은 유지될 때 증가한다.
- **패치버전(PATCH)**: 버그 수정이나 작은 개선이 있을 때 증가한다.

> 🏉 **프리릴리즈와 빌드 메타데이터**

- **프리릴리즈**: 버전 뒤에 `-`와 함께 붙는 추가적인 정보이다. 이것은 해당 버전이 안정적이지 않거나 베타, 알파와 같은 개발 단계임을 나타낸다.
  - 형식: `MAJOR.MINOR.PATCH-PRERELEASE`
  - 예시: `1.1.1-beta`, `2.0.0-alpha.1`
- **빌드 메타데이터**: 버전 뒤에 `+`와 함께 붙는 추가적인 정보를 나타낸다. 버전의 의미를 변경하지 않지만, 빌드 관련 추가 정보를 제공한다.
  - 형식: `MAJOR.MINOR.PATCH+BUILD`
  - 예시: `1.1.1+20240820`, `2.0.0+build1111`

<br />

## 🔧 리액트 프로젝트 생성 방법

### 📁 CRA(create-react-app)

굉장히 고전적인 보일러플레이트이며 더 이상 권장되지 않는다.

> **npx로 설치하는 방법**

```js
npx create-react-app my-app
```

> **npm으로 설치하는 방법**

```js
npm install -g create-react-app
create-react-app my-app
```

### 📁 Vite(빁ㅌ)

> **vite 설치 방법**

```js
npm create vite@latest
```

자세한 내용은 옆 링크를 확인 바랍니당. [**vitejs**](https://github.com/vitejs/vite/tree/main/packages/create-vite)

`vite`를 설치하고

```js
npm install
npm run dev
```

를 하게 되면 서버가 구동된다.

![image](https://github.com/user-attachments/assets/a7c5891b-30d8-4d63-87e2-f609d7ecb2bb) <br />

위와 같이 깔끔하고 빠르게 실행되는 것을 확인할 수 있다.

<br />

## 🔧 리액트는 도대체 왜 사용해?

![image](https://github.com/user-attachments/assets/a14f262c-8edc-4b8b-afab-f8ae3a8020d0)([**출처**](https://npmtrends.com/@angular/core-vs-lit-vs-react-vs-solid-js-vs-svelte-vs-vue)) <br />

위 그림을 보면 React가 압도적인 것을 확인할 수 있다.

왜 이렇게 높을까 특징을 한 번 살펴봤다.

1. **컴포넌트 기반 아키텍처**
   - 모듈화 → 재사용 가능한 컴포넌트로 나누어 유지보수와 개발에 용이
   - 재사용성 → 한 번 작성한 컴포넌트를 여러 곳에서 재사용 가능
2. **가상 DOM**
   - 성능 향상 → 실제 DOM을 직접 조작하는 대신, 가상 DOM에서 변경 사항을 계산하고 최소한의 업데이트만을 통해 실제 DOM을 적용하여 성능을 향상시킴
3. **단방향 데이터 흐름**
   - 예측 가능성 → 데이터가 부모 컴포넌트에서 자식 컴포넌트로 단방향으로 흐르기 때문에, 데이터의 흐름을 예측하고 관리하기 쉬움
4. **JSX (JavaScript XML)**
   - 직관적 → HTML과 JavaScript를 함께 작성 가능한 JSX를 사용하여 UI 정의가 직관적이고 가독성을 높임
5. **상태 관리**
   - 내장 상태 관리 → 컴포넌트 내에서 상태를 쉽게 관리 가능하며 다양한 훅을 제공하여 복잡한 상태 관리가 용이
6. **SSR**
   - SEO 및 성능 개선 → NextJs와 같은 프레임워크를 사용하여 SSR을 지원하고 SEO 및 초기 로드 성능 개선 가능
7. **풍부한 생태계**
   - 라이브러리와 도구 → 커뮤니티가 굉장히 활발하며 다양한 라이브러리와 도구가 제공되어 효율적

## 🔧 웹 브라우저에서 일어나는 일

사용자가 웹 브라우저 url 창에 url을 입력하면 웹 브라우저 내에서는 어떤 과정이 일어날까?

![image](https://github.com/user-attachments/assets/65ad482a-a952-4c6b-ab17-d82f4e8266ee) <br />

1. **URL 입력**
   - client → 주소창에 URL을 입력해요.
   - browser → URL을 해석하고 요청을 준비해요.
2. **DNS 조회**
   - browser → URL에 조회된 도메인 이름을 IP 주소로 변환하기 위해 DNS 서버에 질의를 해요.
   - DNS Server → 도메인 이름을 IP 주소로 변환하여 브라우저에 반환해요.
3. **TCP 연결**
   - browser → 서버와의 통신을 위해 TCP 연결을 해요. 이 과정에서는 3-way handshaking이 있어요.
     1. SYN → 클라이언트가 서버에 연결 요청을 보내요.
     2. SYN-ACK → 서버가 요청을 수락하고 응답을 보내요.
     3. ACK → 클라이언트가 응답을 확인하고 연결을 완료해요.
4. **HTTP/HTTPS 요청**
   - browser → 설정된 TCP 연결을 통해 서버에 HTTP 또는 HTTPS 요청을 보내요. 이 요청에는 메서드, 헤더, 경로 등이 있어요.
5. **서버 처리**
   - server → 요청을 받고 처리해요. 처리 결과를 포함하는 HTTP 응답을 브라우저에 반환해요.
6. **HTTP/HTTPS 응답**
   - browser → 서버로부터 받은 HTTP 응답을 처리해요. 응답에는 헤더, 본문, 상태 코드 등이 포함돼요.
7. **렌더링 과정**
   - HTML 문서를 파싱하여 DOM(Document Object Model)을 만들어요.
   - CSS를 파싱하고 CSSOM(CSS Object Model)을 만들어요.
   - DOM과 CSSOM을 결합해 렌더 트리를 만들어요.
   - 레이아웃을 계산하여 각 요소의 위치와 크기를 결정해요
   - 렌더링을 진행하여 화면에 페이지를 그려요.
8. **자바스크립트 처리**
   - HTML 문서에 포함된 자바스크립트 파일을 다운로드하고 실행해요.
   - 자바스크립트 코드에 의해 페이지의 동작을 제어하고 동적으로 콘텐츠를 업데이트해요.
9. **자원 요청**
   - browser → HTML 문서에서 참조된 추가 자원(이미지 등)을 요청해요.
   - server → 추가 자원 요청에 응답하여 브라우저가 렌더링 해줘요.
10. **페이지 완성**
    - browser → 모든 자원이 로드되고 페이지가 완성돼요.
    - client → 페이지가 화면에 표시되며, 사용자와 상호작용해요.

<br />

## 🔧 가상 DOM은 몽데요

리액트의 가상 DOM(Virtual DOM)은 웹 애플리케이션에서 효율적인 UI 업데이트를 가능하게 한다. 조금 딥하게 알려주셔서 딥하게 적어본다.

![image](https://github.com/user-attachments/assets/59d5f921-1cab-4743-bb51-8b51e4c74ba1) <br />

1. **가상 DOM이 몽데요**
   - 리액트가 관리하는 메모리 내의 **가벼운 DOM 트리 복사본**이라고 한다. 이 트리는 **브라우저의 실제 DOM과 동일 구조**를 가지며, 변경 사항을 추적하고 관리한다.
2. **컴포넌트 렌더링과 상태 변경**
   - 리액트 컴포넌트의 상태나 props가 변경되면, 해당 컴포넌트는 새로운 UI를 나타내는 가상 DOM 트리를 생성한다. 이 시점에 새로운 가상 DOM은 이전 가상 DOM과 비교를 한다.
3. **디핑(Diffing)**
   - 디핑은 리액트가 이전 가상 DOM과 새로운 가상 DOM을 비교하여 두 트리 간의 차이점을 찾아내는 과정이다. 변경된 부분만을 식별해낸다.
4. **재조정(Reconciliation)**
   - 디핑 과정에서 변경된 부분이 확인되면, 실제 DOM을 업데이트하는 재조정 과정을 시작한다.
   - 이 과정에서 가상 DOM의 변경 사항을 실제 DOM에 반영한다. 오직 변경된 부분만을 DOM에 적용하므로, 전체 DOM을 재렌더링하는 대신 필요 부분만 업데이트 하여 성능 최적화에 좋다.
5. **리액트의 최적화**
   - 가상 DOM과 실제 DOM 사이의 불필요한 작업을 피함으로써 성능을 최적화한다.
   - 이 과정 덕에 UI 업데이트가 빠르게 수행되는 것이다.

결론적으로 가상 DOM은 이전 버전과의 **디핑** 과정을 거쳐 비교되고 **재조정** 과정에서 실제 DOM에 반영하게 된다.

<br />

## 🔧 바벨과 웹팩은 몽데요

### 💥 바벨(Babel)

바벨은 **코드 번역기**라고 보면 된다.

![image](https://github.com/user-attachments/assets/61fddb8e-93f9-4d14-baab-29fa93a8acf2) <br />

최신 JavaScript 문법을 변환해주는 것이다. JavaScript는 시간이 지나면서 새로운 기능들이 많이 추가되었다. (ex. ES6, ES7) 하지만 모든 브라우저가 최신 기능을 지원하는 것은 아니기 때문에 최신 문법을 구형 브라우저에서도 동작할 수 있게 바꿔주는 것을 말한다.

리액트를 사용하면 JSX, TSX라는 특수 문법을 쓰는데, 이건 브라우저가 이해하지 못한다. 바벨은 JSX, TSX 코드를 브라우저가 이해할 수 있는 일반적인 JavaScript로 변환해준다❗️

**쉽게 말해서,** 최신 **JavaScript 코드를 이전의 브라우저에서도 돌아가게 해주는 변환기**를 말한다. 또한? **리액트에서 쓰는 문법을 브라우저가 알아듣게 바꿔주는 도구**❗️

### 💥 웹팩(Webpack)

웹팩은 **파일을 묶어주는(번들링) 도구**이다.

![image](https://github.com/user-attachments/assets/056a5f31-69f4-40d2-a2bc-5a23b3319f5b) <br />

웹 애플리케이션은 여러 파일들로 이루어져 있다.(자바스크립트 파일, CSS 파일, 여러 이미지 등) **웹팩은 이 파일들을 하나로 포장해준다❗️**

또한, 코드가 어떤 순서로 실행되어야 하는지를 알아서 정리해주며 파일들이 서로 어떤 순서로 불러와야 할지 신경 쓸 필요가 없다. 알아서 의존성 관리를 해주기 때문이다❗️

**쉽게 말해서,** **여러 파일들을 하나로 묶어주는 포장 도구**이며 **코드를 관리하고, 빌드할 때 사용하기 편하게 만들어주는 것**이다 ~

> **결론적으로 바벨이 JavaScript 코드를 변환하고, 웹팩이 이 변환된 코드와 다른 리소스를 하나로 포장해주는 것이다.**

<br />

## 🔧 리액트의 렌더링 흐름

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Udemy X SniperCampus</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

`<div id="root"></div>` 이 부분이 React 애플리케이션이 렌더링될 컨테이너라고 보면 된다. 이 `div` 엘리먼트를 기반으로 전체 UI를 그린다.

그 아래 스크립트 태그는 `main.tsx` 파일을 불러와 브라우저는 이 파일을 실행하여 React 애플리케이션을 초기화한다.

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.tsx'
import './index.css'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

- `StrictMode`는 React의 개발 모드로 잠재적인 문제를 감지하고 경고를 제공한다.
- `createRoot`는 React 18에서 추가된 API로, React 애플리케이션의 진입점인 루트를 생성한다.
- `App`은 애플리케이션의 최상위 컴포넌트로 모든 컴포넌트가 여기에 포함된다.

결국 이 `main.tsx` 파일은 React 애플리케이션을 초기화하는 역할을 하고 React는 `App` 컴포넌트의 구조에 따라 `root` 엘리먼트 내에서 UI를 렌더링하는 것이다. 이 때 React는 가상 DOM을 활용해 효율적인 UI 업데이트를 수행하는 것이다.

위와 같은 흐름을 통해 **React 애플리케이션이 초기화**되고 **최종적으로 렌더링** 되는 것이다.

<br />

## 🔧 CreateElement

CreateElement는 리액트가 처음 출시될 때부터 존재했던 컴포넌트 작성 방법이다.

```js
const element = createElement(type, props, ...children);
const divElement = createElement("div", { id: "name" }, "Hello"); // <div id="main">Hello</div>
```

위와 같이 `createElement()`를 통해서 `div` 요소를 생성할 수 있다. 주석은 JSX 형태로 바꾼 것이다.

```js
import { createElement } from "react";

const App = () => {
  // <div><h1 class="greeting">Hello, React!</h1><p>Welcome to React 18!</div>
  return createElement(
    "div",
    null,
    createElement("h1", { className: "greeting" }, "Hello, React!"),
    createElement("p", null, "Welcome to React 18!")
  );
};

export default App;
```

low-level로 만든 컴포넌트이다. 주석을 보면 저것과 동일한 코드임을 알 수 있다.

![image](https://github.com/user-attachments/assets/0d8010eb-f5d0-48a9-bdeb-f4aa6a57e30d) <br />

JSX 요소이든 `createElement()`를 사용하든 똑같은 결과가 렌더링됨을 알 수 있다.

![image](https://github.com/user-attachments/assets/53e3028b-d559-47b1-84f3-d598b3427073) <br />

위는 `createElement()`를 통해 만든 간단한 화면이다. 코드는 긴 관계로 생략하도록 하겠다.

## 🔧 컴포넌트 CSS 스타일링

- **일반 CSS 파일**
  - 일반적인 CSS 파일을 작성하고 React 컴포넌트에서 `import`하여 사용한다.

```js
import "./App.css";

function App() {
  return <div className="container">Hello World</div>;
}
```

- **CSS Modules**
  - 파일명에 `.module.css` 확장자를 사용하여 클래스 이름을 고유하게 관리한다.

```js
import styles from "./App.module.css";

function App() {
  return <div className={styles.container}>Hello World</div>;
}
```

이렇게 하면 클래스 이름이 컴포넌트 스코프로 관리되고, CSS 충돌을 방지할 수 있다.

- **Styled Components**
  - `styled-components` 라이브러리를 사용해서 JavaScript 파일 안에서 스타일을 정의한다.

```js
import styled from "styled-components";

const Container = styled.div`
  padding: 20px;
  background-color: lightblue;
`;

function App() {
  return <Container>Hello World</Container>;
}
```

위처럼 컴포넌트와 스타일을 함께 관리 가능하며 동적 스타일링이 가능하다.

- **Inline Styles**
  - 컴포넌트 내에서 `style` 속성을 사용해서 직접 스타일을 정의한다.

```js
function App() {
  const style = {
    padding: "20px",
    backgroundColor: "lightblue",
  };

  return <div style={style}>Hello World</div>;
}
```

이 외에도 CSS를 사용하는 방법은 많다. `classNames`, `tailwindCSS`, `framer` 등 다양하다.

---

# 3️⃣ 느낀점

TypeScript에 대해 기본적인 개념을 마쳤다. 아직 모든 것을 배운 것은 아니지만 보통 많이 사용하는 것들에 대해서는 거의 배웠다고 한다. 배운 내용을 기반으로 9월 프로젝트할 때 써먹도록 해야겠다😌

그리고 나서 React에 대해서 본격적으로 들어갔다. React 초기 실행을 위한 설치 방법, 등에 대해 알아보았다.

또한, 팀프로젝트 배정 결과가 나왔는데 내가 원하던 kt wiz 프로젝트를 하게 되어 정말 좋았따 ~ 😮

내일도 화이팅구리 🌟😊

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
