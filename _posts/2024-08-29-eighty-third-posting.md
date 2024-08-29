---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 9일차"
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

# 여든 세 번째 포스팅

안녕하세요! 여든 세 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 9일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 에러 바운더리

React Error Boundary는 React에서 컴포넌트 트리 내에서 자식 컴포넌트에서 발생하는 **JavaScript 에러**를 포착하고, 해당 에러를 처리할 수 있는 방법을 제공한다. Error Boundary는 컴포넌트가 깨져서 전체 UI가 비정상적으로 렌더링되지 않도록 보호하는 역할을 한다.

Error boundary는 클래스 컴포넌트에서만 구현될 수 있다. `componentDidCatch`와 `getDerivedStateFromError` 이 두 가지 라이프사이클 메서드를 사용한다.

- `getDerivedStateFromError`: 에러가 발생하면 컴포넌트의 상태를 업데이트할 수 있다.
- `componentDidCatch`: 실제로 에러를 잡고, 로깅하는 부수 효과를 수행한다.

```tsx
import React, { Component, ReactNode } from "react";

interface Props {
  children: ReactNode;
}

interface State {
  hasError: boolean;
}

class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    // 다음 렌더링에서 폴백 UI가 보이도록 상태를 업데이트 함.
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    // 에러 로깅 서비스를 이용해 에러를 기록
    console.error("ErrorBoundary caught an error", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 폴백 UI를 정의
      return <h1>문제가 발생했습니다.</h1>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

위와 같이 클래스형 컴포넌트로 만들어준다.

```jsx
import React from "react";
import ErrorBoundary from "./ErrorBoundary";
import MyComponent from "./MyComponent";

const App = () => {
  return (
    <div>
      <ErrorBoundary>
        <MyComponent />
      </ErrorBoundary>
    </div>
  );
};

export default App;
```

`ErrorBoundary` 컴포넌트를 사용해서 자식 컴포넌트를 감싸면 딘다. 자식 컴포넌트에서 에러가 발생할 경우 `ErrorBoundary`가 에러를 잡아주고, 사용자에게 폴백을 보여준다.

이로써, 예기치 않은 에로러 인해 완전히 깨지는 것을 방지할 수 있으며 사용자에게 오류 메시지를 제공할 수 있다.

---

# 2️⃣ 라우터

React Router는 React 애플리케이션에서 CSR을 구현할 수 있게 해주는 라이브러리이다. 이를 사용하면 **페이지를 전환할 때 전체 페이지를 새로고침하지 않고도 URL을 변경**하고, 해당 URL에 맞는 컴포넌트를 렌더링할 수 있다.

React Router의 최신 버전에서는 새로운 라우터 구성 API와 `Data Router` 기능이 도입되었다고 한다. 이로 인해 더 강력하고 선언적인 방식으로 라우팅을 구성할 수 있다.

- `createBrowserRouter`: 라우터 객체를 생성하기 위한 함수이다. 이 함수는 경로와 해당 컴포넌트, 데이터 로딩 로직 등을 포함하는 객체 배열을 인자로 받아 브라우저 기반의 라우터를 만든다.
- `RouterProvider`: `createBrowserRouter`로 생성된 라우터 객체를 `RouterProvider`에 전달해서 애플리케이션의 루트 라우터로 사용 가능하다.

## ♻️ 설치

```bash
npm install react-router-dom
```

## ♻️ 적용

```jsx
import { createBrowserRouter, RouterProvider } from "react-router-dom";

import Home from "./components/Home";
import About from "./components/About";

// 라우터 객체 생성
const router = createBrowserRouter([
  {
    path: "/",
    element: <Home />,
  },
  {
    path: "/about",
    element: <About />,
  },
]);

const App = () => {
  return (
    // RouterProvider에 라우터 객체 전달
    <RouterProvider router={router} />
  );
};

export default App;
```

위처럼 React Router는 선언적으로 관리할 수 있게 해주고 `createBrowserRouter`와 `RouterProvider`를 사용해서 데이터 로딩, 에러 핸들링, 권한 관리 등 다양한 기능을 한꺼번에 통합 및 사용 가능하다.

더욱 자세한 내용은 [**React Router 공식 Docs**](https://reactrouter.com/en/main) 를 참고 바란다.

---

# 3️⃣ 느낀점

오늘은 사전 직무교육 마지막날이다. 교육은 이제 끝이다. 오늘은 `ErrorBoundary`와 `Router`에 대해서 학습했다. 오늘은 대부분 실습 위주로 진행해서 이론적인 내용은 별로 없어 금일 포스트는 많이 짧다. 내용도 알차지 못하다. 하지만 실습이 알찼으니 됐다. 쿄쿄❕

또한, 마지막에 블로그 만드는 실습을 진행하였다. TypeScript로 작성하다 보니 이곳저곳에서 엄청난 빨간줄이 발생했다. 9일동안 배운 내용을 토대로 이 모든 것을 적용시켜 블로그 하나를 뚝딱 완성할 수 있다는 것이 신기했다.

`json-server`라는 것도 처음 접해보았고 간단하게 데이터를 받고 요청하는 것을 블로그에 적용해보았다.

9일동안 TypeScript를 배운 내용을 토대로 이제 9월에 우리 프로젝트에 열심히 적용해서 완성 시키고 싶다😀😉

![image](https://github.com/user-attachments/assets/42ab0b17-9211-45e4-8918-59d379271a95)<br />

프로젝트 완성이 목표가 아니라 좋은 결과물이 나오도록 끝까지 화이팅 쿄쿄💛✨🎵

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
