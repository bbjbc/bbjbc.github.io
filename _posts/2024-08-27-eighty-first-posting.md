---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 7일차"
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

# 여든 한 번째 포스팅

안녕하세요! 여든 한 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 7일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ React 훅

## 💧 useDeferredValue

`useDeferredValue`는 React 18에서 도입된 훅이다. 이는 사용자 인터페이스의 업데이트 우선 순위를 제어하는 데 사용된다. 이는 값의 업데이트를 지연시켜 우선 순위를 조절한다. 이를 통해서 React 애플리케이션의 성능을 최적화하고 UX를 개선할 수 있다.

이것은 보통 성능 최적화가 필요한 검색 기능과 같은 상황에서 유용하다고 한다. 사용자가 입력 필드에 빠르게 입력할 때, 입력 텍스트가 즉시 반영되지만 해당 텍스트에 따라 필터링된 결과를 표시하는 것을 지연시킬 수 있다.

```jsx
const deferredValue = useDeferredValue(value);
```

`value`는 지연시키고 싶은 상태를 말한다. 이 값의 업데이트는 React에 의해 비동기적으로 지연될 수 있다.

`deferredValue`는 `useDeferredValue` 훅이 반환하는 값으로 React가 더 중요한 렌더링 작업을 먼저 처리할 수 있도록 업데이트가 지연된다.

### ✔️ **`useTransition`** vs. **`useDeferredValue`**

> ⭕️ **`useTransition`**

`useTransition`은 상태 업데이트의 **"시점"**을 제어한다고 한다. 상태 업데이트가 트리거되는 순간부터 그 업데이트가 **"느린"** 업데이트로 처리되도록 한다. 전체 UI의 렌더링 주기를 조정하여 사용자 입력에 더 높은 우선순위를 부여할 수 있다.

이는 대규모 컴포넌트 트리 업데이트, 데이터 페칭, 네트워크 요청과 같이 상대적으로 오래 걸리는 작업에서 사용된다. 예를 들어서 페이지 전환 할 때에 네비게이션의 반응성을 유지하며 콘텐츠를 불러오는 작업을 처리할 수 있다. (ex. 로딩 스피너)

> ⭕️ **`useDeferredValue`**

`useDeferredValue`는 **값 자체를 지연**시킨다. 즉, 특정 값의 변경이 자주 발생할 때, 그 값의 업데이트를 지연시키고 해당 지연된 값을 사용하여 UI를 업데이트한다.

이는 사용자의 빠른 입력에 대해 UI의 응답성을 유지하면서도, 값이 자주 변경될 때 불필요한 렌더링을 방지하는 데 적합하다. 어제 실습 예제인 장바구니 예제에서 입력 값에 따른 필터링 값에 적용하면 좋을 것 같기도 하다.

**결론적으로** 두 훅 **모두 UI의 성능을 최적화하고 UX를 개선**하기 위한 도구지만 **`useTransition`은 상태 업데이트 자체를 지연시키지만, `useDeferredValue`는 특정 값의 업데이트를 지연**시킨다.

---

# 2️⃣ 메모이제이션

메모이제이션(Memoization)은 **컴퓨터 프로그램을 최적화하기 위한 기법**이다. **함수의 실행결과를 저장**해두고 동일 입력으로 함수가 호출될 때 **저장된 결과를 반환해 불필요한 계산을 피하는 방법**이다.

즉, 계산된 값을 **캐시에 저장**해 동일한 입력으로 반복해 함수를 호출할 때 **이전 결과를 재사용함**으로써 **성능을 최적화**한다.

```jsx
const fibonacci = (n: number) => {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
};

console.log(fibonacci(10)); // 55
```

위 함수는 피보나치 수열의 `n` 번째 값을 계산하지만 **중복된 계산이 계속해서 발생하**게 된다. 예를 들어서 `fibonacci(5)`를 계산할 때면 `fibonacci(3)`이 여러 번 호출된다.

```jsx
const fibonacci = (n: number, memo = {}) => {
  if (n <= 1) return n;
  if (momo[n]) return momo[n];
  momo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);
  return momo[n];
};

console.log(fibonacci(10)); // 55
```

`memo` 객체를 통해 각 호출마다 계산된 피보나치 수열 값을 저장한다. 이미 계산된 값이 `memo`에 저장되어 있다면 해당 값을 바로 반환하여 불필요한 재귀 호출을 피할 수 있다.

React에서는 `React.memo`, `useMemo`, `useCallback`과 같은 도구들을 통해 메모이제이션을 사용해 불필요한 렌더링을 방지하고, 성능을 최적화하는 데 도움을 준다. 이것들은 컴포넌나 값, 함수의 결과를 메모이제이션해서, 같은 입력 값에 대해 불필요하게 재계산하거나 재렌더링되지 않도록 한다.

## ☑️ React.memo

React의 `React.memo`는 불필요한 재렌더링을 방지하기 위해 사용되는 최적화 기법이다.

`React.memo`는 고차 컴포넌트로, **컴포넌트를 메모이제이션**해서 동일한 props가 전달되는 경우 컴포넌트가 재렌더링되지 않도록 한다.

즉, **부모 컴포넌트가 재렌더링 되더라도** 자식 컴포넌트의 props가 변경되지 않으면, 해당 자식 컴포넌트의 재렌더링을 방지한다. 이로써 불필요한 렌더링을 줄일 수 있다.

```tsx
import React, { useState } from "react";

const MemoizedChild = React.memo(({ count }: { count: number }) => {
  console.log("MemoizedChild 렌더링");
  return <div>Count: {count}</div>;
});

const ParentComponent = () => {
  const [count, setCount] = useState<number>(0);
  const [text, setText] = useState<string>("");

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>증가</button>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <MemoizedChild count={count} />
    </div>
  );
};

export default ParentComponent;
```

`MemoizedChild` 컴포넌트는 `React.memo`로 감싸져 있으며 `count`라는 prop을 받는다.

![memo](https://github.com/user-attachments/assets/d6ed7f92-6eb9-429f-9e57-762606387ac3) <br />

사용자가 버튼을 클릭하여 `count`를 증가시키면 `MemoizedChild`는 재렌더링된다.

사용자가 텍스트 필드에 값을 입력하면 `text` 상태가 업데이트되지만 `count` 값이 변경되지 않았기 때문에 `MemoizedChild`는 재렌더링되지 않는다.

여기서 `MemoizedChild`는 props가 변경되지 않는 한 재렌더링되지 않기 때문에 성능 최적화를 이룰 수 있다.

<br />

## ☑️ useCallback

`useCallback`은 메모이제이션된 콜백 함수를 반환하는 훅이다. 컴포넌트가 재렌더링될 때마다 함수가 새로 생성되지 않도록 함으로써, 함수의 **참조 무결성**을 유지할 수 있다. 즉, 동일한 의존성 배열이 제공되면, 이전에 생성된 함수의 참조를 재사용한다.

이 훅은 함수를 props로 자식 컴포넌트에 전달할 때 유용하다. 만약 자식 컴포넌트가 `React.memo`로 메모이제이션된 경우, 부모 컴포넌트가 재렌더링될 때마다 함수의 참조가 바뀌면 자식 컴포넌트도 불필요하게 재렌더링될 수 있다. `useCallback`을 사용하면 이를 방지할 수 있다❗️

```tsx
import React, { useState, useCallback } from "react";

type MemoizedChildProps = {
  onClick: () => void;
};

const MemoizedChild = React.memo(({ onClick }: MemoizedChildProps) => {
  console.log("MemoizedChild 렌더링");
  return <button onClick={onClick}>눌러줘요</button>;
});

const ParentComponent = () => {
  const [count, setCount] = useState(0);

  // useCallback을 사용하여 함수의 참조를 메모이제이션
  const handleClick = useCallback(() => {
    setCount((prevCount) => prevCount + 1);
  }, []); // 의존성 배열이 비어 있으므로 이 함수는 처음 생성된 이후로 변경되지 않음.

  return (
    <div>
      <MemoizedChild onClick={handleClick} />
      <p>Count: {count}</p>
    </div>
  );
};

export default ParentComponent;
```

`handleClick` 함수는 `useCallback`을 사용해 생성되었다. 의존성 배열이 비어 있으므로 부모 컴포넌트가 재렌더링될 때마다 새로 생성되지 않는다.

부모 컴포넌트가 재렌더링 되더라도 `handleClick`의 참조는 동일하므로, `MemoizedChild` 컴포넌트는 재렌더링되지 않는다.

![useCallback](https://github.com/user-attachments/assets/f321163c-6579-4acf-8487-4b9419c9dce7) <br />

위와 같이 `useCallback`으로 함수의 재렌더링을 막아 콘솔이 로깅되지 않는 것을 확인할 수 있다.

<br />

## ☑️ useMemo

`useMomo`는 **값을 메모이제이션하여 불필요한 계산을 방지**하는 데 사용된다. `useMemo`는 주어진 **의존성 배열**의 값이 변경되지 않는 한, 이전 계산된 값을 재사용한다. 이를 통해서 성능을 최적화하고, 특히 **비용이 큰 계산을 반복적으로 수행하는 것을 방지**할 수 있다.

```jsx
const memoizedValue = useMemo(() => func(a, b), [a, b]);
```

위는 `useMemo`의 문법이며 첫 번째 인자에는 계산하고자 하는 값을 반환하는 함수가 들어가고, 두 번째 인자에는 의존성 배열이 포함된다. 배열 안의 값이 변경될 때에만 `useMemo`가 제공하는 함수가 재실행된다.

```tsx
import { useState, useMemo } from "react";

const computeExpensiveValue = (num: number) => {
  console.log("비용이 큰 계산 수행 중.. 오래 걸리네요..");
  let result = 0;
  for (let i = 0; i < 1000000000; i++) {
    result += num;
  }
  return result;
};

const App = () => {
  const [count, setCount] = useState<number>(0);
  const [input, setInput] = useState<string>("");

  // useMemo를 사용하여 비용이 큰 계산의 결과를 메모이제이션.
  const expensiveValue = useMemo(() => computeExpensiveValue(count), [count]);

  return (
    <div>
      <h1>useMemo 사용해보기</h1>
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="입력해봐요"
      />
      <p>입력 값: {input}</p>
      <p>계산 결과: {expensiveValue}</p>
      <button onClick={() => setCount(count + 1)}>Count 증가</button>
    </div>
  );
};

export default App;
```

위와 같이 정말 큰 루프를 실행해서 계산이 느리다고 가정한다.

`useMemo`는 `computeExpensiveValue(count)`의 결과를 메모이제이션하며, 의존성 배열에 `count`만 포함되어 있기 때문에, `count`가 변경될 때만 `computeExpensiveValue`가 다시 호출된다.

![useMemo](https://github.com/user-attachments/assets/96c8fb57-5000-4f3e-9e29-b7ee8dcdcfc6) <br />

텍스트 필드에 값을 입력하여 `input`이 변경되더라도 `count` 값은 변하지 않으므로 `computeExpensiveValue`함수는 호출되지 않는다.

`useMemo`를 사용하지 않으면 `input` 상태가 변경될 때마다 `computeExpensiveValue` 함수가 다시 실행되어 성능이 저하될 수 있다. `count`가 변경될 때에만 비용이 큰 계산을 재실행하고, 그렇지 않다면 **이전 계산 결과를 재사용**하여 성능을 최적화한다.

> 💢 **하지만**

모든 계산에 대해 `useMemo`를 사용하는 것은 성능을 오히려 저하시킬 수 있다. 이것의 사용으로 이점이 크지 않다면 사용을 피하는 것이 좋다.

또한, 의존성 배열에 포함된 값이 정확히 관리 되어야 한다. 잘못된 의존성 배열을 사용하면 의도치 않은 메모이제이션된 값이 재계산되거나, 필요 시 계산되지 않을 수 있다❗️

<br />

## ☑️ memo / useCallback / useMemo

|    구분     |               `useMemo`               |                `React.memo`                 |                 `useCallback`                  |
| :---------: | :-----------------------------------: | :-----------------------------------------: | :--------------------------------------------: |
|    목적     | 값의 메모이제이션 (계산 결과 재사용)  |          컴포넌트의 재렌더링 방지           |  함수 참조의 메모이제이션 (참조 무결성 유지)   |
|  적용 대상  |             값, 계산 결과             |                  컴포넌트                   |                      함수                      |
|  사용 시점  |           비용이 큰 계산 시           | 자식 컴포넌트의 불필요한 재렌더링을 막을 때 | 함수를 props로 전달할 때 참조 유지가 필요할 때 |
| 의존성 관리 | 의존성 배열의 값이 변경될 때만 재계산 |        props가 변경될 때만 재렌더링         |   의존성 배열의 값이 변경될 때만 함수 재생성   |

---

# 3️⃣ 상태 관리 기법

## ✅ Context API

React의 **Context API**는 **전역 상태 관리를 쉽게 할 수 있게 도와주는 도구**이다. 이를 사용하면 컴포넌트 트리의 깊은 곳에 있는 자식 컴포넌트에 직접적으로 데이터를 전달할 수 있으며, props를 통한 `prop-drilling`에 빠지는 것을 방지할 수 있다.

이는 컴포넌트 트리의 최상위에서 데이터를 정의하고, 하위 컴포넌트들이 이 데이터를 필요에 따라 쉽게 사용할 수 있도록 해준다. **전역적으로 필요한 데이터를 관리**하기 유용하다.

1. `React.createContext`: Context를 생성하는 함수이다. 기본값을 설정 가능하며 이 context를 사용하여 제공자와 소비자를 생성한다.
2. `Context.Provider`: Context의 데이터를 제공하는 컴포넌트이다. Provider는 Context의 값을 상위 컴포넌트에서 설정하고, 하위 컴포넌트에서 이 값을 사용할 수 있다.
3. `Context.Consumer`: 이는 요즘 사용하지 않는 추세이며 `useContext`훅을 사용하는 것이 일반적이다.
4. `useContext`: Context의 현재 값을 구독하고 사용 가능한 훅이다. `Context.Consumer`의 구문을 간소화 해준다.

> 💻 **`AuthContext.tsx`**

```tsx
import { createContext } from "react";

type AuthContextType = {
  user: string | null;
  login: (username: string) => void;
  logout: () => void;
};

// 초기 상태 값 설정 (undefined로 설정하여 타입스크립트의 안전성 유지)
export const AuthContext = createContext<AuthContextType | undefined>(
  undefined
);
```

Context를 생성하는 파일이며 여기서 Context를 정의하고 기본값을 설정한다.

> 💻 **`AuthProvider.tsx`**

{% raw %}

```tsx
import { useState } from "react";

import { AuthContext } from "./AuthContext";

type AuthProviderProps = {
  children: React.ReactNode;
};

const AuthProvider = ({ children }: AuthProviderProps) => {
  const [user, setUser] = useState<string | null>(null);

  const login = (username: string) => {
    setUser(username);
  };

  const logout = () => {
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export default AuthProvider;
```

{% endraw %}

Context의 `Provider`를 정의하여 자식 컴포넌트에서 `value`에 있는 속성들을 사용 가능하게 한다.

> 💻 **`UserStatus.tsx`**

{% raw %}

```jsx
import { useContext } from "react";

import { AuthContext } from "./AuthContext";

const UserStatus = () => {
  const authContext = useContext(AuthContext);

  const { user, login, logout } = authContext!;

  return (
    <div>
      {user ? (
        <>
          <p>환영합니다~ {user}님!</p>
          <button onClick={logout}>로그아웃</button>
        </>
      ) : (
        <>
          <p>로그인을 하세요!</p>
          <button onClick={() => login("Boongranii")}>로그인</button>
        </>
      )}
    </div>
  );
};

export default UserStatus;
```

{% endraw %}

`useContext` 훅을 사용해서 Context의 값을 가져와 사용한다.

> 💻 **`App.tsx`**

```jsx
import AuthProvider from "./AuthProvider";
import UserStatus from "./UserStatus";

const App = () => {
  return (
    <AuthProvider>
      <UserStatus />
    </AuthProvider>
  );
};

export default App;
```

위와 같이 `AuthProvider`로 애플리케이션을 감싸면 마무리가 된다. 그러나 `App` 컴포넌트 전체에 `AuthProvider`로 감싸버리면 제공 범위가 너무 넓어질 수 있으므로, Context가 필요한 컴포넌트들에만 적절하게 제공되도록 범위를 잘 설정해야 한다. 즉, Context를 어디에 제공할지 명확히 판단해서 필요한 곳에서만 사용해야 성능 최적화와 유지보수에 유리하다❗️

**전체적인 순서는 다음과 같다.**

1. `createContext()` 함수를 사용하여 Context 객체를 생성하고, 이 객체는 전역적으로 공유하고자 하는 데이터를 정의하는 것이다.
2. Context 객체의 `Provider`를 정의하여 하위 컴포넌트들이 사용 가능한 데이터를 제공 한다. `Provider`컴포넌트에서 `value` props를 통해 전달하고 싶은 데이터를 전달한다.
3. `Provider` 컴포넌트를 애플리케이션의 루트나 필요한 범위에 감싸서, 하위 컴포넌트들이 Context에 접근할 수 있도록 한다.
4. 하위 컴포넌트에서 `useContext`훅을 사용하여 Context의 값을 가져와서 사용한다.

이로써 React 애플리케이션에서 `Context API`를 사용하여 전역 상태 관리를 효율적으로 설정하고 사용할 수 있다.

---

# 4️⃣ 느낀점

오늘은 어제에 이어 `useDeferreValue` 훅에 대해 학습했다. `useTransition`훅과의 차이점과 공통점에 대해 알게 되었다.

오늘의 하이라이트인 메모이제이션을 통한 최적화 기법을 학습하였다. `React.memo`, `useCallback`, `useMemo`를 사용하여 사용하지 않는 컴포넌트의 재렌더링 방지 방법에 대해 알았다.

또한 props를 통한 `props-drilling`을 겪어 전역적으로 상태를 관리할 수 있는 `context API`에 대해서 학습했다. 너무 상태 관리를 위한 시작이 복잡해 요즘에는 잘 쓰지 않는 것 같다. 이를 활용하여 간단한 실습을 해보았고 최적화 기법을 통해 최적화까지 해보았다.

![image](https://github.com/user-attachments/assets/12c44dec-9141-4f40-9c7d-d871cafea3e1)<br />

팀 배정 결과도 나왔따. 한 달 동안 열심히 달려보좌🔥🔥🔥🔥🔥🔥

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
