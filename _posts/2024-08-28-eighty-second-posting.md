---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 8일차"
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

# 여든 두 번째 포스팅

안녕하세요! 여든 두 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 8일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 상태 관리 기법

## 💧 useReducer

`useReducer`는 React의 내장 훅으로 상태 관리 로직이 복잡할 때 유용하다. 특히 상태가 여러 가지로 변하거나, 여러 가지 상태를 다루는 경우에 `useState`보다 더 적합할 수 있다.

`useReducer`는 총 3개의 인자를 받는다.

1. **reducer 함수**: 현재 상태와 액션을 받아 새로운 상태를 반환하는 함수이다.
2. **초기 상태**
3. **(선택적) 초기화 함수**: 초기 상태를 생성하는 함수이다.

```jsx
const [state, dispatch] = useReducer(reducer, initialState, init);
```

위와 같은 형식을 갖춘다.

> 😧 **왜 사용하는데용**

- 상태 관리가 복잡하거나 여러 상태로 분리되어 있다면 코드가 더 구조적이고 가독성이 좋아진다.
- 상태 업데이트 로직을 한 곳에 모아, 상태 변경의 의도를 명확히 할 수 있다.
- 다른 전역 상태 관리 함수와 결합하면 더욱 풍부하고 쉽게 구현할 수 있다.

> 💻 **`cart-reducer.ts`**

```ts
type CartItem = {
  id: number;
  name: string;
  quantity: number;
};

type Action =
  | { type: "ADD_ITEM"; id: number; name: string }
  | { type: "REMOVE_ITEM"; id: number }
  | { type: "INCREMENT_QUANTITY"; id: number }
  | { type: "DECREMENT_QUANTITY"; id: number };

export const cartReducer = (state: CartItem[], action: Action): CartItem[] => {
  switch (action.type) {
    case "ADD_ITEM":
      return [...state, { id: action.id, name: action.name, quantity: 1 }];
    case "REMOVE_ITEM":
      return state.filter((item) => item.id !== action.id);
    case "INCREMENT_QUANTITY":
      return state.map((item) =>
        item.id === action.id ? { ...item, quantity: item.quantity + 1 } : item
      );
    case "DECREMENT_QUANTITY":
      return state.map((item) =>
        item.id === action.id ? { ...item, quantity: item.quantity - 1 } : item
      );
    default:
      throw new Error("올바르지 않은 액션입니다.");
  }
};
```

위처럼 reducer 함수를 지정해준다. `CartItem`, `Action`의 타입을 지정해주고 필요 데이터를 추가한다.

> 💻 **`shopping-cart`**

```jsx
import { useReducer } from "react";

import { cartReducer } from "./cart-reducer";

const ShoppingCart = () => {
  const [cart, dispatch] = useReducer(cartReducer, []);

  return (
    <>
      <h1>useReducer 장바구니</h1>
      <ul>
        {cart.map((item) => (
          <li key={item.id}>
            {item.name} x {item.quantity}
            <button
              onClick={() =>
                dispatch({ type: "INCREMENT_QUANTITY", id: item.id })
              }
            >
              증가
            </button>
            <button
              onClick={() =>
                dispatch({ type: "DECREMENT_QUANTITY", id: item.id })
              }
            >
              감소
            </button>
            <button onClick={() => dispatch({ type: "REMOVE_ITEM", id: item.id })}>삭제</button>
          </li>
        ))}
      </ul>
      <button
        onClick={() =>
          dispatch({
            type: "ADD_ITEM",
            id: Date.now(),
            name: `의류${cart.length + 1}`,
          })
        }
      >
        장바구니에 추가
      </button>
    </>
  );
};

export default ShoppingCart;
```

위처럼 아까 만든 `cartReducer`를 가져와 사용할 수 있다.

`cartReducer`는 리듀서 함수로 각 액션에 따라 상태를 업데이트하고 새로운 상태 배열을 반환한다.

![useReducer](https://github.com/user-attachments/assets/200e006c-250f-48a2-ae52-5c25880cc304) <br />

위와 같이 작동하는 것을 확인할 수 있다.

### 😯 `useState` vs. `useReducer`

|      특성      |        `useState`         |                       `useReducer`                       |
| :------------: | :-----------------------: | :------------------------------------------------------: |
|      용도      |     단순한 상태 관리      |   복잡한 상태 관리 및 여러 상태 변경 로직이 필요할 때    |
|   상태 타입    |          기본형           |          여러 속성으로 구성된 복잡한 상태 관리           |
| 상태 변경 방법 |        `setState`         |                        `dispatch`                        |
|    리팩토링    | 단순한 경우 리팩토링 쉬움 | 상태 변경 로직을 리듀서 함수로 분리해 쉽게 리팩토링 가능 |
|      성능      |  단순 상태 관리에서 우수  |            초기 상태 계산이 복잡한 경우 우수             |
|  상태 초기화   |  기본적으로 상태 값 설정  |               초기화 함수를 사용하여 계산                |

`useState`는 간단한 상태 관리와 단일 값 상태에 적합하지만, `useReducer`는 복잡한 상태와 여러 상태 변경 로직이 필요한 경우에 적합하며 상태 로직을 분리하고 예측 가능하게 관리 가능하다. 또한, 취향 차이일 수도❗️

<br />

## 💧 zustand

[**zustand 공식 홈페이지**](https://zustand-demo.pmnd.rs/)이다. 정말 웹페이지가 정말로 ~~항시적으로다가 리얼로다주~~ 내 취향이다. 곰이 ~~너무너무너무너무너무너무너무너무너무너무너무너무~~ 귀엽다. 키키

`zustand`는 정말 간단하고, 사용하기 쉽고, 가벼운 상태 관리 라이브러리이다. 이는 독일어로 "상태"를 의미하며, React 애플리케이션에서 컴포넌트 간의 상태를 효율적으로 관리하기 위해 설계되었다.

- `zustand`는 단순하고 직관적인 API를 제공한다. 기본적으로 상태를 생성하는 `create` 함수를 통해 간단한 훅 사용할 수 있다.
- `zustand`는 매우 가벼운 라이브러리로 크기가 정말 작다.
- `zustand`는 `context API`, `redux`보다 보일러플레이트 코드가 적어 상태를 전역적으로 쉽게 관리할 수 있도록 한다.
- `zustand`는 상태의 특정 부분에 대해 구독할 수 있어, 상태 변경이 필요한 컴포넌트만 다시 렌더링된다.

### 💨 설치

```jsx
npm install zustand
```

### 💨 예제

> 💻 **`use-store.ts`**

```ts
import create from "zustand";

interface CounterState {
  count: number;
  increase: () => void;
  decrease: () => void;
  reset: () => void;
}

export const useStore = create<CounterState>((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
  decrease: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));
```

위에서 `create` 함수를 사용해서 상태 스토어를 만든다. 여기서 `set`은 `zustand`의 상태 관리에서 사용하는 함수로, 상태를 업데이트하는 데에 사용된다.

- **객체 리터럴**을 사용해 상태를 업데이트 하는 경우

```tsx
const useStore = create<StoreState>((set) => ({
  count: 0,
  setCount: (count: number) => set({ count }),
}));
```

위와 같이 `set`을 호출하여 기존 상태의 다른 부분을 변경하지 않고 `count`만 업데이트 가능하다.

- **함수**를 사용해 상태를 업데이트 하는 경우

이는 위 예제에서 사용한 것처럼 현재 상태인 `state`를 인자를 받아 새로운 상태를 반환하는 함수로 사용된다. 이를 통해 현재 상태에 기반해 새로운 상태를 계산하고 설정 가능하다.

> 💻 **`counter.tsx`**

```jsx
import { useStore } from "./use-store";

const Counter = () => {
  const { count, increase, decrease, reset } = useStore();

  return (
    <>
      <h1>Count: {count}</h1>
      <button onClick={increase}>증가</button>
      <button onClick={decrease}>감소</button>
      <button onClick={reset}>초기화</button>
    </>
  );
};

export default Counter;
```

위와 같이 하면

![zustand](https://github.com/user-attachments/assets/783d087a-72f9-42dc-81eb-ce9bdec45148) <br />

위와 같이 제대로 빠르게 작동하는 것을 확인할 수 있다.

```jsx
const { count, increase, decrease, reset } = useStore();
```

하지만 위와 같이 구조분해 할당을 사용하면 **최적화에 좋지 않은 영향을 줄 수 있다**. 왜냐하면 구조분해 할당을 사용하면 해당 컴포넌트가 `zustand` 스토어의 모든 상태 변경에 대해 재렌더링되기 때문이다. 즉, 상태의 일부만 변경되더라도 컴포넌트 전체가 다시 렌더링된다.

`zustand`는 상태의 특정 부분에 대해서만 **구독**할 수 있다. 이것을 위해서 **개별 상태와 업데이트 함수를 구조분해 할당하지 않고 각각 `useStore` 훅을 통해 가져와야** 한다. 이렇게 하면 **상태의 특정 부분이 변경될 때만** 해당 상태를 사용하는 컴포넌트만 재렌더링된다.

```jsx
const count = useStore((state) => state.count);
const increase = useStore((state) => state.increase);
const decrease = useStore((state) => state.decrease);
const reset = useStore((state) => state.reset);
```

그래서 위와 같이 개별적으로 나누어 선언을 해준다.

결론적으로 이렇게 `zustand`의 구독 기능을 활용해 **불필요한 렌더링을 줄이고 성능을 최적화할** 수 있다❗️

---

# 2️⃣ 데이터 통신

## 🍦 기본적인 fetch

```jsx
import { useState, useEffect } from "react";

const FetchBasic = () => {
  const [todo, setTodo] = useState(null);
  const [count, setCount] = useState(0);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/todos")
      .then((response) => response.json())
      .then((json) => setTodo(json));
  }, []);

  return (
    <div>
      <h1>Fetch Basic</h1>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>증가</button>
      <pre>{JSON.stringify(todo, null, 2)}</pre>
    </div>
  );
};

export default FetchBasic;
```

보통 위와 같이 `useEffect()` 훅을 사용하여 데이터를 페치한다. `useEffect`훅을 사용하지 않으면 네트워크 탭을 보면 아래와 같이 계속 받아오는 것을 볼 수 있다.

![fetchbasic](https://github.com/user-attachments/assets/dfdf19d5-d07f-43c7-91e1-250c7ffeb13c) <br />

이와 같이 계속해서 데이터가 페치되는 이유는 컴포넌트가 **매번 렌더링될 때마다** 데이터 페치 로직이 실행되기 때문이다. 상태가 업데이트되고 컴포넌트가 다시 렌더링될 때마다 `fetch` 호출이 반복적으로 이루어지며, 이로 인해 **무한 페치**가 발생하게 된다.

이를 방지하기 위해서는 `useEffect` 훅을 사용하여 데이터 페치 로직이 컴포넌트가 처음 렌더링될 때만 실행되도록 설정해야 한다. `useEffect`에 **빈 의존성 배열**을 전달하면, 이 훅은 초기 렌더링 시에만 실행되며 이후에는 실행되지 않는다. 이를 통해 데이터가 한 번만 페치되도록 보장할 수 있다.

<br />

## 🍦 깜빡임 현상

**CSR(Client-Side Rendering)** 방식에서는 사용자가 페이지를 처음 로드할 때 서버에서 데이터가 오기 전에 화면이 렌더링되기 때문에, 새로고침 시 깜빡이거나 데이터를 가져오기 전에는 `null` 상태를 보게 된다. 이는 사용자가 데이터를 기다리는 동안 빈 화면이나 로딩 상태를 경험하게 된다.

![lazy](https://github.com/user-attachments/assets/9d0061cd-9dc3-4cfe-ae0b-b3ea06d5a552) <br />

반면, **SSR(Server-Side Rendering)**은 서버에서 페이지를 렌더링하여 필요 데이터를 미리 가져오기 때문에, 클라이언트 측에서 새로고침을 하더라도 데이터가 즉시 렌더링된다. 결과적으로 초기 로드 시에 더 좋은 UX를 제공할 수 있다.

SSR을 사용하면 서버가 필요한 데이터를 먼저 가져오고, 이를 통해 완성된 HTML을 클라이언트에 보내서 초기 로딩 시 데이터를 기다릴 필요 없이 완전한 페이지가 렌더링된다. 이를 통해서 SSR에서는 깜빡임 문제를 해결할 수 있다.

> 🍫 **깜빡임 현상은 어떻게 해결해요**

1. 데이터가 로드되는 동안 **로딩 스피너 및 스켈레톤이나 메시지를 표시**하여 사용자가 데이터를 기다리고 있음을 표시해야 한다.
2. 초기 상태에 **기본 데이터나 이전에 캐시된 데이터를 설정**해서 사용자가 새로운 데이터를 로드할 때까지 빈 화면이 보이지 않도록 한다
3. **Tanstack Query나 데이터 페칭 라이브러리들을 통해** 캐싱, 동기화를 효율적으로 처리하여 로딩 중 상태 관리나 캐싱을 쉽게 할 수 있다.

## 🍦 로딩 핸들링

```jsx
import { useState, useEffect } from "react";

const FetchBasic = () => {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    setIsLoading(true);
    fetch("http://localhost:3000?t=3000")
      .then((response) => response.json())
      .then((json) => {
        setData(json);
        setIsLoading(false);
      });
  }, []);

  return (
    <div>
      <h1>로딩 핸들링</h1>
      {isLoading ? (
        <h1>Loading...</h1>
      ) : (
        <pre>{JSON.stringify(data, null, 2)}</pre>
      )}
    </div>
  );
};

export default FetchBasic;
```

위 api는 3초 뒤에 렌더링 되도록 하는 것이다. `isLoading` 상태를 통해 값이 `true`라면 메시지를 띄우고 아니라면 데이터 내용을 띄우는 것이다.

![loading](https://github.com/user-attachments/assets/7f77ab92-f14c-4088-8beb-46c7d7d68d47) <br />

위와 같이 3초동안 지연된 후 데이터가 나오는 것을 확인할 수 있다.

<br />

## 🍦 에러 핸들링

만일 api의 url이 잘못되어 로딩이 계속된다면 `에러 핸들링`을 해야 한다.

```jsx
import { useState, useEffect } from "react";

const FetchBasic = () => {
  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    setIsLoading(true);
    setError(null);

    fetch("http://localhost:3000/4?t=3000")
      .then((response) => {
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
      })
      .then((json) => {
        setData(json);
        setIsLoading(false);
      })
      .catch((error) => {
        setError(error.message);
        setIsLoading(false);
      });
  }, []);

  let content;
  if (isLoading) {
    content = <h1>Loading...</h1>;
  } else if (error) {
    content = <h1>Error: {error}</h1>;
  } else {
    content = <pre>{JSON.stringify(data, null, 2)}</pre>;
  }

  return (
    <div>
      <h1>로딩 및 에러 핸들링</h1>
      {content}
    </div>
  );
};

export default FetchBasic;
```

위와 같이 잘못된 api라면 에러 핸들링을 `try-catch`문을 통해서 추가해준다. `fetch` 요청의 응답을 처리할 때, 응답의 `ok` 속성을 사용하여 HTTP 상태 코드가 성공 범위(200-299)에 있는지 확인한다. 성공 범위 밖이라면 `throw new Error()`를 사용해 에러를 발생시킨다.

![image](https://github.com/user-attachments/assets/695c38d0-e75f-482c-bf9f-e848c445f551) <br />

`error`를 관리하는 상태를 추가하여 에러가 발생한다면 에러 관련 내용을 렌더링 하는 것이다.

```jsx
useEffect(() => {
  const fetchData = async () => {
    setIsLoading(true);
    setError(null);

    try {
      const response = await fetch("http://localhost:3000?t=3000");
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const json = await response.json();
      setData(json);
    } catch (error) {
      setError(error.message);
    } finally {
      setIsLoading(false);
    }
  };

  fetchData();
}, []);
```

위에 있던 코드를 `async-await`을 사용해서 리팩토링하였다. `fetchData`라는 이름의 비동기 함수를 `useEffect` 내부에 정의하여 비동기 함수를 선언하고 `await` 키워드를 사용 가능하다.

`await`을 사용해서 해당 작업이 완료될 때까지 기다린다. 이를 사용하면 프로미스가 해결될 때까지 함수 실행이 잠시 멈추기 때문에 `then` 체인을 사용하지 않아도 된다.

이렇게 하면 비동기 코드를 더 간결하고 가독성 있게 작성 가능하다.

---

# 4️⃣ 느낀점

오늘은 어제에 이어 상태 관리 기법에 대해 알아보았다. `useReducer`와 `zustand`에 대해 학습했고 `useReducer`는 `useState`와 비슷하게 쓰여 상황에 맞게 잘 골라 쓰면 된다. 최근 가장 핫한 전역적 상태 관리인 `zustand`를 경험해보았는데 정말 간단하게 상태 관리가 가능해서 신기했다.

또한, 데이터 통신을 `fetch`를 통해 하는 방법을 학습했다. CSR에서의 깜빡임 현상의 원인, 로딩 핸들링하는 방법, 에러 핸들링 하는 방법에 대해 알아보았고 간단한 실습을 진행하였다.

![image](https://github.com/user-attachments/assets/61bd39f4-21c2-45f3-8b80-97ddb371aab2)<br />

이제 사전 직무 교육은 내일이 마지막이다. 지금까지 배운 내용을 토대로 마지막 실습을 할 예정이다. 끝까지 화이팅🔥🔥🔥🔥🔥🔥

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
