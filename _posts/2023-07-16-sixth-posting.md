---
title: "[React Hooks] 리액트 커스텀 Hooks"
categories: [javascript, react]
tags: [hook, http, react, javascript]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 여섯 번째 포스팅

안녕하세요! 여섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

저번보다는 비가 덜 내리는 것 같아요❗️ 지금은 거의 안오고 날씨가 우중충한 상태죠..👎
하지만 날씨에 연연하지 않도록 열심히 공부를 해보아요~

지금까지는 다양한 리액트 내장 훅을 사용해서 코드 작성을 해왔습니다.

이제는 내장 훅도 사용하지만 우리가 직접 만드는 커스텀 훅을 만드는 방법에 대해 알아 보러 갑시다 ~ 💛

Udemy의 React-The Complete Guide (incl Hooks, React Router, Redux) 강의를 바탕으로 작성된 글입니다.

## 1️⃣ 커스텀 훅 ?

### ⭕️What. 커스텀 훅이 뭘까요 ? <br/>

우리가 항상 사용해 왔던 `useState`, `useContext`, `useEffect`, `useReducer` 등 리액트 내장 훅과 같은 정규 함수에 속합니다. <br/>

**하지만 그 안의 로직을 우리가 마음대로 바꾸고 설정 가능한 훅을 말합니다❗️**

커스텀 훅을 만들어서 재사용 가능한 함수에 상태를 설정하는 로직을 아웃소싱 가능합니다.
이렇게 만들어진 훅을 사용하고 싶은 컴포넌트에서 호출이 가능합니다. <br/>
**즉, 로직 재사용이 가능한 메커니즘입니다.**

### ⭕️Why. 커스텀 훅을 왜 사용할까요 ? <br/>

보통 컴포넌트마다 비슷한 로직을 만드는 경우가 생깁니다.
프로그래밍 중 코드가 중복될 때는 코드를 떼어내 하나의 함수로 만들어 내어 리팩토링 하곤 합니다. <br/>
우리는 이를 커스텀 훅으로 만들 것입니다.

비슷한 로직을 보면 상태 관리를 하는 `useState`훅이나 다른 리액트 내장 훅을 사용하게 됩니다. <br/>

또한, 리액트 내장 훅은 다른 훅과 함께 사용하지 못하지만 **커스텀 훅은 리액트 내장 훅과 공존 가능합니다 !**<br/>

하나의 커스텀 훅 컴포넌트에서 사용하여 아웃소싱하여 훅 사용 빈도를 줄이고 코드 수도 줄여 전체적인 코드를 간소화 가능하다는 장점이 있습니다.

### ⭕️How. 커스텀 훅의 규칙이 뭔가요 ?

사용자가 설정하고 싶은 파일명으로 js 파일을 만든 후

> **함수명은 반드시 use로 시작해야 합니다.**

이것은 필수입니다. 리액트 내장 훅도 use로 시작하는 것과 마찬가지로 커스텀 훅도 use로 시작해야 합니다.

> 커스텀 훅을 리액트 내장 훅과 같은 방식으로 사용하는 것입니다. 이것은 리액트가 요구하는 것으로 만약 그렇지 않고 리액트 훅을 커스텀 훅에서 사용하면서, 커스텀 훅을 잘못된 곳에서 사용하게 된다면 리액트 내장 훅 역시 잘못된 곳에서 쓸 수 있음을 내포하는 것입니다.

use다음으로 함수 이름은 사용자 마음대로 설정하셔도 무관합니다 ㅎㅎ

```jsx
const usePosting = () => {};
export default usePosting;
```

이 형태로 사용하고 안에 로직은 사용자 마음대로 설정하고 다른 컴포넌트에 적용 가능합니다.

## 2️⃣ 커스텀 훅을 구성하고 적용 해보자

실제로 커스텀 훅을 짜보도록 하겠습니다. <br/>
그에 앞서 이 내용은 firebase를 사용한 리액트 앱을 백엔드와 연결하는 <br/>
저의 [`리액트 HTTP 요청`](https://bbjbc.github.io/http/javascript/react/fifth-posting/) 게시물을 먼저 보고 오시는 걸 추천 드립니다❗️

또한, `firebase`에서 url을 가져오기 위해서는 개인의 firebase 계정을 생성하여 개인 url을 가져오시면 됩니다.

`App.js`

```jsx
const App = () => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  const [tasks, setTasks] = useState([]);

  const fetchTasks = async () => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(
        "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json"
      );

      if (!response.ok) {
        throw new Error("Request failed!");
      }

      const data = await response.json();

      const loadedTasks = [];

      for (const taskKey in data) {
        loadedTasks.push({ id: taskKey, text: data[taskKey].text });
      }

      setTasks(loadedTasks);
    } catch (err) {
      setError(err.message || "Something went wrong!");
    }
    setIsLoading(false);
  };

  useEffect(() => {
    fetchTasks();
  }, []);

  const taskAddHandler = (task) => {
    setTasks((prevTasks) => prevTasks.concat(task));
  };
};
```

위 코드는 GET 요청을 하고, 오류를 처리하고 로딩 상태를 처리하거나 도착한 데이터를 변경하고 데이터를 표시하는 모든 작업이 fetch API를 통해 이루어져 있습니다.

`NewTask.js`

```jsx
const NewTask = (props) => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const enterTaskHandler = async (taskText) => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(
        "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
        {
          method: "POST",
          body: JSON.stringify({ text: taskText }),
          headers: {
            "Content-Type": "application/json",
          },
        }
      );

      if (!response.ok) {
        throw new Error("Request failed!");
      }

      const data = await response.json();

      const generatedId = data.name;
      const createdTask = { id: generatedId, text: taskText };

      props.onAddTask(createdTask);
    } catch (err) {
      setError(err.message || "Something went wrong!");
    }
    setIsLoading(false);
  };
};
```

위 코드는 `firebase`에 `POST` 요청이 전송되는 코드입니다.

간단하게 말해서 `App.js`는 데이터를 fetch해오는 작업을,
`NewTask.js`는 입력된 데이터를 POST 요청 전송하는 작업을 하는 파일이라고 생각하시면 됩니다.

**다른 여러가지의 컴포넌트들이 존재하지만 현재 게시물에서는 커스텀 훅 사용을 위한 컴포넌트만을 설명 드리도록 하겠습니다.**

위 두 코드를 보면 비슷한 점이 존재합니다.

- `App.js`의 데이터를 fetch 해오는 부분과 `NewTask.js`에서 데이터를 전송 요청 하는 부분
- 컴포넌트의 로딩과 에러를 관리하는 상태 관리 훅

위처럼 비슷한 점과 중복되는 코드가 존재합니다.<br/>
이 중복되는 부분을 아웃소싱하여 커스텀 훅으로 만들 예정입니다.

### 🔘커스텀 훅에 어떤 로직이 포함 되어야 할까요?

1. HTTP 로직을 아웃소싱
2. 로딩 및 오류 상태에 대한 설정 로직

이 두 가지의 로직이 포함되면 중복되는 부분이 줄여지겠죠?
그러면 두 컴포넌트에서 재사용 가능하기 때문에 매우 편리하게 됩니다.

## 3️⃣ 커스텀 훅 빌드

자❗️ 커스텀 훅 만들어 봅시다.
`use-http.js`

```jsx
const useHttp = () => {};
export default useHttp;
```

Http에 관한 커스텀 훅을 만들 것이니 useHttp로 정합니다.
**이름은 use로 시작하고 그 뒤 이름 설정은 자유입니다.**

로딩과 에러를 관리하는 상태, 응답 보내고 받는 로직을 아웃소싱 해야합니다.

```jsx
const useHttp = () => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const sendRequest = async () => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(
        "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json"
      );

      if (!response.ok) {
        throw new Error("Request failed!");
      }

      const data = await response.json();

      const loadedTasks = [];

      for (const taskKey in data) {
        loadedTasks.push({ id: taskKey, text: data[taskKey].text });
      }

      setTasks(loadedTasks);
    } catch (err) {
      setError(err.message || "Something went wrong!");
    }
    setIsLoading(false);
  };
};
export default useHttp;
```

이렇게 위처럼 아웃소싱 해야하는 부분을 `App.js`에서 가져왔습니다. <br/>
`GET`과 `POST` 에서 둘 다 사용할 훅이기 때문에 공통적인 작업명으로 `fetchTasks`를 `sendRequest`로 바꾸었습니다.

**이 커스텀 훅은 재사용이 가능해야 하기 때문에 함수 이름을 일반화 해서 작성하는 것이 좋습니다.**

> 이 훅은 어떤 종류의 요청이든 받아서 모든 종류의 URL로 보낼 수 있어야 하며 어떤 데이터 변환도 할 수 있어야 합니다. 또한, 로딩과 오류라는 상태를 관리하고 모든 과정을 동일 순서대로 실행 가능해야 합니다.

이런 유연한 훅을 만들기 위해서는 매개변수가 필요합니다.

### 매개변수

- firebase URL과 메소드, 본문, 헤더 등 유연성을 갖춘 변수 → `requestConfig` <br/>
  URL 입력 후 `POST` 요청 방법도 생각해야 하니 메소드, 본문, 헤더도 추가해줍니다.

```jsx
const response = await fetch(requestConfig.url, {
  method: requestConfig.method ? requestConfig.method : "GET",
  headers: requestConfig.headers ? requestConfig.headers : {},
  body: requestConfig.body ? JSON.stringify(requestConfig.body) : null,
});
```

`GET`방식과 `POST`방식을 고려하여 매개변수를 활용하여 위와 같이 일반화하여 작성할 수 있습니다.

```jsx
if (!response.ok) {
  throw new Error("Request failed!");
}

const data = await response.json();
```

오류처리방식과 응답을 json으로 변경해주는 곳까지는 동일합니다.

```jsx
const loadedTasks = [];

for (const taskKey in data) {
  loadedTasks.push({ id: taskKey, text: data[taskKey].text });
}

setTasks(loadedTasks);
```

하지만, 위와 같은 부분은 데이터를 가져와 처리하는 부분이므로 공통적인 부분이 아닙니다. <br/>
**따라서, 이 부분은 훅에 포함 되어서는 안됩니다.**

- 이러한 데이터를 그 함수에 넘기는 데이터 매개변수 → `applyData`

```jsx
applyData(data);
```

따라서, 위 코드를 이렇게 변경 해줄 수 있습니다.

이렇게 해서 커스텀 훅의 필요한 로직은 들어갔습니다.<br/>
이제 이 커스텀 훅을 사용하기 위한 반환값을 작성해야 합니다.

**커스텀 훅은 무엇이든 반환 가능합니다.(숫자, 문자열, 배열, 객체, ...)**

```jsx
return {
  isLoading,
  error,
  sendRequest,
};
```

상태 값과 함수를 객체 형식으로 반환 하였습니다.<br/>
이렇게 해서 커스텀 훅을 작성해 보았습니다❕

## 4️⃣ fetch에 적용

이제 커스텀 훅을 `App.js`에 적용해 봅시다. <br/>
이 훅은 리액트 내장 훅이 아니기 때문에 따로 import 해주셔야 합니다.

```jsx
useHttp({
  url: "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
});
```

커스텀 훅에서 URL 속성에 접근하기 때문에 위와 같이 작성 가능합니다.
fetch는 default method가 `GET`이므로 뒤에 추가적인 인자를 작성할 필요는 없습니다.

하지만 이것이 끝이 아닙니다. 왜냐하면 커스텀 훅은 2가지 매개변수를 가지고 있기 때문에 데이터 입력 함수를 넣어줘야 합니다.

이 함수의 이름은 `transformTasks`라고 하겠습니다.

```jsx
const transformTasks = (tasksObj) => {
  const loadedTasks = [];

  for (const taskKey in tasksObj) {
    loadedTasks.push({ id: taskKey, text: tasksObj[taskKey].text });
  }

  setTasks(loadedTasks);
};
```

원래 `App.js`에 있던 데이터 부분을 매개변수에 맞게 변경해줍니다.

```jsx
useHttp(
  {
    url: "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
  },
  transformTasks
);
```

이제 데이터 함수를 포인팅 가능합니다.

커스텀 훅에서 반환했던 것에서 요청을 활성화하기 위해서는 호출해야 합니다.

```jsx
const { isLoading, error, sendRequest } = useHttp(
  {
    url: "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
  },
  transformTasks
);
```

반환값의 구조분해할당을 통해 가져옵니다. <br/>
하지만 `sendRequest`라는 함수는 `App.js`에 존재하지 않습니다. 여기에 별칭을 통해 `fetchTasks`를 사용할 수 있습니다.

```jsx
sendRequest: fetchTasks;
```

이 컴포넌트에서의 함수를 가리키고 있는 포인터입니다.

그리고나서 커스텀 훅에 사용했던 로직인 `fetchTasks`함수를 모두 삭제할 수 있습니다.

```jsx
useEffect(() => {
  fetchTasks();
}, []);
```

아직 이 부분을 해결하지 못했습니다. <br/>
기존에 의존성으로 `fetchTasks`를 추가해야 했지만 기존 `fetchTasks`함수는 상태 갱신 함수만 호출했으니 리액트에서는 상태 갱신 함수에 대해서는 불변성을 보장하니 작성할 필요가 없었던거죠.

하지만 `sendRequest`함수는 알지 못했고 `fetchTasks`가 변할 때마다 재실행 하기 위해서는 의존성을 이제 추가해 줘야 합니다.

**하지만, 지금 상태에서 추가한다면 무한루프가 형성됩니다.** <br/>
그러므로 커스텀 훅을 조정한 후 다시 추가 해야 합니다.

> 무한루프가 형성되는 것은 함수가 객체이기 때문입니다. 내부에 같은 로직을 가지더라도 함수가 재생성되면 이는 메모리에서 새로운 객체로 인식되어 `useEffect`가 이를 새로운 값으로 받아들이며 같은 함수일지라도 재실행을 유발합니다.

따라서 이를 피하기 위해서는 `sendRequest`를 `useCallback`으로 감싸주면 됩니다.

### 🔥`useCallback`훅 ?

> `useCallback`이 뭐에요?

이것은 리액트 내장 훅입니다. 리액트에서 함수를 기억하고 최적화하는 데에 사용합니다.

1. 성능 최적화
   - 함수를 기억하여 동일 함수 인스턴스를 유지하고, 의도치 않은 재렌더링을 방지하고 성능을 향상시킬 수 있습니다.
2. 의존성 배열 관리
   - `useCallback`은 두번째 인자로 의존성 배열을 함께 받습니다. 함수가 사용하는 외부 변수나 상태가 변경될 때만 함수를 새로 생성할 수 있도록 제어 가능합니다. 이를 통해 불필요한 재렌더링을 방지할 수 있습니다.
3. 불필요한 재생성 방지
   - 함수가 변경되지 않은 경우 이전에 기억된 함수를 반환합니다.

```jsx
const useHttp = (requestConfig, applyData) => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const sendRequest = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(requestConfig.url, {
        method: requestConfig.method ? requestConfig.method : "GET",
        headers: requestConfig.headers ? requestConfig.headers : {},
        body: requestConfig.body ? JSON.stringify(requestConfig.body) : null,
      });

      if (!response.ok) {
        throw new Error("Request failed!");
      }

      const data = await response.json();
      applyData(data);
    } catch (err) {
      setError(err.message || "Something went wrong!");
    }
    setIsLoading(false);
  }, []);
};
```

이 경우에는 `sendRequest`함수 내부에서 외부 변수인 `requestConfig`와 `applyData`를 사용하고 있어서 의존성에 추가해주면 됩니다.

하지만, 이렇게 해도 문제가 발생합니다. <br/>
두 매개변수 모두 객체입니다.

`App.js`에서 `url`과 `transformTasks` 객체들이 컴포넌트가 재실행될 때마다 재생성되지 않도록 해야합니다. <br/>

이를 해결하기 위해서는 매개변수에 있던 두 객체를 `sendRequest`의 매개변수에 두면 됩니다. 그러면 함수 내부에서 외부 변수 사용하는 것이 없어지기 때문에 의존성 배열에 추가할 것이 없어집니다. <br/>
그러면 함수를 실행 시켜도 `App.js`의 객체들이 컴포넌트가 재실행될 때마다 재생성되지 않게 됩니다❗️

```jsx
const sendRequest = useCallback(async (requestConfig, applyData) => {}, []);
```

이런식으로 매개변수의 위치를 옮겨주면 됩니다.

이제 커스텀 훅에 매개변수가 없으니 `App.js`에서 훅사용에 대해 수정을 해야합니다.

```jsx
const { isLoading, error, sendRequest: fetchTasks } = useHttp();
```

훅 사용에 대한 인자를 빼고 `fetchTasks`에 객체를 추가하면 됩니다.

```jsx
fetchTasks(
  {
    url: "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
  },
  transformTasks
);
```

이런식으로 함수안에 그대로 넣으시면 됩니다.
`transformTask`함수가 `useEffect`밖에 있으므로 훅 안으로 넣어주면 모든 것을 사용할 수 있게 됩니다.

```jsx
const { isLoading, error, sendRequest: fetchTasks } = useHttp();

useEffect(() => {
  const transformTasks = (tasksObj) => {
    const loadedTasks = [];

    for (const taskKey in tasksObj) {
      loadedTasks.push({ id: taskKey, text: tasksObj[taskKey].text });
    }

    setTasks(loadedTasks);
  };

  fetchTasks(
    {
      url: "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
    },
    transformTasks
  );
}, [fetchTasks]);
```

바로 이렇게요❗️이렇게 하면 더 이상 무한루프가 만들어지는 코드가 아니게 됩니다.

이렇게 해서 데이터 fetch 과정을 커스텀 훅으로 만들었습니다.

## 5️⃣ POST 요청에 적용

완성된 커스텀 훅을 통해 `POST` 요청하는 컴포넌트에도 적용합시다. <br/>

```jsx
const { isLoading, error, sendRequest: sendTaskRequest } = useHttp();
```

커스텀 훅을 다음과 같이 사용합니다. 매개변수는 리팩토링으로 삭제했으니 빈 채로 두면 됩니다.

`sendTaskRequest`함수를 만들어봅시다. 방식은 `POST`이기 때문에 `method`, `headers`, `body`를 모두 사용해줍니다.

```jsx
sendTaskRequest({
  url: "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
  method: "POST",
  headers: {
    "Content-Type": "application.json",
  },
  body: { text: taskText },
});
```

그리고 두 번째 인자로는 응답 데이터를 받아서 무엇인가 행하는 함수입니다. → `createTask`

```jsx
const createTask = (taskData) => {
  const generatedId = taskData.name;
  const createdTask = { id: generatedId, text: taskText };
  props.onAddTask(createdTask);
};
```

하지만 이렇게 하면 `taskText`에 대해서 에러가 발생합니다. `taskText`는 다른 함수에 속해 있는 매개변수기 때문입니다. <br/>
다른 함수에 속해 있는 `createTask`에 `taskText`를 사용하게 하려면 `createTask`의 매개변수에 추가하면 됩니다.

이렇게 하면 `createTask`는 매개변수가 2개가 됩니다.
하지만 `useHttp`훅에서는 데이터를 담당하는 함수인 `applyData`는 한 개의 인자를 가져야만 합니다.

> **이를 어떻게 해결하면 좋을까요?**<br/>

자바스크립트에 내장되어 있는 `bind`메소드를 사용하면 됩니다. <br/>

```jsx
createTask.bind();
```

위와 같이 구성해줍니다. <br/>
`bind`메소드는 함수를 사전에 구성할 수 있게 해줍니다. 호출 즉시 함수가 실행되지 않습니다.

> `bind`의 첫 번째 인자는 실행이 예정된 함수에서 `this` 예약어를 사용하게 하는 것인데 이 곳에선 쓸 일이 없습니다. 따라서 `null`로 둡니다.<br/>
> 두 번째 인자는 호출 예정인 함수가 받는 첫 번째 인자가 됩니다. 바로 `taskText`입니다.

```jsx
const createTask = (taskText, taskData) => {
  const generatedId = taskData.name;
  const createdTask = { id: generatedId, text: taskText };

  props.onAddTask(createdTask);
};

const enterTaskHandler = async (taskText) => {
  sendTaskRequest(
    {
      url: "https://react-http-14653-default-rtdb.firebaseio.com/tasks.json",
      method: "POST",
      headers: {
        "Content-Type": "application.json",
      },
      body: { text: taskText },
    },
    createTask.bind(null, taskText)
  );
};
```

이렇게 하면 모든 데이터를 받을 수 있게 됩니다.

---

지금까지 HTTP 전송과 관련한 커스텀 훅을 만들어 보았습니다. <br/>
저번에 리액트 HTTP 전송에 대한 내용을 복습하는 차원의 포스팅이 되었으며 정말 헷갈리고 복잡하였지만 다시 한 번 차근차근 읽어보면서 다지는 시간이 되었습니다😏<br/>

긴 글 읽어주셔서 정말 감사합니다.<br/>
더욱 더 좋은 포스팅이 되도록 노력하겠습니다✨
