---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 3일차"
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

# 일흔 일곱 번째 포스팅

안녕하세요! 일흔 일곱 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 3일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 여러 가지 CSS 알아보자

## 💧 TailwindCSS

[**TailwindCSS 공식홈페이지**](https://tailwindcss.com/docs/guides/vite)를 참고 바랍니다.

### 🙋 설치방법 및 적용

```js
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

위와 같이 `init`을 해주면 `tailwind.config.js` 파일이 만들어진다.

- **postcss** <br />
  postCSS는 css를 처리하는 도구로, tailwindCSS를 포함한 여러 css 프레임워크 및 플러그인과 같이 사용된다. 이는 tailwindCSS가 작성한 css 코드를 변환하고 최적화하는 데 사용된다.
- **autoprefixer** <br />
  Autoprefixer는 PostCSS의 플러그인 중 하나로, css 코드에 필요한 **브라우저 접두사를 자동으로 추가**해주는 것이다. 예로, 특정 브라우저에서만 동작하는 css 속성에 `-webkit-`, `-moz-` 와 같은 접두사를 자동으로 붙여준다.

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

위 파일에 이렇게 삽입을 하면 된다.

> 👀 **content** <br>

`content`는 tailwindCSS가 실제로 사용될 파일을 지정하는 파트이다. `index.html`은 프로젝트 루트 디렉토리에 존재하는 파일을 의미한다.

`"./src/**/*.{js,ts,jsx,tsx}"`는 `src` 폴더 내에 있는 모든 자바스크립트 파일, 타입스크립트 파일, 리액트 파일들을 의미한다.

tailwindCSS는 이 파일들에서만 클래스를 검색하여 스타일을 적용하게 된다. 이를 통해 사용하지 않는 css를 제거해서 파일 크기를 줄일 수 있게 된다.

> 👀 **theme** <br>

`theme`은 tailwindCSS에서 사용할 디자인 요소를 정의하는 부분이다.

기본 제공되는 테마를 확장하거나 새롭게 정의할 수 있으며 애니메이션을 적용할 때도 이것을 사용하면 편리하다.

> 👀 **plugins** <br>

`plugins`는 tailwindCSS의 기능을 확장하기 위해 추가할 수 있는 플러그인들을 정의하는 부분을 말한다.

이후에 `src` 디렉토리에 있는 `index.css` 파일에 다음과 같은 내용을 추가해주면 된다.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

이렇게 하고 나서 이제 tailwindCSS를 `className`에 적용해주면 된다.

```jsx
const App = () => {
  return (
    <div className="flex justify-center">
      <h1 className="flex justify-center w-full text-3xl font-bold underline items-center p-10 m-10 bg-yellow-600 shadow-lg rounded-2xl">
        Udemy X SniperCampus X Boongranii
      </h1>
    </div>
  );
};

export default App;
```

![image](https://github.com/user-attachments/assets/17acdcd8-e339-4024-8bd7-daa9a3a215a0) <br />

위와 같이 css가 적용된 것을 확인할 수 있다.

### 🙋 tailwindcss 클래스 정렬 플러그인

내가 필수로 설치하는 플러그인이다. css의 가독성 및 통일성을 위해서 저장할 때마다 알아서 css를 정렬해준다.

```js
npm install -D prettier prettier-plugin-tailwindcss
```

위와 같이 설치를 한다.

```js
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

그 후에 자신의 프로젝트 폴더의 루트 디렉토리에 `.prettierrc` 파일을 추가한 후 위와 같이 추가를 해주면 된다.

![image](https://github.com/user-attachments/assets/fb807b22-af25-48aa-bf1c-32920368ada1) <br />

요랬다가 ~

![image](https://github.com/user-attachments/assets/63701fba-13b7-4894-9bfe-cda10855e523) <br />

요래 됐숨다 ~

**이렇게 하면 모든 tailwindCSS에 통일성을 갖게 된다.**

### 🙋 tw-merge

나는 지금까지 `classNames`를 사용하여 클래스를 조건에 따라 동적으로 쉽게 결합하곤 했다. `classNames`는 여러 클래스를 조건부로 적용하는 데 유용하지만, tailwindCSS를 사용할 때는 조금 더 최적화된 방법이 필요하다.

tailwindCSS에서의 중복 클래스 문제를 해결하는 도구로 `tw-merge`가 있다. `tw-merge`는 클래스 중 중복되거나 충돌하는 것을 병합하여 불필요한 클래스를 제거해준다. 이를 통해 tailwindCSS에서 동적으로 스타일을 지정하면서도, 효율적이고 깔끔한 코드를 유지할 수 있다.

```jsx
import { twMerge as cn } from "tailwind-merge";

const Button = ({ isPrimary, isLarge }) => {
  return (
    <button
      className={cn(
        "p-4 text-white",
        isPrimary ? "bg-blue-500" : "bg-gray-500",
        isLarge ? "text-xl" : "text-sm"
      )}
    >
      눌러줘요
    </button>
  );
};

export default Button;
```

이렇게 `tw-merge`를 사용하여 tailwindCSS에서 동적이며 중복 없이 안전한 `className`을 지정할 수 있다.

`classNames`와 `tw-merge`의 목적이 다르므로, tailwindCSS를 사용할 때는 이 두 도구를 적절히 조합해 사용하는 것이 좋다. 나는 tailwindCSS를 자주 사용하니, 중복 클래스를 관리할 때는 `tw-merge`를 적극 활용해야겠다👀 쿄쿄

### 🙋 @tailwind 밑줄 없애기

`index.css`에 추가할 때 `@tailwind`에 밑줄이 있는 것을 볼 수 있다.

이는 해당 키워드가 css가 인식하지 못하는 키워드이기 때문이다. 별로도 밑줄을 없애지 않아도 애플리케이션이 작동하는 데에는 전혀 지장이 없다. 하지만 굳이 지우고 싶다면 다음과 같이 수정하면 된다.

```json
"css.lint.unknownAtRules": "ignore", // 추가 or 업데이트
```

`settings.json`파일에 들어가서(vscode 톱니바퀴 → Settings → Open Settings(JSON)) 위와 같이 수정을 하면 된다.

<br />

## 💧 CSS-in-JS

CSS-in-JS는 자바스크립트 코드 안에서 css 스타일을 작성하고 관리하는 방식이다. 자바스크립트 안으로 통합해서 컴포넌트와 스타일을 더 밀접하게 관련 지을 수 있다.

### 🌀 Styled Components

자세한 내용은 [**Styled Component 공식 홈페이지**](https://styled-components.com/)을 참고 바랍니다.

> 🐾 **설치 방법**

```js
npm i styled-components
```

위와 같이 설치를 마친다.

```jsx
import styled from "styled-components";

const MyStyle = styled.h1`
  font-size: 30px;
  color: #ed4848;
  text-decoration: line-through;
  background-color: #add8e6;
  &:hover {
    color: blue;
  }
`;

const App = () => {
  return (
    <div>
      <MyStyle>Udemy X SniperCampus X Boongranii</MyStyle>
    </div>
  );
};

export default App;
```

`styled-component` 라이브러리를 임포트하여 보통 `styled`를 사용한다.

`styled.h1`과 같이 `<h1>` 태그에 저러한 스타일을 적용하겠다는 것이다. 다른 태그를 추가하고 싶으면 h1 자리에 다른 요소를 넣으면 된다.

이제 스타일을 적용했던 `MyStyle`을 실제 컴포넌트에 리액트에서 컴포넌트를 사용하는 것처럼 추가해주면 된다.

개인적으로 정말 불편하다고 생각한다. tailwindCSS를 자주 사용해서 그런지 모르겠지만. . . . . 너무 복잡해보이기도 하고 컴포넌트인지 스타일 컴포넌트인지 구분이 어려워 보이기도 한다... 그냥 그렇다... 테일윈드 최고💛

### 🌀 Emotion

자세한 내용은 [**Emotion 공식 홈페이지**](https://emotion.sh/docs/introduction)을 참고 바랍니다.

> 🐾 **설치 방법**

```jsx
npm i @emotion/css
```

위와 같이 설치를 마친다.

```jsx
import { css } from "@emotion/css";

const App = () => {
  return (
    <div>
      <h1
        className={css`
          font-size: 30px;
          color: #ed4848;
          text-decoration: line-through;
          &:hover {
            color: blue;
          }
        `}
      >
        Udemy X SniperCampus X Boongranii
      </h1>
    </div>
  );
};

export default App;
```

`styled-components`는 태그를 사용하는 방식이었다면, `emotion`은 inline-css 방식과 비슷한 느낌으로 사용한다.

`styled-components`에서의 단점을 보완해서 나왔다. `emotion`은 스타일 컴포넌트의 헷갈림을 해소하고자 했으며 별도의 폴리필 없이도 ie 11을 지원한다.

### 🌀 Vanilla Extract

자세한 내용은 [**Vanilla Extract 공식 홈페이지**](https://vanilla-extract.style/)을 참고 바랍니다.

> 🐾 **설치 방법**

```js
npm i @vanilla-extract/css @vanilla-extract/vite-plugin
```

위와 같이 설치를 마친다.

Vanilla Extract의 가장 큰 특징 중 하나로 **제로 런타임**이 있다. 이는 스타일이 런타임에서 계산되지 않고 빌드 타임에 미리 생성된다는 것이다.

> 🐾 **Zero Runtime?**
>
> **제로 런타임**이란 스타일을 적용하기 위해 브라우저에서 자바스크립트 코드가 실행될 필요가 없다는 것을 의미한다. 즉, 스타일이 애플리케이션 실행 중 계산되거나 생성되는 대신, 빌드될 때 미리 생성되고 CSS 파일로 출력되는 것이다. <br /> 이 방식은 **런타임 성능을 최적화**하고, **번들 크기를 줄이며** 스타일 적용 시점의 **복잡성을 크게 낮춘다**.

<br />

## 💧 폰트 설정 방법

### 👀 구글 폰트

[**구글 폰트 공식 홈페이지**](https://fonts.google.com/?subset=korean&script=Kore)에서 원하는 폰트 선택 후 코드를 얻어내면 된다.

```css
@import url("https://fonts.googleapis.com/css2?family=Jua&display=swap");

.jua-regular {
  font-family: "Jua", sans-serif;
  font-weight: 400;
  font-style: normal;
}
```

`src/styles/fonts.css` 파일을 새로 만들어 붙여 넣어주면 된다. (파일을 만들지 않고 원하는 곳에 넣어도 된다.)

그 후 원하는 태그에 `jua-regular`를 넣어주면 적용이 될 것이다.

![image](https://github.com/user-attachments/assets/7ca37e4f-6116-49eb-a2a9-c60de869d533) <br />

위와 같이 폰트가 적용된 것을 확인할 수 있다.

### 👀 눈누

[**눈누 공식 홈페이지**](https://noonnu.cc/)에서 원하는 폰트 선택 후 코드를 얻어내면 된다.

![image](https://github.com/user-attachments/assets/d4c2a7f8-fb4f-4a40-81ca-d2662430d9bb) <br />

위와 같이 오른쪽에 `font-face`를 가져와 사용하면 된다.

```css
@font-face {
  font-family: "GmarketSansMedium";
  src: url("https://fastly.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansMedium.woff")
    format("woff");
  font-weight: normal;
  font-style: normal;
}

.gmarket {
  font-family: "GmarketSansMedium", sans-serif;
  font-weight: 400;
  font-style: normal;
}
```

위와 같이 한 후 폰트 적용하고 싶은 태그에 `gmarket`을 사용하면 된다.

하나하나 태그에 사용하기 귀찮다면

```css
body {
  font-family: "GmarketSansMedium", sans-serif;
  font-weight: 400;
  font-style: normal;
}
```

`body`에 다 때려박아 버리면 된다.

### 👀 WOFF

외부 사이트에서 제공하는 URL을 사용하는 것 외에도, 직접 다운로드한 `.woff` 파일을 통해 폰트를 적용할 수 있다.

```css
@font-face {
  font-family: "Gmarket";
  src: url("../fonts/GmarketSansMedium.woff") format("woff");
  font-weight: 400;
  font-style: normal;
}

body {
  font-family: "Gmarket", sans-serif;
}
```

위는 css를 통해 로컬 폰트 파일을 사용하는 방법이다.

이 방법으로 로컬에 저장된 폰트를 웹 페이지에서 사용할 수 있다. 폰트 파일의 위치와 경로를 정확히 지정하는 것이 중요하다❗️

### 👀 TTF, WOFF, WOFF2가 몽데요

- **TTF** <br />
  기본적인 폰트 포맷으로 벡터 기반으로 흘러간다. 호환성이 높지만 파일 크기가 커서 웹 최적화에는 비효율적이다.

- **WOFF** <br />
  웹에서 폰트를 사용할 때 최적화된 포맷으로, TTF와 OTF 파일을 압축해서 파일 크기를 줄일 수 있다.

- **WOFF2** <br />
  WOFF의 후속작으로 더효율적인 압축을 제공해서 웹 페이지 로딩 속도를 더욱 개선하였다.

보통 `WOFF2`를 사용한다. `TTF`는 웹 폰트로 사용하기보다는 일반적인 데스크톱 애플리케이션에 적합하기 때문이다. `WOFF2`를 사용하면 파일 크기도 줄여지고 페이지 로딩 성능도 더 나아진다.

---

# 2️⃣ React의 시작

## 🍊 React 컴포넌트의 원리 및 특징

1. **React의 컴포넌트 트리 구조**
   - React 애플리케이션은 컴포넌트들로 구성된 트리 구조를 갖는다. 상위 컴포넌트가 있고, 하위에 자식 컴포넌트가 배치된다.
   - 트리 구조에서 상위 컴포넌트가 자식 컴포넌트를 포함하는 방식으로 구성된다.
2. **재렌더링의 기본 원리**
   - React 컴포넌트는 **상태**나 **props**가 변경될 때 재렌더링된다.
   - 상위 컴포넌트의 상태나 프롭스가 변경되면 이 컴포넌트도 재렌더링되고, 이에 따라 자식 컴포넌트들도 재렌더링된다.
3. **깊이 우선 탐색(DFS)**
   - React는 컴포넌트 트리를 DFS 방식으로 탐색하면서 컴포넌트를 렌더링한다. DFS는 트리의 가장 깊은 곳부터 순차탐색을 한다.
4. **재렌더링 방지(최적화)**
   - `React.memo`: 자식 컴포넌트가 동일 프롭스를 받았을 때 불필요한 재렌더링을 방지할 수 있다. 이를 사용하면 프롭스가 변경되지 않은 경우에 컴포넌트가 재렌더링되지 않는다.

<br />

## 🍊 컴포넌트의 이벤트

### 🔥 이벤트 핸들러 함수 정의

React 컴포넌트에서 이벤트 처리는 HTML에서의 이벤트 처리와 유사하다. 하지만 JSX 문법에 맞춰야 하기 때문에 약간의 차이가 있다.

- React에서의 이벤트는 카멜 케이스(camelCase)를 사용해야 한다.
- JSX를 사용해서 문자열이 아닌 이벤트 핸들러를 전달한다.

이벤트를 처리하기 위해서는 이벤트 핸들러를 정의하고 JSX 요소의 속성으로 이 함수를 전달하면 된다.

```html
<button onclick="buttonClickHandler()">눌러줘요</button>
```

위는 HTML에서의 이벤트 처리 방식이다.

```jsx
<button onClick={buttonClickHandler}>눌러줘요</button>
```

위는 React에서의 이벤트 처리 방식이다.

이벤트 핸들러를 함수로 전달해야 하며, 함수 호출 결과가 아닌 함수 자체를 전달해야 한다.

`buttonClickHandler()`처럼 함수를 호출하는 코드를 넣으면 안된다.

> 🍬 **잠깐만❗️**

```jsx
<button onClick={buttonClickHandler}>눌러줘요</button>
<button onClick={()=>buttonClickHandler()}>눌러줘요</button>
```

위처럼 이벤트 핸들러를 전달하는 방식은 두 가지가 있다. 뭐가 다를까?

**첫 번째**는 `buttonClickHandler` 함수 자체를 `onClick`에 전달하기 때문에 이벤트가 발생할 때 함수가 호출된다. 불필요한 함수 호출이 없어서 코드가 간결하다는 장점이 있다.

**두 번째**는 화살표 함수를 사용해서 클릭 시 화살표 함수가 호출되고 그 안에서 `buttonClickHandler()`가 호출된다. 이것은 `buttonClickHandler`에 **인자를 전달하거나 클릭할 때 어떠한 논리적 처리를 추가**하고자 할 때 이 방법을 사용한다.

### 🔥 이벤트 객체

이벤트가 발생하면 React는 자동으로 이벤트 객체를 핸들러 함수에 전달한다. 이 객체는 원래 DOM 이벤트 객체를 wrapping한 것으로 브라우저 간의 일관성을 유지한다.

이벤트 객체는 첫 번째 인자로 전달되고 보통 `event`나 `e`와 같은 이름으로 받는다.

```jsx
const buttonClickHandler = (e) => {
  console.log(e.target); // 클릭 요소 참조
};
```

### 🔥 event 객체 타입 추론과 명시적 타입 지정

이벤트 핸들러에서 `event` 객체를 자주 다루게 된다. 특히 TypeScript를 사용할 때는 이 `event` 객체의 타입을 명시적으로 지정해서 코드의 안정성을 높이는 것이 좋다.

React에서 이벤트 핸들러를 정의할 때, TypeScript는 자동으로 `event` 객체의 타입을 추론한다. 이 타입은 핸들러가 어떤 HTML 요소에서 발생하는 이벤트인지에 따라 달라지게 된다.

```jsx
<button onClick={(event) => buttonClickHandler(event)}>눌러줘요</button>
```

위 코드에서 `event`에 마우스를 올리면 TypeScript가 `event`의 타입을 추론해서 아래 사진처럼 툴팁으로 보여준다.

![image](https://github.com/user-attachments/assets/70c08c01-5f5a-40e9-8519-cc843214b958) <br />

버튼 클릭하는 이벤트에서는 `event`가 `React.MouseEvent<HTMLButtonElement>` 타입으로 추론한다.

TypeScript의 자동 타입 추론 기능은 매우 강력하고 좋지만, 명시적으로 타입을 지정하면 코드의 가독성과 유지보수성도 높일 수 있다.

```jsx
const buttonClickHandler = (event: React.MouseEvent<HTMLButtonElement>) => {
  console.log(event.currentTarget);
};

<button onClick={(event) => buttonClickHandler(event)}>눌러줘요</button>;
```

타입 추론된 결과를 바탕으로 위와 같이 명시적으로 타입을 지정하면 코드가 더 명확해지고 오류를 줄일 수 있는 방법이다.

**결론적으로**, 자동 타입 추론이 잘 작동하더라도 명시적으로 타입을 지정하면 좋다. <br /> 특히, 함수에 전달된 `event` 객체의 타입이 복잡하거나, 특정 상황에서 타입 안전성을 강화하고자 할 때 도움이 된다.

<br />

## 🍊 props가 몽데요

React에서 컴포넌트를 재사용할 때 가장 핵심적인 개념은 바로 `props`이다. `props`는 **부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달하는 방법**으로, **컴포넌트 간의 데이터를 주고받을** 수 있게 해준다.

`props`는 "properties"의 줄임말이며 컴포넌트에 전달된 데이터를 의미한다. React에서는 컴포넌트를 쓸 때, 함수의 매개변수처럼 `props`를 받아 사용 가능하다❗️

이 덕에 컴포넌트는 동적으로 데이터를 받아 사용하며 렌더링하여 다양한 상황에서 재사용이 가능해진다.

```jsx
const Nav = () => {
  return (
    <nav>
      <ul>
        <li>
          <a href="#section1">소개</a>
        </li>
        <li>
          <a href="#section2">서비스</a>
        </li>
        <li>
          <a href="#section3">연락처</a>
        </li>
      </ul>
    </nav>
  );
};

export default Nav;
```

위와 같이 `Nav` 컴포넌트가 존재한다. 링크 항목들이 하드코딩되어 있어 재사용성이 매우 떨어진다. 만약 새로운 링크를 추가하거나 수정하고자 한다면 위 컴포넌트의 내부 코드를 직접 수정해야 한다.

위 문제를 해결하기 위해 `props`를 사용해서 각 링크 항목을 컴포넌트화하고 재사용 가능하도록 만들어보자.

```tsx
type ListItemProps = {
  href: string;
  title: string;
};

const ListItem = ({ href, title }: ListItemProps) => {
  return (
    <>
      <li>
        <a className="text-blue-500 underline" href={href}>
          {title}
        </a>
      </li>
    </>
  );
};

export default ListItem;
```

링크 항목 하나를 `ListItem`이라는 컴포넌트로 분리한다.

이 컴포넌트는 `href`와 `title`이라는 두 개의 `props`를 받아 링크의 주소와 제목을 동적으로 설정이 가능하다.

```jsx
import ListItem from "./components/list-item";

const App = () => {
  return (
    <nav>
      <ul>
        <ListItem href="#section1" title="소개" />
        <ListItem href="#section2" title="서비스" />
        <ListItem href="#section3" title="연락처" />
      </ul>
    </nav>
  );
};

export default App;
```

이제 `Nav` 컴포넌트는 재사용 가능한 ListItem 컴포넌트를 이용해 동적으로 다양한 링크를 렌더링할 수 있다. 새로운 링크를 추가하거나 수정하고자 할 때, `ListItem` 컴포넌트를 사용하여 `props`만 전달하면 된다.

**결론적으로, React에서 `props`를 사용하면 컴포넌트를 유연하고 재사용 가능하도록 만들 수 있다.**

---

# 3️⃣ 느낀점

오늘은 어제에 이어 여러 CSS에 관하여 배워보았다. 나는 tailwindCSS를 사용했어서 이것이 가장 편한듯하다. 하지만 다른 것도 써보면 좋다고 한다. CSS는 종류가 너무 많은 것 같다. 개인 취향에 따라 사용하는 것이 좋은 듯하다..

그리고 나서 React에 대해서 본격적으로 들어갔다. 컴포넌트, 이벤트, 이벤트 객체, props에 관해서 공부했다. 원래 알고 있는 내용도 있었지만 조금 더 알게 되어 좋았다👍

![image](https://github.com/user-attachments/assets/2cee0b1f-6650-4754-92cb-638252b645a3) <br />

수업이 끝나고 미니게임을 진행했다. 미니게임 재밌더라 ㅋ 끝나고 다람쥐를 외치며 도망나왔. . .다람지.

내일도 화이팅 해봅시다람지 💫👋

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
