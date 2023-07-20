---
title: "[React] 클래스 컴포넌트 구축 방법"
categories: [react]
tags: [javascript, react, component]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 두 번째 포스팅

안녕하세요! 두 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

React에서는 주로 **함수형 컴포넌트**를 사용합니다. 하지만 기호에 따라 **클래스형 컴포넌트**를 사용하는 개발자도 있습니다.

오늘은 클래스 컴포넌트에 관해서 설명 드리도록 하겠습니다.

Udemy의 React-The Complete Guide (incl Hooks, React Router, Redux) 강의를 바탕으로 작성된 글입니다.

## 1️⃣ What & Why?

### What?

**함수형 컴포넌트**

```jsx
const Wallet = (props) => {
  return <h2>A Wallet!</h2>;
};
```

**클래스 컴포넌트**

```jsx
class Wallet extends Component {
  render() {
    return <h2>A Wallet!</h2>;
  } // 무엇이 화면에 렌더링 되어야 하는지를 평가함
}
```

> 위와 같은 형식의 컴포넌트를 클래스 컴포넌트 or 클래스 기반 컴포넌트라고 합니다.
> 함수형 컴포넌트가 Default이며 가장 최신의 접근 방법입니다.

### Why?

> 최근에는 대부분의 개발자들은 함수형 컴포넌트를 사용합니다.
> **오류 경계의 예외 때문에 클래스 기반 컴포넌트를 사용할 이유가 없기 때문입니다.**
> 하지만, 개인적인 선호도에 따라 사용하기도 합니다.

> 과거, 리액트 16.8버전 이전에 이러한 것들을 사용하는 시나리오와 유스케이스 같은 것들이 존재 했었습니다. 특히 상태를 처리하고 사이드 이펙트를 다룰 때 클래스 컴포넌트를 사용 했어야 했습니다. 16.8 이전에는 함수 컴포넌트에서는 상태 변경이 불가능했고 사이드 이펙트 또한 다룰 수 없었기 때문입니다.
> 16.8 이후 리액트에서는 리액트 훅이라는 훅에 대한 개념을 도입한 후로 함수형 컴포넌트를 사용하게 되었습니다.(ex. `useState`, `useEffect`, `useContext`, ...)

**Importance!!**
**클래스 컴포넌트에서는 리액트 훅을 사용할 수 없습니다.**

## 2️⃣ Class Component - 빌드

다음은 일반적으로 리액트에서 사용하는 함수형 컴포넌트입니다.

```jsx
import classes from "./User.module.css";

const User = (props) => {
  return <li className={classes.user}>{props.name}</li>;
};

export default User;
```

이것을 클래스 컴포넌트로 변환하면 다음과 같습니다.

```jsx
import { Component } from "react"; //props 대신 extends Component를 해줌

import classes from "./User.module.css";

class User extends Component {
  render() {
    return <li className={classes.user}>{this.props.name}</li>;
  }
}
```

class명으로는 컴포넌트 이름을 삽입하고 중괄호로 닫으면 됩니다. (마치 java)
class 안에 있는 render() 메소드 안에는 반환값을 넣으면 됩니다. (render 메소드는 리액트에 있는 특정 메소드임)
**하지만 문제되는 점이 있습니다. 그것은 바로 props죠.**
props는 리액트가 자동으로 전달해 줬습니다.
하지만 클래스 컴포넌트에서는 다릅니다.

```jsx
import { Component } from "react";
```

를 상단에 선언한 후 class는 Component를 상속 받으면 됩니다.

```jsx
class User extends Component {...}
```

> 이제 User 클래스는 Component클래스를 상속 받고 리액트를 통해 정의한 클래스가 됩니다. 그리고 props.name을 this.props.name으로 바꿔줍니다. 이것은 우리가 컴포넌트를 상속받았기 때문에 가능한 것입니다. 이를 통해 this라는 예약어를 갖게 되고 props에 접근 가능하게 됩니다.
> 위 두 코드는 동일하게 작동합니다.

## 3️⃣ Class Component - State 관리

이제 useState훅이 포함되어 있는 컴포넌트를 클래스 컴포넌트로 변환하도록 하겠습니다.

**함수형 컴포넌트**

```jsx
import { useState } from "react";

import User from "./User";
import classes from "./Users.module.css";

const DUMMY_USERS = [
  { id: "u1", name: "Max" },
  { id: "u2", name: "Manuel" },
  { id: "u3", name: "Julie" },
];

const Users = () => {
  const [showUsers, setShowUsers] = useState(true);

  const toggleUsersHandler = () => {
    setShowUsers((curState) => !curState);
  };

  const usersList = (
    <ul>
      {DUMMY_USERS.map((user) => (
        <User key={user.id} name={user.name} />
      ))}
    </ul>
  );

  return (
    <div className={classes.users}>
      <button onClick={toggleUsersHandler}>
        {showUsers ? "Hide" : "Show"} Users
      </button>
      {showUsers && usersList}
    </div>
  );
};

export default Users;
```

위는 useState훅이 포함된 함수형 컴포넌트입니다.

**클래스 컴포넌트**

```jsx
import { Component } from "react";

import User from "./User";
import classes from "./Users.module.css";

const DUMMY_USERS = [
  { id: "u1", name: "Max" },
  { id: "u2", name: "Manuel" },
  { id: "u3", name: "Julie" },
];

class Users extends Component {
  constructor() {
    super();
    this.state = {
      showUsers: true,
    };
  }

  toggleUsersHandler() {
    this.setState((curState) => {
      return { showUsers: !curState.showUsers };
    });
  }

  render() {
    const usersList = (
      <ul>
        {DUMMY_USERS.map((user) => (
          <User key={user.id} name={user.name} />
        ))}
      </ul>
    );

    return (
      <div className={classes.users}>
        <button onClick={this.toggleUsersHandler.bind(this)}>
          {this.state.showUsers ? "Hide" : "Show"} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
}

export default Users;
```

클래스 컴포넌트에서는 `render()` 메소드 안에 함수를 추가하지 않습니다. 따라서 따로 분리하여 추가하면 됩니다. 클래스는 하나의 그룹이며 내부 기능을 그룹화할 수 있습니다.

```jsx
class Users extends Component {
    toggleUsersHandler() {...}
}
```

> 리액트 16.8 이전에는 상태를 변경하거나 선언하는 방법은 클래스 컴포넌트 뿐이었습니다. 따라서, 상태 관련하여 사용하고 싶다면 클래스 컴포넌트를 사용했어야 했습니다.
> 함수형 컴포넌트에서는 useState훅을 사용할 수 있지만
> 클래스 컴포넌트에서는 초기화, 상태 정의를 해줘야 합니다.
> 초기화는 useState훅에서 하는 것과 동일한 셈이지만 상태 정의는 위 함수 `toggleUsersHandler()` 처럼 필요할 때 업데이트하는 것을 말합니다.

클래스 컴포넌트에서는 상태를 정의할 때 `constructor()`를 통해 정의합니다.(생성자)

```jsx
constructor() {
    this.state = {}; // 객체 형식으로 선언 가능
}
```

> **클래스 컴포넌트에서는 상태는 항상 객체 형식이어야 합니다.**
> => 상태에 필요한 라이선스나 조각들을 하나의 상태 객체로 만들기 때문에 상태는 항상 객체여야 합니다.
> `this.state`는 **항상** 같은 것이며, state를 다른 이름으로 선언하면 안됩니다.
> 위 코드에서 보듯 무조건 하나의 객체 안에 넣어야 합니다.

```jsx
constructor() {
    this.state = {
        showUsers: true,
        moreState: 'Test' // 다른 상태를 넣고 싶다면 이런 식으로 여러 가지 선언이 가능하다.
    };
}
```

또한, 상태 변경 함수에서 아래와 같이 하시면 안됩니다.

```jsx
class Users extends Component {
    toggleUsersHandler() {
        this.state.showUsers: false; // NOT!!!!!!!
    }
}
```

위 대신 `setState()`를 사용해야 합니다.

```jsx
class Users extends Component {
  toggleUsersHandler() {
    // this.state.showUsers: false; // NOT!!!!!!!
    // this.setState({showUsers: false}) // OK!
    this.setState((curState) => {
      return { showUsers: !curState.showUsers };
    });
  }
}
```

이 `setState()`도 마찬가지로 항상 객체로 사용해야 합니다. 이제 이 객체는 설정하려는 새로운 상태를 포함하면 됩니다.

기존의 함수형 컴포넌트에서는 상태 갱신 함수를 호출하면 기존의 상태를 이 갱신 함수에 전달한 값으로 오버라이드합니다. 기존 상태를 오버라이드하면 리액트는 이를 자동적으로 병합 해주지 않습니다.

```jsx
const [showUsers, setShowUsers] = useState(true);
```

하지만 클래스 컴포넌트의 `setState()`는 자동으로 병합 작업을 해줍니다.

```jsx
this.setState((curState) => {
  return { showUsers: !curState.showUsers };
});
```

위 코드에서 `setState()`도 객체로 사용했듯이 내부도 객체로 사용해야 합니다. `curState`로 계속 갱신 함수가 될 수 있도록 해줍니다.

render()로 내려가보면

```jsx
render() {
    return (
      <div className={classes.users}>
        <button onClick={toggleUsersHandler}>
          {this.state.showUsers ? "Hide" : "Show"} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
```

`this.state`를 통해 상태 객체에 접근시켜 줍니다. 이를 통해 showUsers 프로퍼티에 접근 가능합니다.

```jsx
<button onClick={toggleUsersHandler}>
```

> 또한, 위 부분에서 함수를 바로 이렇게 사용할 수 없습니다. this를 통해 클래스의 메소드 부분을 가리켜야 합니다. 그럼에도 올바르게 작동하지는 않습니다.
> this가 위 toggleUsersHandler를 둘러싼 클래스를 참조하고 있는지를 확인해야 합니다.
> 그렇기 때문에 `bind(this)`를 사용합니다.
> 코드가 평가될 시점의 동일한 값이나 동일한 내용을 갖도록 설정합니다.
> 함수형 컴포넌트는 이벤트핸들러를 함수형으로 선언하기 때문에 이런 과정이 필요 없게 되는 것입니다.

```jsx
<button onClick={this.toggleUsersHandler.bind(this)}>
```

따라서, 이렇게 적용해야 작동하게 됩니다.

마지막으로 클래스에 생성자를 추가하고 **상위 클래스를 상속 받았기 때문에 생성자 안에** `super()`를 넣어줘야 합니다.

```jsx
constructor() {
    super()
    this.state = {
      showUsers: true,
    };
}
```

이렇게 하면 함수형 컴포넌트와 클래스 컴포넌트가 동일하게 작동할 것입니다.

---

언제나 클래스 컴포넌트는 사용해도 됩니다.
클래스 컴포넌트와 함수형 컴포넌트는 명백히 기호도의 차이가 있지만
함수형 컴포넌트가 코드 수도 줄일 수 있고 훅을 사용할 수 있다는 장점이 존재하기 때문에 무얼 사용하든 그것은
개발자의 몫이라고 생각합니다😁
