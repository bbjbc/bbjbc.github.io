---
title: "[Redux] Dive into Redux"
categories: [redux, react]
tags: [redux, react, context]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 스물 한 번째 포스팅

안녕하세요! 스물 한 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

후기 이외에 포스팅하는건 되게 오랜만이네요! 날씨가 진짜 정말 추워졌어요🥶<br>
다들 몸관리 잘 하시고 감기 걸리지 않게 조심해요💪

오늘의 포스팅 내용은 **리덕스(Redux)**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 React-The Complete Guide (incl Hooks, React Router, Redux) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ 리덕스

아주 유명한 서드파티 리액트 라이브러리인 리덕스 라이브러리에 대해 알아보겠습니다❗️

## ⏩ 리덕스란

**리덕스는 Cross-Component 또는 App-wide State를 위한 상태 관리 시스템**입니다. 화면에 렌더링되는 데이터를 관리하도록 도와주는 역할을 합니다. <br>
상태 관리를 위해서 `useState()`와 `useReducer()`와 같은 훅을 사용했습니다.

<br>

## ⏩ 3가지 상태

> **Local State** <br>
> 범위는 해당 컴포넌트 내에서만 유효합니다. <br>
> 로컬 상태는 주로 컴포넌트 내부에서 발생하는 상태를 나타냅니다. 이 상태는 해당 컴포넌트 내에서만 유지되며, 다른 컴포넌트에서는 직접 접근할 수 없습니다. 주로 `useState()`훅(또는 `useReducer()` 훅 사용)을 사용하여 변경합니다.

```jsx
import React, { useState } from "react";

const ExampleComponent = () => {
  const [localState, setLocalState] = useState(value);

  // ...
};
```

> **Cross-Component State** <br>
> 범위는 여러 컴포넌트 간에 공유가 가능합니다. <br>
> 여러 컴포넌트 간에 공유되는 상태를 말하며 주로 부모 컴포넌트에서 상태를 관리하고, 필요한 하위 컴포넌트로 전달하는 방식으로 구현됩니다. 이것을 통해 여러 컴포넌트 간에 데이터를 전달하고 동기화할 수 있습니다. 하지만 prop chain이나 prop drilling과 같은 문제를 야기할 수 있습니다.

```jsx
import React, { useState } from "react";
import ChildComponent from "./ChildComponent";

const ParentComponent = () => {
  const [crossComponentState, setCrossComponentState] = useState(value);

  return (
    <div>
      <ChildComponent
        state={crossComponentState}
        setState={setCrossComponentState}
      />
      {/* 다른 하위 컴포넌트들 */}
    </div>
  );
};
```

> **Application-Wide State** <br>
> 범위는 애플리케이션 전체에서 공유됩니다. <br>
> 전역 상태는 애플리케이션 전체에서 공유되는 상태를 말합니다. Redux, Context API 등을 사용하여 구현 가능합니다. 이를 통해 여러 컴포넌트 간에 데이터를 전역적으로 공유하고 상태를 효율적으로 관리할 수 있습니다.

```jsx
import React, { createContext, useContext, useState } from "react";

const AppContext = createContext();

const AppProvider = ({ children }) => {
  const [appWideState, setAppWideState] = useState(value);

  return (
    <AppContext.Provider value={(appWideState, setAppWideState)}>
      {children}
    </AppContext.Provider>
  );
};

const useAppContext = () => {
  return useContext(AppContext);
};
```

애플리케이션의 최상위 컴포넌트에서 AppProvider를 사용하여 앱 전체 상태를 관리합니다.

위와 같은 3가지 상태 관리 방식으로 통해 React 애플리케이션에서 상태를 효과적으로 관리할 수 있습니다. 방식 선택은 애플리케이션의 크기와 복잡성과 선호도에 따라 달라집니다.

<br>

## ⏩ 리덕스 작동 방식

![udemy-redux](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/5b892f9e-b194-4c2a-9000-c36d6d5c0ac4)<br>

**Redux**는 JavaScript 애플리케이션 상태 관리 라이브러리로, 주로 React와 함께 사용됩니다.

> 1. **상태(State)** <br>
>    Redux는 애플리케이션의 상태를 중앙 저장소(store)에 저장합니다. 상태는 일반 JavaScript 객체로 표현되며, 애플리케이션의 전역 상태를 나타내는 중요한 데이터를 포함합니다.
> 2. **액션(Action)** <br>
>    액션은 상태 변경을 나타내는 객체입니다. 액션은 일반적으로 Type과 데이터를 포함하는 객체로 정의합니다.
> 3. **리듀서(Reducer)** <br>
>    리듀서는 이전 상태와 액션을 받아서 새로운 상태를 생성하는 함수입니다. 이전 상태와 액션만을 기반으로 새로운 상태를 계산합니다.
> 4. **스토어(Store)** <br>
>    Redux 스토어는 애플리케이션의 전역 상태를 보유하고, 상태를 변경하는 액션을 디스패치할 수 있는 메서드를 제공합니다. 스토어는 또한 상태가 변경될 때 리듀서를 호출하여 새로운 상태를 생성하고, 이를 구독하는 컴포넌트에게 알리게 됩니다.
> 5. **컴포넌트와 연결** <br>
>    React 컴포넌트는 Redux 스토어와 연결이 가능합니다. 이를 통해 컴포넌트는 스토어의 상태를 읽고 액션을 디스패치하여 상태 변경이 가능합니다. 변경된 상태는 자동으로 컴포넌트에 반영되어 UI가 업데이트 됩니다.

Redux는 상태 관리를 중앙화하고 예측 가능하게 만들어 복잡한 애플리케이션 상태를 관리하기에 유용합니다. 이러한 구성 요소들이 함께 작동하여 Redux는 애플리케이션의 상태 변화를 추적하고 관리하도록 돕습니다.

<br>

## ⏩ 리덕스 vs. 리액트 컨텍스트

하나의 어플리케이션 안에서 리액트 컨텍스트와 리덕스 모두 사용할 수 있습니다. 일반적으로 앱 와이드 상태에서는 리덕스를 사용하고 중요한 일부 다중 컴포넌트 상태에는 컨텍스트를 사용할 수 있습니다.

리덕스는 크로스 컴포넌트 상태 또는 앱 와이드 상태를 위한 상태 관리 시스템입니다. 리액트 컨텍스트를 사용하면 prop chain이나 prop drilling을 막을 수 있고 ContextProvider 컴포넌트를 중심으로 상태를 관리 가능합니다. <br>
이미 다수의 컴포넌트에 영향을 미치는 상태를 관리하는 React Context API가 있는데 왜 리덕스가 필요할까요❓

리액트 컨텍스트는 잠재적인 단점이 존재합니다.

> **중첩된 JSX 코드** <br>

리액트 컨텍스트를 사용하면 설정이 복잡해질 수 있으며 리액트 컨텍스트를 이용한 상태 관리가 복잡해질 가능성이 높아집니다. <br> 대형 애플리케이션을 구축하여 사용한다면 이러한 코드가 나올 수 있습니다.

```jsx
return (
  <AuthContextProvider>
    <ThemeContextProvider>
      <UIInteractionContextProvider>
        <MultiStepFormContextProvider>
          <UserRegistration />
        </MultiStepFormContextProvider>
      </UIInteractionContextProvider>
    </ThemeContextProvider>
  </AuthContextProvider>
);
```

아주 다양하고 많은 컨텍스트와 다수의 컴포넌트나 상태가 존재합니다. 결과적으로 심하게 중첩된 JSX 코드가 나오게 됩니다. 하나의 컨텍스트가 모든 것을 관리할 수도 있지만 그렇게 된다면 유지보수가 어려워질 것이고 많은 것을 관리해야 하기 때문에 단점이 됩니다.

> **성능의 문제점** <br>

![redux1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/59c6e49b-8ac8-48c2-ad29-e1f0e68005fa)<br>

리액트 팀원이 올린 공식 언급입니다. 현재 사용하고 있는 컨텍스트를 말하고 있습니다. **테마를 변경하거나 인증 같은 저빈도 업데이트에는 아주 좋지만, 데이터가 자주 변경되는 경우에는 좋지 않다고 언급**되어 있습니다. <br>
또한 새 컨텍스트는 유동적인 상태 확산을 대체할 수 없다고도 언급하고 있습니다.

리덕스는 유동적인 상태 관리 라이브러리입니다. <br>
결국, 위 팀원은 리액트 컨텍스트가 모든 시나리오와 모든 경우에서 리덕스를 훌륭하게 대체할 수 없다는 것입니다.

---

# 2️⃣ 리덕스 적용

## ☑️ 의존성 추가

```js
npm install redux react-redux
```

리덕스를 사용해야 하기 때문에 redux 패키지를 설치합니다. 그리고 리덕스는 리액트에서만 쓰는 것이 아닙니다. 리덕스는 어떤 자바스크립트 프로젝트에서도 사용 가능합니다. 그래서 리액트 앱과 리덕스의 작업을 쉽게 하기 위해서 react-redux 패키지를 설치합니다.<br>
이것은 리액트 앱과 리덕스 스토어와 리듀서에 접속하게 하며 리덕스 스토어에 컴포넌트를 subscribe합니다.

<br>

## ☑️ React용 Redux 저장소 생성

store폴더에 index.js파일을 생성합니다. 이 곳에는 리덕스 로직을 저장하는 것입니다.

```jsx
import { createStore } from "redux";

const counterReducer = (state = { counter: 0 }, action) => {
  if (action.type === "increment") {
    return {
      counter: state.counter + 1,
    };
  }
  if (action.type === "decrement") {
    return {
      counter: state.counter - 1,
    };
  }
  return state;
};

const store = createStore(counterReducer);

export default store;
```

저장소를 만들어주는 `createStore`를 import 합니다.
`createStore`에는 포인터가 있어야 합니다. 카운터리듀서를 위와 같이 설정하며 액션 타입에 따라서 카운터의 상태가 계속 변경될 것입니다. <br>
store 상수에 리덕스 스토어를 만들 수 있습니다.

이제 store를 index.js 파일 외부에서 사용 가능하며 리액트 앱을 이 store에 연결해야 합니다.

<br>

## ☑️ 스토어 제공

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Provider } from "react-redux";

import "./index.css";
import App from "./App";
import store from "./store/index";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

리덕스 저장소를 리액트 앱에 제공하기 위해서 앱 전체를 렌더링하는 `index.js` 파일에 무엇을 작성해야 합니다. <br>
리액트 애플리케이션에서 컴포넌트 트리의 최상단은 루트 컴포넌트를 렌더링한 이 파일입니다.

애플리케이션 전체를 저장소에 액세스하기 위해서는 루트 컴포넌트에서 제공하면 됩니다.
react-redux로부터 Provider를 import 합니다. 루트 컴포넌트인 App을 Provider로 감싸줍니다. 이제 감싸진 App의 하위 컴포넌트들은 리덕스에 액세스가 가능합니다.

하지만 이렇게 Provider로 래핑해서 저장소를 리액트에게 알리는 것이 아닙니다. Provider 안에는 `store` prop이 존재합니다. value에 위에서 구축한 store를 넣어주면 이제 리덕스 저장소에 이 리액트 앱을 제공할 수 있게 됩니다.

<br>

## ☑️ 리덕스 데이터 사용

```jsx
import { useSelector } from "react-redux";

const Counter = () => {
  const toggleCounterHandler = () => {};

  return (
    <main>
      <h1>Redux Counter</h1>
      <div>-- COUNTER VALUE --</div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};

export default Counter;
```

위 코드에서 현재의 카운터를 출력하기 위해서 리덕스 저장소와 그 안에 있는 데이터에 액세스해야 합니다.<br>
react-redux 가 만든 커스텀 훅인 `useSelector()` 훅을 import 해줍니다.

> **useSelector() 훅** <br>
> useSelector() 훅은 redux가 실행시키며 저장소에서 데이터를 받기 위해서 사용합니다. 이것은 함수이며 react-redux가 실행합니다.

리덕스가 관리하는 상태를 `useSelector()` 훅에 함수로 넣어줍니다.

```jsx
const Counter = () => {
  const counter = useSelector((state) => state.counter);

  const toggleCounterHandler = () => {};

  return (
    <main>
      <h1>Redux Counter</h1>
      <div>{counter}</div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
```

관리된 데이터를 함수에 넣고 필요로 하는 상태를 받게 되며 useSelector 전체가 그 리턴된 값을 우리에게 전달해 줍니다.

또한, useSelector를 사용할 때 react-redux는 컴포넌트를 위해 리덕스 저장소에 자동으로 구독을 설정해 줍니다. 그래서 컴포넌트는 리덕스 저장소에서 데이터가 변경될 때마다 자동으로 업데이트되고 최신 카운터를 받을 수 있는 것입니다. 리덕스 저장소가 변경되면 컴포넌트 함수는 재실행 되며 계속 최신 카운터를 갖게 되는 것입니다.

따라서 저장소에서 데이터를 받기 위해 이 훅을 사용하는 것입니다.

그럼 이제 데이터에 액세스를 했는데 이 데이터를 어떻게 변경할 수 있을까요? 어떻게 액션을 발송할 수 있을까요?

<br>

## ☑️ Action Dispatch

```jsx
import { useSelector } from "react-redux";

const Counter = () => {
  const dispatch = useDispatch();
  const counter = useSelector((state) => state.counter);

  const toggleCounterHandler = () => {};

  return (
    <main>
      <h1>Redux Counter</h1>
      <div>{counter}</div>
      <div>
        <button>Increment</button>
        <button>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
```

이제 버튼이 동작하기 위한 액션을 발송해야 하는데 React 컴포넌트 내부에서 어떻게 작동할까요?

또 다른 훅을 사용하면 됩니다. 바로 `useDispatch()` 훅입니다. 위처럼 어떠한 인자도 전달하지 않고 실행가능한 dispatch function을 반환합니다. dispatch 상수는 결국 호출 가능한 함수입니다. **이 함수는 Redux store에 대한 action을 발송합니다.**

```jsx
import { useSelector, useDispatch } from "react-redux";

const Counter = () => {
  const dispatch = useDispatch();
  const counter = useSelector((state) => state.counter);

  const incrementHandler = () => {
    dispatch({ type: "increment" });
  };

  const decrementHandler = () => {
    dispatch({ type: "decrement" });
  };

  const toggleCounterHandler = () => {};

  return (
    <main>
      <h1>Redux Counter</h1>
      <div>{counter}</div>
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
```

버튼 핸들러 안에 dispatch 함수를 호출하고 타입은 store에서 생성했던 action.type과 동일하게 작성해야 합니다.

<br>

## ☑️ 상태 관리

위에서는 하나의 카운터 value로만 작업을 했습니다. toggle버튼을 눌러 토글링 되도록 하기 위해서 리덕스 상태 관리를 해보겠습니다.

```jsx
import { createStore } from "redux";

const initialState = { counter: 0, showCounter: true };
const counterReducer = (state = initialState, action) => {
  if (action.type === "increment") {
    return {
      counter: state.counter + 1,
      showCounter: state.showCounter,
    };
  }
  if (action.type === "decrement") {
    return {
      counter: state.counter - 1,
      showCounter: state.showCounter,
    };
  }
  if (action.type === "toggle") {
    return {
      counter: state.counter,
      showCounter: !state.showCounter,
    };
  }
  return state;
};

const store = createStore(counterReducer);

export default store;
```

위와 같이 showCounter를 통해 참/거짓으로 토글버튼을 통해 increment, decrement 버튼이 숨겨짐을 확인할 것입니다.

`[Counter.js]`

```jsx
const show = useSelector((state) => state.showCounter);

const toggleCounterHandler = () => {
  dispatch({ type: "toggle" });
};

    //...
return (
  {show && <div>{counter}</div>}
)
```

위와 같은 코드를 추가하면 이제 상태가 바뀌어 토글버튼이 활성화 되는 것을 확인할 수 있습니다.

이렇게 여러 개의 데이터를 리덕스에서 state를 통해 관리가 가능합니다.

---

오늘은 Redux에 대해서 알아보았습니다. Redux를 React앱에 적용시키는 것도 살펴보았고 Redux와 React Context API의 차이점에 대해서 살펴보았습니다.

조금 생소하지만 잘 숙지하도록 반복해서 봅시다👌

|     특성      |                          Redux                           |                         React Context                         |
| :-----------: | :------------------------------------------------------: | :-----------------------------------------------------------: |
|   상태 관리   |               중앙 집중화된 전역 상태 관리               |              컴포넌트 트리를 통한 전역 상태 관리              |
|    사용성     |            복잡한 설정이 필요하나 높은 유연성            |           상대적으로 간단한 설정, 일반적으로 간편함           |
| 액션과 리듀서 |          액션과 리듀서를 사용하여 상태 업데이트          | Context.Provider 및 Context.Consumer를 사용하여 상태 업데이트 |
|     성능      |             미들웨어를 사용하여 최적화 가능              |                성능은 상대적으로 낮을 수 있음                 |
|     적합      | 대규모 애플리케이션 또는 복잡한 상태 관리, 잦은 업데이트 | 중소규모 애플리케이션 또는 단순한 상태 관리, 저빈도 업데이트  |
|   공식 지원   |                 React와 독립적으로 존재                  |                      React의 일부로 제공                      |

긴 게시물 읽어주셔서 감사합니다👀
