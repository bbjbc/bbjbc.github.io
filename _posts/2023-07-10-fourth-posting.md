---
title: "[React] 클래스 컴포넌트 - 컨텍스트 및 오류 경계"
categories: [javascript, react]
tags: [javascript, react, component]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 네 번째 포스팅

안녕하세요! 네 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

세 번째 포스팅에 이어 과연 리액트 훅을 사용할 수 없는 **클래스 컴포넌트**에서는 `context`는 어떻게 사용할까?

알아보러 가시져 ㅎㅋ😉

Udemy의 React-The Complete Guide (incl Hooks, React Router, Redux) 강의를 바탕으로 작성된 글입니다.

## ☑️ 클래스 컴포넌트 컨텍스트?

컨텍스트를 사용하기 위해서는 먼저 `users-context.js` 파일을 만들어야겠죠?

```jsx
import React from "react";

const UsersContext = React.createContext({
  users: [],
});

export default UsersContext;
```

`createContext`를 호출하여 이렇게 정의를 할 수 있습니다. 그 후 괄호 안에 초기 값을 선언할 수 있고, `provider` 컴포넌트를 제공 가능합니다.

이에 따라 `App.js`에도 변화를 줄 수 있습니다.

```jsx
const DUMMY_USERS = [
  { id: "u1", name: "Max" },
  { id: "u2", name: "Manuel" },
  { id: "u3", name: "Julie" },
];

const App = () => {
  const usersContext = {
    users: DUMMY_USERS,
  };

  return (
    <UsersContext.Provider value={usersContext}>
      <UserFinder />
    </UsersContext.Provider>
  );
};

export default App;
```

`DUMMY_USERS`를 이 곳으로 가져와 값을 전달할 수 있습니다.

이 어플리케이션을 관리하는 컨텍스트로부터 데이터를 가져오도록 하겠습니다.

> 함수형 컴포넌트였다면?

```jsx
const ctx = useContext(UsersContext);
```

이런식으로 사용할 수 있었을 것입니다.

**하지만? 원하는건 클래스 컴포넌트잖아**
클래스 컴포넌트에선 훅 사용이 불가능 하다고 했습니다. 그 대신 다른 2가지 방법이 있습니다.

### 1️⃣ Context.consumer

이것은 `Context.consumer` 컴포넌트인데 함수형과 클래스 컴포넌트에 모두 사용 가능합니다.

> `UserFinder.js`

```jsx
render() {
    return (
      <UsersContext.Consumer> // 추가된 부분
        <div className={classes.finder}>
          <input type="search" onChange={this.searchChangeHandler.bind(this)} />
        </div>
        <Users users={this.state.filteredUsers} />
      </UsersContext.Consumer>
    );
  }
```

위처럼 Consumer 컴포넌트에 접근하여 사용하면 됩니다.
**하지만 이것은 `useReducer()`훅을 사용해야 하므로 여기서는 굳이 사용할 필요가 없게 됩니다.**

### 2️⃣ 정적 프로퍼티 추가

```jsx
static contextType = UsersContext;
```

리액트에게 이 컴포넌트는 UsersContext라는 컨텍스트에 접근할 수 있다고 전달하는 것입니다.
이 정적 `property`, 즉 `static contextType`는 단 한 번만 설정할 수 있으므로, 동시에 연결해야 하는 2개의 컨텍스트가 있다면 이것이 아닌 다른 방법을 찾아봐야 합니다.

추가로 `UserFinder.js` 에서는 더 이상 `DUMMY_USERS`가 사용되지 않으므로 컨텍스트를 통해 전달 하면 됩니다.

```jsx
this.context.users;
```

위처럼 말이죠.

사용하는 것은 간단하긴 하지만 유연함이 떨어진다고 볼 수 있습니다. 보통 1개의 컨텍스트를 사용하긴 하지만 혹시 2개 이상의 컨텍스트를 사용해야 한다면 제약 부담이 클 수 밖에 없습니다.

---

지금까지 클래스 컴포넌트로 어떻게 사용하며 props, render() 메소드, 상태 및 부작용을 피하기 위한 생명 주기 메소드를 다루는 방법과 컨텍스트를 다루는 방법까지 알아보았습니다.

좀 더 편한 걸 사용하는 것은 개발자의 몫입니다.
하지만 좀 더 유연하고 리액트 훅도 있는 함수형 컴포넌트를 주로 사용하긴 하죠.

**하지만 오류 경계를 다룰 때는 클래스 컴포넌트만을 사용해야 합니다.**

> **그 래 서 ?**

## ⛔️ 오류 경계?

리액트 컴포넌트에서 반환하는 `jsx`나 `render()` 메소드를 반환하던 중 **오류** 를 마주하게 된다면 렌더링을 멈추고 오류가 뜬 아래와 같은 화면을 볼 수 있게 됩니다.

![오류경계](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d4b291be-4d55-4590-b87b-ee40b4dcc421)

대체 화면을 보여주게 되는 것이 오류 경계를 말합니다.

일단 예시를 위해 `Users.js`에 다음 생명 주기 함수를 추가해줍니다.

```jsx
componentDidUpdate() {
    if (this.props.users.length === 0) {
      throw new Error("No Users provided!");
    }
  }
```

이것은 검색하고자 하는 사용자가 없을 때 오류를 띄우라는 말이다. 이를 통해 콜 스택에서 오류를 발생 시킵니다.
해결을 해주지 않으면 애플리케이션은 작동 중지를 하게 됩니다.

일반적인 자바스크립트에서는 예외 처리를 할 때 `try-catch`문을 사용합니다.

```jsx
try {
  someCodeWhichMightFail();
} catch (err) {
  // handle error
}
```

이런 식으로 말이죠.

> 하지만 컴포넌트에서 오류가 발생하고 처리할 수 없는 상황이라고 가정을 한다면?

→ 자식 컴포넌트가 아닌 부모 컴포넌트에서 처리한다고 한다면 `try-catch`문은 사용할 수 없을 것입니다.

`UserFinder.js`에서

```jsx
<Users users={this.state.filteredUsers} />
```

이 코드의 jsx에서 오류가 발생 했다고 볼 수 있습니다.
`try-catch`문은 정규 자바스크립트 문법이며 `jsx와` 함께 사용할 수 없으므로 위 코드를 `try-catch`로 감싼다고 해도 사용할 수 없을 것입니다.

> 그럼 어떻게 해야 하는데요?

**오류 경계를 만들면 되죠.(좀 멋진듯 ㅋ)ㅈㅅ**
오류 경계 컴포넌트에서는 **`componentDidCatch()` 생명 주기 메소드를 활용 해야 합니다!**

클래스 컴포넌트여야 하고 컴포넌트 생명 주기 메소드를 갖는 컴포넌트여야 합니다. ➕ 메소드를 넣어주면 클래스 컴포넌트를 **오류 경계**로 만들어 줍니다.

> `componentDidCatch()` 생명 주기 메소드의 특이점?

**→하위 컴포넌트 중 하나가 오류를 만들거나 전달할 때 발생하게 됩니다.**

> `ErrorBoundary.js`

```jsx
class ErrorBoundary extends Component {
  componentDidCatch() {}
  render() {
    return this.props.children; // 이 곳이 특이점임!!
}
```

위 코드에서 `this.props.children`을 return하는 이유는 오류 경계 컴포넌트를 우리가 보호하려고 하는 컴포넌트로 둘러싸야 하기 때문입니다.

> `UserFinder.js`

```jsx
<ErrorBoundary>
  <Users users={this.state.filteredUsers} />
</ErrorBoundary>
```

이런식으로 오류 경계 컴포넌트로 보호하고자 하는 컴포넌트를 wrap 할 수 있습니다.

```jsx
class ErrorBoundary extends Component {
  constructor() {
    super();
    this.state = { hasError: false };
  }

  // 에러를 만났을 때
  componentDidCatch(error) {
    console.log(error);
    this.setState({ hasError: true });
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong!</p>;
    }
    return this.props.children;
  }
}
```

이제 위처럼 오류 경계 로직을 짜주면 됩니다. 오류가 발생한다면 오류창이 아닌

```jsx
<p>Something went wrong!</p>
```

이렇게 렌더링 될 수 있게 만들어 줍니다.

이제 실행 화면을 보고 사용자 중 없는 알파벳을 검색하겠습니다.

![오류경계2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/fa2384c5-6e26-4e20-a180-e34ae117323d)

위와 같이 오류 처리를 하는 것을 볼 수 있습니다.

> **아까처럼 검정화면이 떠요!**<br/>
> 현재 개발용 서버라 그렇지 다음에 배포용 서버로 하면 뜨지 않고 바로 위 사진과 같이 렌더링 될 것입니다!
> 검정색 화면이 뜬다면 오버레이를 닫을 수 있으니 참고하시기 바랍니다!

#### ❓ 오류 경계를 사용하는 이유?

**오류가 발생해도 애플리케이션 전체가 작동 중단되지 않고 오류를 catch 후에 자바스크립트의 `try-catch`처럼 세련된 방식으로 처리 가능하기 때문입니다.**
