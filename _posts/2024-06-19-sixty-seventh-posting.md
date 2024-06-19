---
title: "[React] 도대체 Tanstack Query가 뭔데 이렇게 많이 써?"
categories: [tanstack-query, react]
tags: [react, tanstack, tanstack query, react query]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 일곱 번째 포스팅

안녕하세요! 예순 일곱 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **React - Tanstack Query**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ Tanstack Query가 뭔데?

Tanstack Query (이전 이름은 React Query)는 웹 애플리케이션에서 데이터 가져오기, 캐싱, 동기화 및 서버 상태를 업데이트를 간편하게 만들어주는 리액트의 라이브러리이다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/6dca6a4b-9b86-4793-a034-4b428d26fffe)

웹 애플리케이션의 데이터 가져오기를 완벽하게 해주는 라이브러리로 자주 언급된다고 한다.

즉, React에서 API 요청과 상태 관리를 쉽게 해주는 도구인거야!

## ⚾️ Motivation

대부분의 핵심 웹 프레임워크는 `Data fetching`이나 업데이트를 전체적으로 다루는 방법을 제공하지 않는다. 이로 인해 개발자들은 엄격한 의견을 반영한 메타 프레임워크를 만들거나, 데이터를 가져오는 자신만의 방법을 개발하게 된다. 이는 보통 컴포넌트 기반의 상태와 부작용을 결합하거나, 비동기 데이터를 앱 전반에 걸쳐 저장하고 제공하기 위해 보다 일반적인 상태 관리 라이브러리를 사용하는 것을 말한다.

대부분의 전통적인 상태 관리 라이브러리는 클라이언트 상태를 다루는 데는 훌륭하지만, 비동기나 서버 상태를 다루는 데는 그리.. 뛰어나지 않다. 서버 상태는 완전히 다르기 때문이다!

예를 들어 서버 상태는

> - 원격에 저장되며, 이를 제어하거나 소유할 수 없는 경우가 많다.
> - 데이터를 가져오거나 업데이트하기 위해 비동기 API를 필요로 한다.
> - 공유 소유권을 가지며, 다른 사람이 모르게 변경할 수 있다.
> - 주의를 기울이지 않으면 응용 프로그램에서 잠재적으로 "out of date" 것이 될 수 있다.

기능은 다음과 같다.

> - 캐싱
> - 동일한 데이터에 대한 중복 요청을 단일 요청으로 통합
> - 백그라운드에서 오래된 데이터 업데이트
> - 데이터가 얼마나 오래되었는지 알 수 있음.
> - 데이터 업데이트를 가능한 빠르게 반영
> - 페이지네이션 및 데이터 지연 로드와 같은 성능 최적화
> - 서버 상태의 메모리 및 가비지 수집 관리
> - 구조 공유를 사용하여 쿼리 결과를 메모화

React Query는 서버 상태를 관리하는 데 있어 손에 꼽힐 만큼 훌륭한 라이브러리이다. 설정 없이도 놀라울 정도로 잘 작동하며, 애플리케이션이 성장함에 따라 원하는 대로 맞춤화할 수 있다.

<br>

## ⚾️ Technical Note

React Query는 다음과 같은 기술적 장점이 있다.

- 복잡하고 이해하기 어려운 코드를 많이 줄일 수 있으며, 이를 소수의 React Query 로직으로 대체할 수 있다.
- 새로운 서버 상태 데이터 소스를 연결하는 걱정 없이 새로운 기능을 쉽게 구축할 수 있게 해주어 애플리케이션을 더 유지 관리하기 쉽게 만든다.
- 애플리케이션을 더 빠르고 반응성이 뛰어나게 만들어 최종 사용자에게 직접적인 영향을 미칠 수 있다.
- 대역폭을 절약하고 메모리 성능을 향상시킬 수 있다.

```ts
import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from "@tanstack/react-query";

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  );
}

function Example() {
  const { isPending, error, data } = useQuery({
    queryKey: ["repoData"],
    queryFn: () =>
      fetch("https://api.github.com/repos/TanStack/query").then((res) =>
        res.json()
      ),
  });

  if (isPending) return "Loading...";

  if (error) return "An error has occurred: " + error.message;

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{" "}
      <strong>✨ {data.stargazers_count}</strong>{" "}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  );
}
```

위와 같은 예시처럼 React Query를 사용하여 github 프로젝트의 통계를 가져오는 기본적이고 간단한 형태를 보일 수 있다.

<br>

## 💿 Installation

```bash
npm i @tanstack/react-query
# or
pnpm add @tanstack/react-query
# or
yarn add @tanstack/react-query
# or
bun add @tanstack/react-query
```

위와 같이 설치를 할 수 있다.

개발 중에 버그와 불일치를 잡는 데 도움이 되는 `ESLint Plugin Query`를 사용하는 것도 권장된다고 한다.

```bash
npm i -D @tanstack/eslint-plugin-query
# or
pnpm add -D @tanstack/eslint-plugin-query
# or
yarn add -D @tanstack/eslint-plugin-query
# or
bun add -D @tanstack/eslint-plugin-query
```

### 📀 사용 방법(1)

플러그인에 대한 `권장하는 모든 rule`을 적용하려면 아래와 같이 `.eslintrc.js` 파일의 `extends` 배열 안에 `plugin:@tanstack/eslint-plugin-query/recommended`을 추가한다.

```js
module.exports = {
  // ...
  extends: ["plugin:@tanstack/eslint-plugin-query/recommended"],
  rules: {
    /* 수정하고자 하는 rule 추가 가능... */
  },
};
```

`rule`을 변경하고 싶다면 `rules`에 아래와 같이 추가해주면 된다.

### 📀 사용 방법(2)

원하는 `rule`을 개별적으로 설정해서 적용하려면 아래처럼 `.eslintrc.js` 파일의 `plugins` 배열 안에 `@tanstack/query`를 추가하고, 적용하고자 하는 `rules`에 규칙을 추가한다.

```js
module.exports = {
  // ...
  plugins: ["@tanstack/query"],
  rules: {
    "@tanstack/query/exhaustive-deps": "error",
    "@tanstack/query/no-rest-destructuring": "warn",
    "@tanstack/query/stable-query-client": "error",
  },
};
```

---

# 2️⃣ Query는 뭔데?

## 🔔 Basic Query Concept

쿼리는 고유한 키에 연결된 비동기 데이터 소스에 대한 선언적 의존성을 말한다. 쿼리는 서버에서 데이터를 가져오기 위해 `Promise` 기반 메서드(`GET` 및 `POST`메서드 포함)를 사용할 수 있다. 메서드가 서버의 데이터를 수정하는 경우에는 `Mutations`를 사용할 수 있다.

컴포넌트나 커스텀 훅에서 쿼리를 구독하기 위해서는 최소한 아래와 같은 사항을 포함하여 `useQuery` 훅을 호출한다.

- 쿼리에 대한 고유한 키 → `queryKey`
- 데이터를 해결하거나 오류를 발생시키는 `promise를 반환하는 함수` → `queryFn`

```tsx
import { useQuery } from "@tanstack/react-query";

function App() {
  const info = useQuery({ queryKey: ["todos"], queryFn: fetchTodoList });
}
```

제공한 고유 키는 내부적으로 애플리케이션 전반에 걸쳐 쿼리를 다시 가져오고, 캐싱하고, 공유하는 데 사용된다.

`useQuery`에서 반환된 쿼리 결과는 템플릿 작성 및 데이터의 기타 사용에 필요한 모든 정보를 포함한다.

```tsx
const result = useQuery({ queryKey: ["todos"], queryFn: fetchTodoList });
```

`result` 객체는 생산성을 높이기 위해 알아야 할 몇 가지 중요한 상태를 포함한다. 쿼리는 주어진 순간에 아래와 같은 상태 중 하나만 가질 수 있게 된다.

- `isPending` 또는 `status==='pending'` → 쿼리에 아직 데이터가 없음
- `isError` 또는 `status==='error'` → 쿼리에서 오류가 발생함
- `isSuccess` 또는 `status==='success'` → 쿼리가 성공적이며 데이터가 있음

위와 같은 주요 상태 외에도 쿼리 상태에 따라 더 많은 정보를 얻을 수 있다.

- `error` → 쿼리가 `isError` 상태인 경우, error 속성을 통해 오류 확인이 가능하다.
- `data` → 쿼리가 `isSuccess` 상태인 경우, data 속성을 통해 데이터를 확인 가능하다.
- `isFetching` → 쿼리가 어느 상태에 있든지 백그라운드에서 데이터를 다시 가져오는 중일 때 `isFetching`은 `true`가 된다.

대부분의 쿼리에서는 일반적으로 `isPending` 상태를 확인한 다음, `isError` 상태를 확인하고, 마지막으로 데이터를 사용할 수 있다고 가정하고 성공 상태를 렌더링한다.

```tsx
function Todos() {
  const { isPending, isError, data, error } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodoList,
  });

  if (isPending) {
    return <span>Loading...</span>;
  }

  if (isError) {
    return <span>Error: {error.message}</span>;
  }

  // 이 시점에서 `isSuccess === true`라고 가정 가능
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

Boolean이 익숙하지 않다면, `status` 상태를 사용할 수도 있다.

```tsx
function Todos() {
  const { status, data, error } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodoList,
  });

  if (status === "pending") {
    return <span>Loading...</span>;
  }

  if (status === "error") {
    return <span>Error: {error.message}</span>;
  }

  // 또한 status === 'success'이다. 하지만 "else" 논리도 작동함.
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

<br>

## 🔔 FetchStatus

`status` 필드 외에도 아래와 같은 옵션이 있는 추가 `fetchStatus` 속성도 사용 가능하다.

- `fetchStatus === 'fetching'` → 쿼리가 현재 데이터를 가져오는 중
- `fetchStatus === 'paused'` → 쿼리가 데이터를 가져오려고 했지만 일시 중지됨.
- `fetchStatus === 'idle'` → 쿼리가 현재 아무 작업도 하지 않고 있음.

<br>

## 🔔 왜 두 개의 다른 상태가 필요해?

백그라운드 재조회와 `stale-while-revalidate` 로직은 모든 `status`와 `fetchStatus` 조합을 가능하게 한다.

- `success` 상태의 쿼리는 보통 `idle`, `fetchStatus`에 있지만, 백그라운드 재조회가 발생하면 `fetching` 상태일 수도 있다.
- 데이터가 없는 상태로 마운트된 쿼리는 보통 `pending` 상태와 `fetching`, `fetchStatus`에 있지만, 네트워크 연결이 없으면 `paused` 상태일 수도 있다.

따라서 쿼리가 데이터를 실제로 가져오지 않고도 `pending` 상태일 수 있다는 점을 유의해야 한다.

- `status`는 데이터에 대한 정보를 제공하며 '데이터를 가지고 있는가! 아닌가!'를 나타냄.
- `fetchStatus`는 `queryFn`에 대한 정보를 제공하며 '실행 중인가! 아닌가!'를 나타냄.

---

# 3️⃣ Query Key는 뭔데?

Tanstack Query는 쿼리 키를 기반으로 쿼리 캐싱을 관리한다고 한다. 쿼리 키는 최상위 레벨에서 배열 형태여야 하며, 단일 문자열을 포함하는 간단한 배열부터 여러 문자열과 중첩된 객체를 포함하는 복잡한 배열까지 다양할 수 있다. 쿼리 키가 직렬화 가능하고 쿼리 데이터에 고유하다면 사용 가능하다!

## 〰️ 단순한 쿼리 키

가장 간단한 형태의 키는 상수 값이 있는 배열이다. 이러한 형식은 `일반적인 목록/인덱스 리소스`, `비계층적 리소스`와 같은 경우에 유용하다.

```tsx
// 할 일 목록
useQuery({ queryKey: ['todos'], ... })

// 다른 어떤 것
useQuery({ queryKey: ['something', 'special'], ... })
```

<br>

## 〰️ 변수와 함께 사용하는 배열 키

쿼리가 데이터를 고유하게 설명하기 위해 더 많은 정보가 필요할 때, 문자열과 직렬화 가능한 객체를 포함한 배열을 사용할 수 있다.

- 계층적 또는 중첩 리소스
- 항목을 고유하게 식별하기 위한 ID, 인덱스 또는 다른 원시 값을 전달하는 경우
- 추가 매개변수가 있는 쿼리
- 추가 옵션 객체를 전달하는 경우

위와 같은 경우에 유용하게 사용 가능하다.

```tsx
// 개별 할 일
useQuery({ queryKey: ['todo', 5], ... })

// "preview" 형식의 개별 할 일
useQuery({ queryKey: ['todo', 5, { preview: true }], ...})

// "done" 유형의 할 일 목록
useQuery({ queryKey: ['todos', { type: 'done' }], ... })
```

<br>

## 〰️ 쿼리 키는 결정적으로 해시된다

이는 객체 내 키의 순서에 상관없이 모든 쿼리가 동일하게 간주된다는 것이다.

```tsx
useQuery({ queryKey: ['todos', { status, page }], ... })
useQuery({ queryKey: ['todos', { page, status }], ...})
useQuery({ queryKey: ['todos', { page, status, other: undefined }], ... })
```

그러나 아래와 같은 쿼리 키는 동일하지 않음. 배열 항목의 순서가 중요해지는 예시이다.

```tsx
useQuery({ queryKey: ['todos', status, page], ... })
useQuery({ queryKey: ['todos', page, status], ...})
useQuery({ queryKey: ['todos', undefined, page, status], ...})
```

<br>

## 〰️ 쿼리 함수가 변수에 의존하는 경우, 해당 변수를 쿼리 키에 포함

쿼리 키는 가져오는 데이터를 고유하게 설명하기 때문에, 쿼리 함수에서 사용하는 변수를 포함해야 한다.

```tsx
function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ["todos", todoId],
    queryFn: () => fetchTodoById(todoId),
  });
}
```

쿼리 키는 쿼리 함수의 종속성으로 동작한다. 종속 변수를 쿼리 키에 추가하면 쿼리가 독립적으로 캐시되고, 변수가 변경될 때마다 쿼리가 자동으로 다시 가져오게 된다. (설정한 `staleTime`에 따라 다름.)

---

# 4️⃣ useQuery의 주요 옵션이 있다고?

## 📣 staleTime과 gcTime

- `stale`은 용어 뜻대로 `썩은`이라는 의미이다. 즉, 최신 상태가 아니라는 의미이다.
- `fresh`는 뜻 그대로 `신선한`이라는 의미이다. 즉, 최신 상태라는 의미이다.

```tsx
const {
  data,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  gcTime: 5 * 60 * 1000, // 5분
  staleTime: 1 * 60 * 1000, // 1분
});
```

1. staleTime: `(number | Infinity)`
   - `staleTime`은 데이터가 `fresh에서 stale` 상태로 변경되는 데 걸리는 시간, 만약 `staleTime`이 `3000`이면 fresh 상태에서 `3초` 뒤에 stale한 상태로 변환된다.
   - `fresh` 상태일 때는 쿼리 인스턴스가 새롭게 마운트되어도 네트워크 요청(fetch)이 일어나지 않는다.
   - 참고로, `staleTime`의 기본값은 `0`이기 때문에 일반적으로 fetch 후에 바로 `stale`이 된다.
2. gcTime: `(number | Infinity)`
   - 데이터가 사용하지 않거나, `inactive` 상태일 때 `캐싱 된 상태로` 남아있는 시간(밀리초)이다.
   - 쿼리 인스턴스가 언마운트되면 데이터는 `inactive 상태로 변경`되며, 캐시는 `gcTime`만큼 유지된다.
   - `gcTime`이 지나면 `가비지 콜렉터`로 수집된다.
   - `gcTime`이 지나기 전에 쿼리 인스턴스가 다시 마운트되면, 데이터를 fetch하는 동안 캐시 데이터를 보여준다.
   - `gcTime`은 `staleTime`과 관계없이, 무조건 `inactive`된 시점을 기준으로 캐시 데이터 삭제를 결정한다.
   - `gcTime`의 기본값은 `5분`이다. SSR환경에서는 `Infinity`이다.

여기서 주의할 점은 `staleTime`과 `gcTime`의 기본값은 각각 `0분`과 `5분`이다. 따라서 `staleTime`에 어떠한 설정도 하지 않으면 해당 쿼리를 사용하는 컴포넌트가 마운트됐을 때 매번 다시 API를 요청할 것이다.

`staleTime`을 `gcTime`보다 길게 설정했다고 가정하면, `staleTime`만큼의 캐싱을 기대했을 때 원하는 결과를 얻지 못할 것이다. 즉, 두 개의 옵션을 적절하게 설정해 줘야 한다.

<br>

## 📣 refetchOnMount

```tsx
const {
  data,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  refetchOnMount: true,
});
```

- `refetchOnMount`: `boolean | "always" | ((query: Query) => boolean | "always")`
- `refetchOnMount`는 데이터가 `stale` 상태일 경우, 마운트마다 `refetch`를 실행하는 옵션이다.
- `refetchOnMount`의 기본값은 `true`이다.
- `always`로 설정하면 마운트 시마다 매번 `refetch`를 실행한다.
- `false`로 설정하면 최초 fetch 이후에는 `refetch`하지 않는다.

<br>

## 📣 refetchOnWindowFocus

```tsx
const {
  data,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  refetchOnWindowFocus: true,
});
```

- `refetchOnWindowFocus`: `boolean | "always" | ((query: Query) => boolean | "always")`
- `refetchOnWindowFocus`는 데이터가 `stale` 상태일 경우 윈도우 포커싱 될 때마다 `refetch`를 실행하는 옵션이다.
- `refetchOnWindowFocus`의 기본값은 `true`이다.
- 예를 들어, 크롬에서 다른 탭을 눌렀다가 다시 원래 보던 중인 탭을 눌렀을 때도 이 경우에 해당한다. 심지어 F12로 개발자 도구 창을 켜서 네트워크 탭이든, 콘솔 탭이든 개발자 도구 창에서 놀다가 페이지 내부를 다시 클릭했을 때도 이 경우에 해당한다.
- `always`로 설정하면 항상 윈도우 포커싱 될 때마다 refetch를 실행한다는 의미이다.

<br>

## 📣 Polling

```tsx
const {
  data,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  refetchInterval: 2000,
  refetchIntervalInBackground: true,
});
```

> 🌟 **Polling(폴링)이 뭔데?** <br>
> 실시간 웹을 위한 기법으로 "일정한 주기(특정한 시간)를 가지고 서버와 응답을 주고받는 방식이 폴링 방식이다. react-query에서는 "refetchInterval", "refetchIntervalInBackground"을 이용해서 구현할 수 있다.

1. `refetchInterval`: `number | false | ((data: TData | undefined, query: Query) => number | false)`
   - `refetchInterval`은 `시간(ms)`를 값으로 넣어주면 일정 시간마다 자동으로 `refetch`를 시켜준다.
2. `refetchIntervalInBackground`: `boolean`
   - `refetchIntervalInBackground`는 `refetchInterval`과 함께 사용하는 옵션이다.
   - 탭/창이 백그라운드에 있는 동안 `refetch` 시켜준다. 즉, 브라우저에 `focus` 되어 있지 않아도 `refetch`를 시켜주는 것을 의미한다.

> 😲 **refetch는 Next.js에서의 revalidate와 유사한건가?** <br>

그러고 보니까 일정시간에 따라서 데이터를 갱신하는 거면 Next.js에서의 `revalidate`와 유사한 거 아닌가? 궁금해서 찾아보았읍 니 다.

1. 💥 **Tanstack Query의 `refetchInterval`**
   - `refetchInterval`은 밀리초 단위로 시간을 설정하여, 해당 시간 간격마다 자동으로 데이터를 다시 가져오도록 설정하는 것이다. 이를 통해 주기적으로 데이터를 최신 상태로 유지할 수 있다.
2. 🎵 **Next.js의 `revalidate`**
   - Next.js의 `revalidate`는 `getStaticProps` 또는 `getServerSideProps` 함수에서 반환되는 props의 유효 기간을 설정하는 데 사용된다. 이 시간 동안 페이지는 정적 생성된 상태를 유지하며, 유효 기간이 지나면 백그라운드에서 데이터를 다시 가져와 페이지를 갱신한다!

```js
export async function getStaticProps() {
  const data = await fetchData();

  return {
    props: { data },
    revalidate: 60, // 60초마다 재검증
  };
}
```

결론적으로는 두 기능 모두 일정 시간 간격으로 데이터를 최신 상태로 유지하는 데 유용하며, 사용되는 컨텍스트에 따라 차이가 있다.

`Tanstack Query`는 클라이언트 측 데이터 페칭에 중점을 두고 있으며, `revalidate`는 서버 측 정적 페이지 생성을 위한 것이다. 결국 클라이언트 측이냐/서버 측이냐의 차이가 가장 큰 것 같다..

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/7d1e4111-e74d-4b64-a6f8-4351d3e7947f)

<br>

## 📣 enabled refetch

```tsx
const {
  data,
  refetch,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  enabled: false,
});

const handleClickRefetch = useCallback(() => {
  refetch();
}, [refetch]);

return (
  <div>
    {data?.data.map((info: Data) => (
      <div key={info.id}>{info.name}</div>
    ))}
    <button onClick={handleClickRefetch}>Fetch My Info</button>
  </div>
);
```

- `enabled`: `boolean`
- `enabled`는 쿼리가 자동으로 실행되지 않도록 할 때 설정할 수 있다.
- `enabled`를 `false`를 주면 쿼리가 자동 실행되지 않는다.
  - `useQuery` 반환 값 중 `status`가 `pending` 상태로 시작한다.
- `refetch`는 쿼리를 `수동`으로 다시 요청하는 기능이다. 쿼리 오류가 발생하면 오류만 기록된다.
  - 오류를 발생시키려면 `throwOnError` 속성을 `true`로 해서 전달해야 한다.
- 보통 자동으로 쿼리 요청을 하지 않고 `버튼 클릭`이나 특정 이벤트를 통해 요청을 시도할 때 같이 사용한다.
- 주의할 점은, `enabled: false`를 줬다면 `queryClient`가 쿼리를 다시 가져오는 방법 중 `invalidateQueries`와 `refetchQueries`를 무시한다.

<br>

## 📣 retry

```tsx
const {
  data,
  refetch,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  retry: 10, // 오류를 표시하기 전에 실패한 요청을 10번 재시도함.
});
```

- `retry`: `(boolean | number | (failureCount: number, error: TError) => boolean)`
- `retry`는 쿼리가 실패하면 `useQuery`를 특정 횟수만큼 재요청하는 옵션이다.
- `retry`가 `false`인 경우, 실패한 쿼리는 기본적으로 다시 시도하지 않는다. `true`인 경우에는 실패한 쿼리에 대해서 `무한 재요청`을 시도한다.
- 값으로 `숫자`를 넣을 경우, 실패한 쿼리가 해당 숫자를 충족할 때까지 요청을 재시도한다.
- 기본값은 클라이언트 환경에서는 `3`, 서버 환경에서는 `0`이다.

<br>

## 📣 select

```tsx
const {
  data,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  select: (data) => {
    const myName = data.data.map((info) => info.name);
    return myName;
  },
});

return (
  <div>
    {data.map((myName, idx) => (
      <div key={`${myName}-${idx}`}>{myName}</div>
    ))}
  </div>
);
```

- `select`: `(data: TData) => unknown`
- `select` 옵션을 사용하여 쿼리 함수에서 `반환된 데이터의 일부를 변환하거나 선택`할 수 있다.
- 참고로, 반환된 데이터 값에는 영향을 주지만 쿼리 캐시에 저장되는 내용에는 영향을 주지 않는다.

<br>

## 📣 placeholderData

```tsx
const placeholderData = useMemo(() => generateFakeInformation(), []);

const {
  data,
  // ...
} = useQuery({
  queryKey: ["my-info"],
  queryFn: getMyInfo,
  placeholderData: placeholderData,
});
```

- `placeholderData`: `TData | (previousValue: TData | undefined; previousQuery: Query | undefined,) => TData`
- `placeholderData`를 설정하면 쿼리가 `pending` 상태인 동안 특정 쿼리에 대한 placeholder data로 사용된다.
- `placeholderData`는 캐시에 유지되지 않으며, 서버 데이터와 관계없는 보여주기용 `가짜 데이터`다.
- `placeholderData`에 함수를 제공하는 경우 첫 번째 인자로 이전에 관찰된 쿼리 데이터를 수신하고, 두 번째 인자는 이전 쿼리 인스턴스가 된다.

---

# 5️⃣ 마무리

이번 포스팅은 요즘 핫하디 핫한 **Tanstack Query**에 대하여 간략하게 알아보았다.

공부를 해야 하는데 이것이 무엇인지 감도 못 잡고 있어서 공식문서와 깃허브 문서를 활용하여 정리해보았다.

내용만 보았는데도 사람들이 **Tanstack Query**를 왜 사용하는지에 대해 크게 감명을 받았다.

![image](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/ffaf30b4-73c2-498a-9cb5-516ca8a0d1a3)

데이터 fetching할 때에 상태와 데이터를 한꺼번에 관리할 수 있게 도와주는 것으로 코드가 매우 간결해지고, 캐싱된 데이터를 계속 사용할 수도 있다는 것이 정말 신기했다. 또한 네트워크 상태에 따라 데이터를 새로 고치거나 백그라운드에서 업데이트하는 기능도 제공하여 사용자 경험도 향상시킨다니 정말로 GOAT이다.

앞으로 추가적인 내용은 더 포스팅할 계획이다❗️

---

📑 **레퍼런스**

- [**Tanstack Query Docs**](https://tanstack.com/query/latest/docs/framework/react/overview)
- [**React-Query-Tutorial**](https://github.com/ssi02014/react-query-tutorial)

정말 깔끔하게 정리해 주신 기여자 및 개발자 분들 정말 감사합니다.
