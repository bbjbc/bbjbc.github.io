---
title: "[React] 컴포넌트 생명 주기"
categories: [javascript, react]
tags: [javascript, react, component]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 세 번째 포스팅

안녕하세요! 세 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

두 번째 포스팅에 이어 **클래스 컴포넌트**에서만 사용 가능한 컴포넌트 생명 주기에 대해 알아보겠습니다!

Udemy의 React-The Complete Guide (incl Hooks, React Router, Redux) 강의를 바탕으로 작성된 글입니다.

## 💻 컴포넌트 생명 주기

클래스 컴포넌트의 부작용은 존재할까요?
**네 존재합니다!** <br/>
클래스 컴포넌트에서는 리액트 훅을 사용할 수 없습니다. 따라서, `useEffect`훅을 사용할 수 없게 됩니다. 하지만 컴포넌트 생명 주기라는 개념이 존재합니다.
생명 주기가 다른 클래스 컴포넌트를 서로 다른 시점에서 추가 가능합니다!

render() 메소드와 같은 내장 함수로 리액트에서 사용 가능합니다.

여러 가지의 생명 주기가 존재하지만 3가지에 대해 알아보겠습니다.

> **componentDidMount() 메소드** <br/>
> 이것은 컴포넌트를 생성하고 첫 번째 렌더링이 끝났을 때 호출되는 함수를 말합니다.

```jsx
useEffect(..., [])
```

`useEffect()`을 위와 같이 사용한 것과 같습니다.

> **componentDidUpdate() 메소드**<br/>
> 이것은 컴포넌트 업데이트 작업이 끝나고 재렌더링 후에 호출되는 함수를 말합니다.

```jsx
useEffect(..., [someDependency])
```

`useEffect()`을 위와 같이 사용한 것과 같습니다.

> **componentWillUnmount() 메소드**<br/>
> unmount란 컴포넌트가 DOM에서 제거되는 것을 말합니다. 이것은 해당 컴포넌트가 제거 되기 직전에 호출되는 함수를 말합니다.

```jsx
useEffect(()=>{return () => {...}}, [])
```

`useEffect()`을 위와 같이 사용한 것과 같습니다. cleanup함수를 사용한 것과 같게 됩니다.

사용 예시를 살펴보도록 하겠습니다.

**함수형 컴포넌트**

```jsx
const UserFinder = () => {
  const [filteredUsers, setFilteredUsers] = useState(DUMMY_USERS);
  const [searchTerm, setSearchTerm] = useState("");

  useEffect(() => {
    setFilteredUsers(
      DUMMY_USERS.filter((user) => user.name.includes(searchTerm))
    );
  }, [searchTerm]);

  const searchChangeHandler = (event) => {
    setSearchTerm(event.target.value);
  };

  return (
    <>
      <div className={classes.finder}>
        <input type="search" onChange={searchChangeHandler} />
      </div>
      <Users users={filteredUsers} />
    </>
  );
};

export default UserFinder;
```

**클래스 컴포넌트**

```jsx
class UserFinder extends Component {
  constructor() {
    super();
    this.state = {
      filteredUsers: [],
      searchTerm: "",
    };
  }

  searchChangeHandler(event) {
    this.setState({ searchTerm: event.target.value });
  }

  componentDidMount() {
    // Send http request ...
    this.setState({ filteredUsers: DUMMY_USERS });
  }

  componentDidUpdate(prevProps, prevState) {
    if (prevState.searchTerm !== this.state.searchTerm) {
      this.setState({
        filteredUsers: DUMMY_USERS.filter((user) =>
          user.name.includes(this.state.searchTerm)
        ),
      });
    }
  }

  render() {
    return (
      <>
        <div className={classes.finder}>
          <input type="search" onChange={this.searchChangeHandler.bind(this)} />
        </div>
        <Users users={this.state.filteredUsers} />
      </>
    );
  }
}
```

`useEffect()`를 사용한 부분 외에는 상태 변경 등은 <br/>
[`[React] 클래스 컴포넌트 구축 방법-3️⃣번`](https://bbjbc.github.io/javascript/react/second-posting/#3%EF%B8%8F%E2%83%A3-class-component---state-%EA%B4%80%EB%A6%AC)에서 했던 것처럼 바꾸면 됩니다.

### componentDidUpdate()

> 클래스 컴포넌트에서는 리액트 훅을 사용하지 못하는데 useEffect() 부분은 어떻게 해야 할까요?

```jsx
componentDidUpdate() {
    this.setState({
        filteredUsers: DUMMY_USERS.filter((user) =>
          user.name.includes(this.state.searchTerm)
        ),
      }); // 안에 있는 것은 훅에서 가져온 것임
}
```

를 선언 해줍니다. 이것은 상태 변화로 인해 컴포넌트가 재평가 되면 자동적으로 호출됩니다.
하지만, 위처럼 작동하게 되면 무한 루프가 형성 됩니다.
`UserFinder`라는 컴포넌트가 변경될 때마다 `componentDidUpdate()`가 실행되기 때문입니다.

```jsx
componentDidUpdate(prevProps, prevState) {
    if (prevState.searchTerm !== this.state.searchTerm) {
      this.setState({
        filteredUsers: DUMMY_USERS.filter((user) =>
          user.name.includes(this.state.searchTerm)
        ),
      });
    }
  }
```

따라서, 위처럼 이전 `props`와 `state`를 받도록 인자를 넣어준 후 조건문을 넣어주면 됩니다.
이것으로 `useEffect()`를 대체 되었습니다. 코드가 훨씬 길어진 것을 볼 수 있죠.

### componentDidMount()

> **componentDidMount**는 어떻게 이루어지죠?
> `DUMMY_USERS`를 데이터베이스에서 가져온다고 가정을 해봅시다.

```jsx
constructor() {
    super();
    this.state = {
      filteredUsers: [],
      searchTerm: "",
    };
  }
```

처음이니까 `filteredUsers`는 비어있는 배열일 것입니다.

```jsx
componentDidMount() {
    // Send http request ...
    this.setState({ filteredUsers: DUMMY_USERS });
  }
```

상태 설정을 데이터베이스에서 가져온 `DUMMY_USERS`로 설정하는 것입니다. 이런 식으로 `componentDidMount`는 컴포넌트가 초기 렌더링될 때만 실행됩니다. 이 후 갱신이 이루어져도 `componentDidMount`는 작동하지 않고 `componentDidUpdate`가 작동하게 될 것입니다.

### componentWillUnmount()

> **componentWillUnmount**는 어떻게 이루어지죠?

```jsx
class User extends Component {
  componentWillUnmount() {
    console.log("User will unmount!");
  }

  render() {
    return <li className={classes.user}>{this.props.name}</li>;
  }
}
```

`User`컴포넌트에 예를 들어보겠습니다.<br/>
이 토이 프로젝트는
![블로그1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/55e854b7-f5b7-435d-afe1-9e07afbcbe71)
위 토글 버튼을 누르면
![블로그2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/a40b00af-1f3f-42b0-9da0-09ecf03551e8)
이렇게 아래 이름이 사라집니다. 이것은 DOM에서 제거되기 직전에 콘솔창에 띄울 것입니다.<br/>
![블로그3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/08302f5f-f64f-44a9-9d63-845da53f0539)
<br/>바로 이렇게 3번이나 로깅 되었습니다. <br/>
그 이유는 우리가 `User`컴포넌트를 3번 사용했기 때문입니다. 각 인스턴스가 삭제되었기 때문에 그 전에 띄워진 것입니다.

---

지금까지 클래스 컴포넌트에서만 사용 가능한 사이드 이펙트 효과를 주는 **컴포넌트 생명 주기**였습니다.
