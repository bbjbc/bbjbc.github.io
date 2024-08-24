---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 5일차"
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

# 일흔 아홉 번째 포스팅

안녕하세요! 일흔 아홉 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 5일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ React 훅

React Hooks란 함수형 컴포넌트에서 상태와 생명주기 기능을 사용 가능하도록 해주는 기능이다.

이전에는 클래스형 컴포넌트에서만 상태 관리와 생명주기 메서드를 사용할 수 있었지만, React 16.8로 업데이트 되며 훅이 도입되면서 함수형 컴포넌트에서도 이 기능들을 사용할 수 있게 되었다.

## 💧 useState

`useState`훅은 함수형 컴포넌트 내에서 상태를 관리하기 위해 사용된다. 이 훅은 상태 변수와 상태를 업데이트할 수 있는 함수를 반환한다.

```jsx
const [state, setState] = useState(initialState);
```

`state`는 현재 상태 값을 나타내는 변수가 된다. `setState`는 상태를 업데이트하는 함수이다. `initialState`는 상태의 초기값을 나타낸다.

```jsx
import { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>현재 카운트는 {count}입니다.</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
};

export default Counter;
```

위와 같은 예제를 보면 `count`의 초기값을 0으로 설정하고 `count` 값을 업데이트할 수 있는 `setCount` 함수를 반환한다.

`onClick` 이벤트 핸들러를 통해 업데이트 함수인 `setCount`를 호출하여 `count` 값을 1씩 증가시킨다.

이제 사용자가 버튼을 클릭할 때마다 `count` 상태가 변경되며 컴포넌트가 재렌더링되어 화면에 새로운 `count` 값이 표시된다.

### 😸 불변성에 대한 고찰

React에서 `useState` 훅을 사용할 때 **"불변성을 지켜서 값을 변경해야 한다"**라는 말이 있다.

> 🐾 **불변성이 몽데요**

**불변성(Immutability)**은 객체나 데이터 구조가 한 번 생성되면 변경되지 않는 특성을 의미한다. 객체가 불변이라면 그 객체의 상태를 바꿀 수 없고, 상태를 변경하려면 새로운 객체를 만들어야 한다.

예를 들어서, JavaScript에서 배열을 불변적으로 수정하는 것은 기존 배열을 직접 수정하는 게 아니라, 새로운 배열을 반환하는 방법으로 이루어져야 한다.

```jsx
const arr = [1, 2, 3];
const newArray = arr.concat(4); // [1, 2, 3, 4]
```

위 코드에서는 원본 배열을 수정하지 않고 새로운 배열을 만들게 된다.

> 🐾 **리액트에서 불변성이 중요한 이유**

React는 상태가 변경될 때마다 이전 상태와 새로운 상태를 비교해서 어떤 부분이 변경되었는지 판단하고, 필요한 경우에 UI를 업데이트한다. **이 때 불변성을 유지하면** 상태가 변경되었는지 여부를 빠르게 알 수 있다.

불변성을 유지하는 경우에 React는 이전 상태와 새로운 상태의 참조가 다른지 비교할 수 있다. 참조가 다르면 상태가 변경된 것으로 판단하고 재렌더링을 수행한다. 이를 **참조 비교(Reference Equality)**라고 한다.

또한, 불변성을 지키면 **상태 변경이 예측 가능하고 쉽게 디버깅**할 수 있다. 불변 객체는 상태 변경이 일어나지 않으므로 이전 상태를 안전하게 보존할 수 있다.

> 🐾 **불변성 유지하며 상태를 업데이트 하는 방법**

React에서 상태를 업데이트할 때 불변성을 유지하는 방법은 **새로운 객체를 생성하고 이를 상태로 설정하는 것**이다.

```jsx
const [items, setItems] = useState([1, 2, 3]);

const addItem = (newItem) => {
  setItems((prev) => [...prev, newItem]);
};
```

위를 보면 `setItems`는 이전 상태를 사용하여 새로운 배열을 생성한다. **새로운 배열은 기존 배열을 복사하고 새로운 요소를 추가해서 불변성을 유지**한다.

```jsx
const [user, setUser] = useState({ name: "Bong", age: 25 });

const updateName = (newName) => {
  setUser((prevUser) => ({ ...prevUser, name: newName }));
};
```

위와 같이 객체 상태 업데이트도 불변성을 유지한 채 변경 가능하다.

> 🐾 **결론**

React에서 `useState`와 같은 상태 관리 훅을 사용할 때, 불변성을 유지하는 것은 예측 가능한 상태 관리와 성능 최적화를 위해 필수적이다.

상태를 불변적으로 관리하면 상태 변경을 쉽게 추적하고 효율적으로 재렌더링이 가능하다.

그러므로 **항상 새로운 객체나 배열을 생성하여 상태를 업데이트하고, 기존 상태를 직접 변경하지 않는 습관**을 들이는 것이 매우 중요하다❗️

### 😸 비동기적인 상태 업데이트

React의 상태 업데이트는 기본적으로 **비동기적**이다. 이는 React가 성능 최적화를 위해 여러 상태 업데이트를 한 번에 묶어서 처리하기 때문이다.

이를 통해 **한 번의 렌더링 사이클에서 처리하여 렌더링 최적화**를 이뤄내고, 비동기적으로 상태를 업데이트함으로써, **여러 컴포넌트에서 발생하는 상태 변경이 동기적으로 렌더링을 방해하지 않도록** 한다.

```jsx
const [count, setCount] = useState(0);

setCount(count + 1);
setCount(count + 1);
```

위 코드에서 `count + 1`을 두 번 실행하지만, 두 번째 `setCount`는 첫 번째 업데이트 후의 상태를 고려하지 않는다.

> 🐾 **동기적 상태 업데이트도 가능하다**

함수형 업데이트를 사용하면, 비동기적으로 실행되더라도 현재 상태를 기반으로 안전하게 계산된 새로운 상태를 설정할 수 있다.

```jsx
setCount((prevCount) => prevCount + 1);
setCount((prevCount) => prevCount + 1);
```

위 코드는 `setCount`가 함수형 업데이트를 사용하여 상태를 업데이트한다. 여기서 **React가 상태 업데이트 함수를 호출할 때마다 최신 상태를 전달**한다.

위 결과 상태는 `2`가 된다.

이렇게 함수형 업데이트를 사용하면 상태 업데이트가 **"동기적"**으로 동작하는 것처럼 보일 수 있다.

**결론적으로** `useState`의 상태 업데이트는 비동기적으로 처리되지만, **함수형 업데이트를 사용해서 현재 상태를 기반으로 동기적 계산**이 가능하다. 따라서 **복잡한 상태 업데이트 시에는 함수형 업데이트를 사용하여 안전하게 상태를 관리**할 수도 있다.

<br />

## 💧 커스텀 훅

React의 커스텀 훅은 **재사용 가능한 로직을 쉽게 관리**하고, **컴포넌트의 코드 중복을 줄이며, 더 깔끔하고 유지 보수하기 쉬운 코드를 작성**하는 데 도움을 주는 도구이다.

> 🐾 **커스텀 훅이 몽데요**

커스텀 훅은 이름이 `use`로 시작하는 JavaScript 함수이다. 이 함수는 하나 이상의 React 훅을 내부에서 호출하여 특정 로직이나 상태 관리를 캡슐화하고 재사용 가능하게 한다.

React 훅의 규칙을 따르기 때문에, 커스텀 훅도 훅의 규칙을 따라야 한다.

> 🐾 **커스텀 훅 구조**

```jsx
import { useState, useEffect } from "react";

const useCustomHook = (initialValue) => {
  const [value, setValue] = useState(initialValue);

  useEffect(() => {
    console.log("Value: ", value);
  }, [value]);

  return [value, setValue];
};

export default useCustomHook;
```

위와 같이, React 컴포넌트에서 반복적으로 사용되는 로직을 재사용 가능하게 모듈화할 때 유용하다.

커스텀 훅의 이름은 그 기능을 명확히 설명하는 것이 좋다. 예를 들어, 데이터 fetching을 담당하는 훅은 `useFetchData`, 브라우저의 로컬 저장소를 사용하는 훅은 `useLocalStorage`와 같이 이름을 지으면 좋다. 이렇게 하면 훅의 목적과 기능을 쉽게 이해할 수 있다.

> 🐾 **결론**

React의 커스텀 훅은 개발자가 공통 상태 관리나 로직을 쉽게 재사용할 수 있도록 돕는다.

이를 통해 **코드 재사용성을 높이고 컴포넌트를 더 깔끔하고 유지 보수**하기 쉽게 만들 수 있다. <br />
커스텀 훅을 사용하는 습관을 들이면 더 깨끗하고 효율적인 React 애플리케이션을 만들 수 있다.람지.

<br />

## 💧 useRef

`useRef`은 React의 훅 중 하나이다. 함수 컴포넌트에서 **변경 가능한 참조를 생성**하는 데 사용된다. 이를 사용하면 DOM 요소를 직접 참조하거나, 컴포넌트의 라이프사이클 동안 유지되는 변경 가능한 값을 저장할 수 있게 된다.

> 🐾 **왜 사용하는데요**

1. **DOM 요소 접근** <br />
   `useRef`는 특정 DOM 요소에 직접 접근해야 할 때 사용한다. React에서는 일반적으로 상태와 props를 통해 제어하지만, 때때로 DOM 요소에 직접 접근해야 할 때가 있다. (ex. 스크롤 위치 수동 조정, 텍스트 필드에 자동 포커스 설정)
2. **변경 가능한 값 저장** <br />
   `useRef`은 상태처럼 컴포넌트의 전체 렌더링 사이클 동안 값을 유지하지만, 상태와 달리 값이 변경되어도 컴포넌트를 다시 렌더링하지 않는다!

> 🐾 **useState와 차이점이 있나요**

```jsx
import { useRef, useState } from "react";

const UseRef = () => {
  console.log("UseRef rendering!");
  const count = useRef(0);
  const [count2, setCount2] = useState(0);

  return (
    <>
      <h1>useRef: {count.current}</h1>
      <h1>useState: {count2}</h1>
      <button onClick={() => (count.current += 1)}>useRef 증가</button>
      <button onClick={() => setCount2((count2) => count2 + 1)}>
        useState 증가
      </button>
    </>
  );
};

export default UseRef;
```

`useRef`와 `useState`의 차이점을 이해하기 위한 코드이다. 둘은 서로 다른 목적으로 사용되고, 컴포넌트 렌더링에 서로 다른 영향을 준다.

1. **`useRef`**
   - `useRef`는 변경 가능한 객체를 반환하고, `current` 프로퍼티를 통해 해당 객체에 접근 가능하다.
   - 컴포넌트가 재렌더링될 때 `useRef` 값이 유지된다. 하지만 값이 변경되어도 컴포넌트는 재렌더링되지 않는다.
   - 위 코드에서 `count.current` 값을 증가하는 버튼을 클릭하면 콘솔에 렌더링이 찍히지 않는다. 값이 변경되어도 컴포넌트가 재렌더링 되지 않기 때문이다.
1. **`useState`**
   - `useState`는 상태가 변경되면 컴포넌트는 재렌더링이 된다.
   - 위 코드에서 `setCount2`를 사용해서 `count2` 값을 증가시키면, 컴포넌트가 재렌더링되어 콘솔이 찍히게 된다.

![image](https://github.com/user-attachments/assets/b9cca3c1-a5e3-4de2-a178-9f3f99c0cd20) <br />

위 그림은 `useState 증가`를 눌렀을 때이다. 상태가 변경되어 컴포넌트가 재렌더링 되어 콘솔이 로그된 것을 확인할 수 있다.

![image](https://github.com/user-attachments/assets/c3635a3e-5c29-44d1-babe-35b57253b56a) <br />

위 그림은 `useRef 증가`를 9번 클릭 후 `useState 증가`를 한 번 클릭한 결과이다. `useRef`은 값이 변경되어도 재렌더링이 이루어지지 않기 때문에 `useState`를 누른 후 재렌더링이 이루어진 후 값이 클릭된 횟수만큼 변경된 것을 확인할 수 있다.

위 차이를 제대로 이해하면 언제 `useRef`를 사용하고, 언제 `useState`를 사용하는 것이 적절한지에 대한 감을 잡을 수 있을 것이다.

`useRef`는 렌더링 성능 최적화와 특정 DOM 접근이 필요한 경우에 사용하고, `useState`는 UI 업데이트와 연관된 상태 관리에 사용한다.

### ✊ 컴포넌트에서 `ref`를 사용하는 방법

React에서는 DOM 요소나 자식 컴포넌트에 직접 접근해야 할 때 `ref`를 사용한다. 하지만 컴포넌트에 `ref`를 전달할 때는 `ref`를 받을 수 있는 타입을 지정해야 제대로 사용할 수 있다.

> 💥 **`ref`와 `forwardRef`가 몽데요**

위에서 언급했듯이, `ref`는 React에서 DOM 요소나 자식 컴포넌트에 직접 접근할 수 있는 방법이다. `input` 태그에 `ref`를 사용하면 JavaScript를 통해 해당 요소에 직접 접근하여 조작이 가능하다.

하지만 일반적인 컴포넌트에는 `ref`를 바로 사용할 수 없으며, 이 경우에는 `forwardRef`라는 함수를 사용해야 한다.

`forwardRef`는 React에서 제공하는 고차 함수로 컴포넌트가 자신에게 전달된 `ref`를 DOM 요소나 다른 자식 컴포넌트에 전달할 수 있게 해준다.

> 💥 **`forwardRef`는 어떻게 사용하는데요**

`forwardRef`를 사용하면 `ref`를 부모 컴포넌트에서 자식 컴포넌트로 전달할 수 있다.

```tsx
import { forwardRef } from "react";

type TInputProps = React.ComponentPropsWithRef<"input">;

const Input = forwardRef<HTMLInputElement, TInputProps>((props, ref) => {
  const { ...rest } = props;

  return <input ref={ref} {...rest} />;
});

Input.displayName = "Input";

export default Input;
```

위 코드에서 `Input` 컴포넌트는 `forwardRef`를 사용하여 `ref`를 전달받고, 이를 `input` 요소에 전달한다. `forwardRef`를 사용할 때는 첫 번째 타입 인자로 `ref`가 참조할 수 있는 요소의 타입인 `HTMLInputElement`를, 두 번째 타입 인수로 컴포넌트의 props 타입인 `TInputProps`를 지정한다.

> 💥 **`displayName`은 왜 써용**

```jsx
Input.displayName = "Input";
```

`forwardRef`로 컴포넌트를 감싸면 React DevTools에서 디버깅할 때 컴포넌트 이름이 `ForwardRef`로 표시된다. 그러므로 `displayName`을 명시적으로 지정해줘야 디버깅이나 빌드 시 오류를 피할 수 있다.

> 💥 **`ComponentPropsWithRef` vs. `ComponentPropsWithoutRef`**

- **`ComponentPropsWithoutRef<T>`** <br />
  지정한 컴포넌트 타입 `T`의 props 타입을 추출하지만 `ref`는 포함되지 않는다.
- **`ComponentPropsWithRef<T>`** <br />
  지정한 컴포넌트 타입 `T`의 props 타입을 추출하며, `ref`도 포함된다.

위 둘은 성능상으로는 큰 차이는 없다. 이들은 타입 시스템에서만 작동하기 때문에 런타임 성능에 영향을 미치지 않는다. 대신에 어떤 props 타입이 필요한지에 따라 올바른 유틸리티 타입을 선택하는 것이 중요하다.

---

# 2️⃣ 느낀점

오늘은 어제에 이어 `useState` 훅에 대해서 깊게 알아보았다. `useState`훅을 통한 여러 가지 예제와 커스텀훅을 통한 재사용성 높이기 등을 실습하였다.

위 내용 외에도 `useRef`훅을 학습했다. `input` 태그에서의 컴포넌트에서 `ref` 전달하는 깊은 방법에 대해 배웠는데 좀 많이 복잡했다. 현재는 좀 많이 복잡했는데 React 19 버전에서는 이 어려운 과정이 해소된다고 한다.

현재 위 복잡한 내용은 없지만 이해하면 수정해서 추후에 업데이트 하도록 할 것이다.

![image](https://github.com/user-attachments/assets/b26c866b-0c4c-4d13-9054-080b5403be82) <br />

사전직무교육의 반이 끝났다. 일주차에는 React에 대한 기본 사항에 대해 학습했다. 조금 헷갈리고 잘 몰랐던 개념에 대해 배울 수 있어 좋은 경험을 했다.

다음주도 화이팅 해봅시다람지 💁👋

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
