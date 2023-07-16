---
title: "[HTTP] 리액트 HTTP 요청"
categories: [http, javascript, react]
tags: [http, react, javascript, firebase]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 다섯 번째 포스팅

안녕하세요! 다섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

비가 엄청나게 내리는 날이에요. 앞으로 중부 지방에 엄청나게 많은 비를 쏟는다고 하네요..☔️ 정말 싫어요 이 꿉꿉함 ㅠㅠ 😿
하지만 뭐,, 집에서 버티면서 공부를 해보아요❗️

지금까지는 DUMMY 데이터를 통해, 어플리케이션 내에 있는 데이터를 로컬에 있는 데이터를 통해 작업 하였습니다.
이번 시간에는 **리액트가 백엔드, 데이터베이스에 연결하는지**를 알아볼 예정입니다!

HTTP 요청을 리액트로부터 백엔드로 보내는 방법에 대해 알아보러 갑시다 ~ 😲

Udemy의 React-The Complete Guide (incl Hooks, React Router, Redux) 강의를 바탕으로 작성된 글입니다.

## 1️⃣ 리액트가 데이터베이스와 통신하는 방법?

리액트 앱이나 브라우저에서는 자바스크립트 코드가 데이터베이스와 직접 통신하면 절대 안됩니다 !!

각 서버에서 본인들만의 것을 작업하는 것은 괜찮지만 앱으로 직접 데이터를 가져오거나 저장하며 연결 맺는 그런 행동은 절대 해서는 안됩니다❗️

왜냐구요? **데이터베이스의 정보를 노출시키는 행위니까요.**

우리가 작성하는 자바스크립트 코드는 어디서나 접근 가능하고 읽을 수 있어요. 개발자 도구만 열어도 코드가 보이기 때문에 보안과 관련된 세부 사항을 노출하는 것이 문제가 될 수 있기 때문이지요.

> **그 래 서**

**우리는 백엔드 앱을 이용해야 합니다.** 백엔드 앱은 브라우저 안에서 실행되지 않고 타 서버(ex. `node.js`, `PHP`, `AHP.NET` ...)에서 실행되기 때문입니다.

백엔드 앱은 데이터베이스와 연결이 가능합니다. 사용자는 백엔드 코드를 볼 수 없기 때문에 데이터베이스의 인증 정보를 안전하게 저장 가능합니다.

**따라서, 리액트는 백엔드 서버, 백엔드 API라고 불리는 서로 다른 URL로의 요청을 전송하는 서버와 통신하게 됩니다.**

## 2️⃣ 실습에 앞선 선행사항

우리는 버튼을 클릭하면 STAR WARS API에서 영화 정보를 가져오는 앱을 통해 리액트 통신 방법에 대해 알아볼 겁니다.
[`SWAPI사이트`](https://swapi.dev/) 를 통해 데이터를 가져올 것입니다. 이 더미 API에는 GET 요청을 해서 실습 가능한 데이터들이 있습니다.
**이것은 API일뿐 절대 데이터베이스가 아닙니다.**
리액트는 데이터베이스와 직접 연결이 안된다고 했었죠❗️

> **API**: **A**pplication **P**rogramming **I**nterface

이것을 통해 명확히 정의된 인터페이스를 다루고 작업에 대한 규칙이 명확하게 정의된 것을 다루고 있다는 것입니다.
`SWAPI`는 `REST API`입니다.

> `REST`란?
>
> REST(Representational State Transfer)
> 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 것입니다.
> HTTP URI를 통해 자원을 명시 후 HTTP 통신 방법을 통해 URI에 대한 CRUD를 적용하는 것을 말합니다.

그래서 `SWAPI`에 요청을 전송하면 특정 형식에 맞춰 데이터를 전달해줍니다.
`swapi.dev/api/films`에 요청을 전달하면 JSON형식의 데이터가 뜨게 될 것입니다.

**앞으로 리액트에서는 무엇을 하면 될까요?**

## 3️⃣ GET 요청 보내기

`GET` 요청을 보내보도록 하겠습니다. `GET` 요청을 보내서 `SWAPI`에 있는 정보를 가져와서 렌더링 시키는 과정을 설명 드리도록 하겠습니다.

자바스크립트에는 HTTP 요청을 전송하는 내장 메커니즘이 생겼습니다. **그것은 바로 `Fetch API`입니다.**
**`Fetch API`는 브라우저 내장형이며 데이터를 불러오고 데이터 전송까지 가능합니다!**
이 API를 통해서 HTTP 요청 전송과 응답 처리를 가능합니다.

`App.js`

```jsx
return (
  <React.Fragment>
    <section>
      <button>Fetch Movies</button>
    </section>
    <section>
      <MoviesList movies={dummyMovies} />
    </section>
  </React.Fragment>
);
```

여기에서 버튼을 클릭하면 `SWAPI`에서 영화 정보를 가져오려고 합니다.

`App.js`안에

```jsx
function fetchMoviesHandler() {
  fetch("https://swapi.dev/api/films");
}
```

다음과 같은 함수를 넣어줍니다. fetch하기 위한 함수이며,
`fetch()`는 내장형 함수이고 API를 `GET`한다는 말입니다.

```jsx
function fetchMoviesHandler() {
  fetch("https://swapi.dev/api/films", {});
}
```

`fetch()`는 다음과 같이 두 번째 인자도 객체형식으로 가질 수 있습니다. 이 안에 body, headers, HTTP 요청 메소드의 변경 등을 삽입 가능합니다.<br/>
`fetch()`는 default로 GET을 사용하기 때문에 지금은 사용하지 않도록 합니다.<br/>
`fetch()`는 프로미스라는 객체를 반환합니다. 이 객체는 우리가 잠재적으로 발생할 수 있는 오류나 호출에 대한 응답에 반응할 수 있게 해줍니다.<br/>

그래서 `promise`가 반환 되었는데 어떤 즉각적 행동 대신 어떤 데이터를 전달하는 객체입니다.

### **Http 요청 전송은 비동기 작업!**

> 비동기가 뭔가요?

비동기(Asynchronous)는 동시에 일어나지 않는다는 뜻입니다. 이 뜻은 요청과 결과가 동시에 일어나지 않는다는 의미입니다.
요청과 응답이 다른 시간대에 존재하므로 즉시 끝나는 작업이 아닙니다.

```jsx
function fetchMoviesHandler() {
  fetch("https://swapi.dev/api/films", {}).then();
}
```

`fetch()`가 끝나고 응답을 받을 때 호출되는 `then()`을 뒤에 작성해 줍니다.

API는 데이터를 `JSON` 형식으로 전송해줍니다. `JSON`은 데이터 교환에 사용하는 간단한 형식입니다. `JSON`을 찾아보게 되면 마치 자바스크립트 객체와 같은 문법을 취하고 있습니다.
키 값은 큰따옴표로 묶이고 값은 일반적으로 써줍니다.
자바스크립트에서 `JSON`으로의 변환은 다음과 같습니다.

```jsx
function fetchMoviesHandler() {
  fetch("https://swapi.dev/api/films", {}).then((response) => {
    return response.json();
  });
}
```

`response` 객체에 있는 내장 메소드로 `response`의 본문을 코드에서 사용할 수 있는 자바스크립트 객체로 자동 변환을 해줍니다.
`response` 객체 또한 프로미스 객체이므로 반환 해준 뒤 `then()`을 사용 가능합니다.

```jsx
fetch("https://swapi.dev/api/films", {})
  .then((response) => {
    return response.json();
  })
  .then((data) => {
    data.results;
  });
```

`swapi.dev/api/films`에 나온대로 `results`는 전부 배열로 이루어져 있습니다.
이제 `fetchMoviesHandler()`메소드가 실행되면 영화가 `GET`한 대로 가져올 수 있도록 `useState`를 이용하여 `data.results`로 바꿔줄 수 있도록 해줍니다.

```jsx
<Movie
  key={movie.id}
  title={movie.title}
  releaseDate={movie.releaseDate}
  openingText={movie.openingText}
/>
```

만들어진 변수명은 이렇지만 `SWAPI`에서는 `opening_crawl`, `title`, `release_date`의 조금 다른 변수명을 갖고 있습니다. 이것에 맞춰 들어오는 데이터의 형식을 원하는 형식으로 바꿔야 합니다.

```jsx
fetch("https://swapi.dev/api/films", {})
  .then((response) => {
    return response.json();
  })
  .then((data) => {
    const transformedMovies = data.results.map((movieData) => {
      return {
        id: movieData.episode_id,
        title: movieData.title,
        openingText: movieData.opening_crawl,
        releaseDate: movieData.release_date,
      };
    });
    setMovies(data.results);
  });
```

위처럼 `map()`을 사용하여 바꿔줄 수 있습니다.

```jsx
<button onClick={fetchMoviesHandler}>Fetch Movies</button>
```

이제 버튼을 클릭하면 가져온 영화 정보가 뜰 수 있게 됩니다.
<br/>
<br/>
![블로그4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/45c7c4e0-2e6a-4f9d-bd67-9fb91f16efea) <br/>

바로 이렇게 말이죠.

**지금까지 `SWAPI`에서 영화 정보를 `GET`요청 보내는 방법에 대해 알아 보았습니다.**

## 4️⃣ async/await 사용하기

`fetch()`는 `promise`를 반환한다고 했었습니다.
그래서 `then()`도 사용 가능했구요.
하지만, `then()`을 계속 사용하다 보면 then체인에 빠질 수 있습니다.
그것을 피하기 위해 나온 문법이 `async`와 `await`입니다.
프로미스를 반환하는 작업 앞에 `await`을 붙일 수 있습니다.

```jsx
async function fetchMoviesHandler() {
  const response = await fetch("https://swapi.dev/api/films", {});
  const data = await response.json();

  const transformedMovies = data.results.map((movieData) => {
    return {
      id: movieData.episode_id,
      title: movieData.title,
      openingText: movieData.opening_crawl,
      releaseDate: movieData.release_date,
    };
  });
  setMovies(transformedMovies);
}
```

위처럼 코드를 변환 가능합니다. 코드는 더욱 더 간단해지며 **비동기화 코드** 가 되었습니다.
프로미스를 다룰 때는 이렇게 다른 방법을 사용할 수 있습니다. <br/>
**이것은 리액트의 기능이 아닌 자바스크립트 기본 기능입니다❗️**

## 5️⃣ `useState`를 통한 로딩 처리

버튼을 클릭하면 fetch 하는 데에 로딩 시간이 걸립니다.
실제로는 로딩 텍스트나 로딩 아이콘을 통해 표시하곤 합니다. <br/>
로딩 텍스트를 통해 현재 데이터를 불러오고 있다는 신호를 `useState`를 통해 보일 것입니다.

```jsx
const [isLoading, setIsLoading] = useState(false);
```

다음과 같이 선언을 해줍니다.
`fetchMoviesHandler()`함수가 시작될 때부터 로딩이 시작되니 그 때부터 로딩이 시작하고 이 함수 마지막에 다시 상태를 바꿔주면 됩니다.

```jsx
async function fetchMoviesHandler() {
    setIsLoading(true);
    ...
    setIsLoading(false);
}
```

로딩중이 아닐 때와 영화가 한 편 초과일 때는 영화 목록을 그대로 렌더링 해줍니다.<br/>
또한, 로딩중이 아니고 영화가 없다면 텍스트를 렌더링 해줍니다. <br/>
로딩중일 때는 텍스트를 렌더링 해줍니다. <br/>

```jsx
return (
  <>
    <section>
      <button onClick={fetchMoviesHandler}>Fetch Movies</button>
    </section>
    <section>
      {!isLoading && movies.length > 0 && <MoviesList movies={movies} />}
      {!isLoading && movies.length === 0 && <p>Found no movies.</p>}
      {isLoading && <p>Loading...</p>}
    </section>
  </>
);
```

위처럼 작성하면 로딩에 따른 텍스트를 렌더링할 수 있습니다.<br/>
간단한 작업이니 간단하게 설명했습니다.

## 6️⃣ Http 오류 처리 ?

통신 과정에서 네트워크 연결이 없거나 요청이 전송 되었는데 오류 응답 코드를 받을 수 있습니다. <br/>

> HTTP 상태 코드에 대한 자세한 내용이 궁금하시다면 여기에 들어가셔서 확인하고 오시면 됩니다.
> [`HTTP 상태 코드`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)

보통 2xx로 시작하는 코드는 정상적인 응답을 뜻합니다. <br/>
하지만 4xx는 보통 요청이 성공적으로 전송되었고 이 과정에서는 아무런 문제가 없는데 서버가 요청을 받았으나 원하는 응답을 주지 않았음을 의미합니다.

오류를 처리하기 위해 다음과 같이 유효하지 않은 링크로 만들어줍니다.

```jsx
const response = await fetch("https://swapi.dev/api/film");
```

이 상태로 요청을 보내면 영화 데이터는 돌아오지 않습니다.
개발자 도구를 열어보면 오류 코드는 404로 뜨게 됩니다.

이런 오류를 처리하기 위해서는 일단 state를 사용해야 합니다.

```jsx
const [error, setError] = useState(null);
```

```jsx
setError(null);
```

`setError`는 함수 첫 줄에 작성하여 함수가 실행 될 때마다 전에 받았던 오류를 초기화 시키는 상태입니다.

**하지만, 오류가 실제로 발생했다면 null 이외의 값을 사용해야 합니다.**

> `async`와 `await`을 사용하지 않고 `then`을 사용하였다면

```jsx
const response = await fetch("https://swapi.dev/api/films").then().catch();
```

이런식으로 `catch()`를 추가하여 오류를 확인하면 됩니다.

> 하지만 우리는 `async`와 `await`을 사용하였기에 `try-catch`를 통해 오류를 확인할 수 있습니다.

try블럭 안으로 집어넣고 오류가 발생한다면 catch로 가서 오류를 확인할 수 있게 됩니다.

### fetch API는 어떻게 오류 발생하게 만들까?

> fetch API는 에러 상태 코드를 실제 에러로 취급하지 않습니다. 실제 에러 코드를 받아도 기술적인 오류로 처리하지 않는다는 것입니다.

> 가져오지 못한 데이터로 어떤 작업을 하려 할 때만 오류가 발생하게 됩니다.
> 그러므로 오류 코드를 받았을 때 실제 오류가 발생하게끔 만들어야 합니다.

```jsx
if (!response.ok) {
  throw new Error("Something went wrong!");
}
```

이런 구문을 통해 응답이 정상적이지 않다면 에러가 발생하게끔 걸어줍니다.<br/>
만약 에러가 발생하면 그 아래 데이터를 가져오지 않을 것입니다.<br/>
**response 바디 부분을 파싱하기 전에 응답이 ok인지 확인을 해줘야 합니다.** <br/>
▷ 그러므로, data를 json()형식으로 변환하기 전에 이 코드를 작성해 줍니다❗️

```jsx
try {
  const response = await fetch("https://swapi.dev/api/films");
  const data = await response.json();

  if (!response.ok) {
    throw new Error("Something went wrong!");
  }

  const transformedMovies = data.results.map((movieData) => {
    return {
      id: movieData.episode_id,
      title: movieData.title,
      openingText: movieData.opening_crawl,
      releaseDate: movieData.release_date,
    };
  });
  setMovies(transformedMovies);
} catch (error) {
  setError(error.message);
}
setIsLoading(false);
```

이렇게 예외 처리를 해주면 에러가 발생하면 위에 에러를 설정한대로 메시지를 띄우게 될 것입니다.

에러가 발생하면 로딩을 중단할 수 있게끔 맨 아래에 작성해줍니다.

에러에 따른 조건부 렌더링을 설정 해줘야 합니다.

```jsx
<section>
  {!isLoading && movies.length > 0 && <MoviesList movies={movies} />}
  {!isLoading && movies.length === 0 && !error && <p>Found no movies.</p>}
  {!isLoading && error && <p>{error}</p>}
  {isLoading && <p>Loading...</p>}
</section>
```

이렇게 error를 추가하여 렌더링 되도록 할 수 있습니다.

> 하지만, 이렇게 하는 방법 말고도 또 다른 방법이 있습니다.

```jsx
let content = <p>Found no movies.</p>;

if (movies.length > 0) {
  content = <MoviesList movies={movies} />;
}

if (error) {
  content = <p>{error}</p>;
}

if (isLoading) {
  content = <p>Loading...</p>;
}
```

이렇게 한 후

```jsx
<section>
    {content}
<section/>
```

이런 식으로 처리하는 방법도 존재합니다.

## 7️⃣ `useEffect()`훅을 사용해 바로 렌더링하기

다른 웹페이지들을 보면 어디선가 데이터를 가져오는데 버튼 누르지 않고도 로딩 시간을 줄여 바로 렌더링이 되곤 합니다. <br/>
**우리는 그것을 `useEffect()`를 이용하여 구현할 수 있습니다.**<br/>

```jsx
useEffect(() => {
  fetchMoviesHandler();
}, [fetchMoviesHandler]);
```

[]안에 의존성을 추가하지 않는다면 첫 화면 렌더링될 때만 실행되고 그 이후로는 버튼을 클릭해야 데이터를 가져올 것입니다.

의존성에 `fetchMoviesHandler`를 추가한다면 함수가 변경될 때마다 이펙트는 수행될 것입니다.

**하지만! 저런식으로 작성하면 무한루프가 형성되기 마련입니다.** <br/>

> 따라서 `useCallback`을 사용하여 루프형성을 막아줄 수 있습니다.

```jsx
async function fetchMoviesHandler() {}
```

이것을 변경할 수 있습니다.

```jsx
const fetchMoviesHandler = useCallback(async () => {}, []);
```

비동기도 화살표함수를 통해 이런식으로 표현 가능합니다.

**데이터를 즉시 fetch 할 수 있게 됩니다❗️**

## 8️⃣ POST 요청 보내기

데이터 fetch로 끝나지 않고 서버로 데이터를 보내는 작업도 합니다. <br/>
![블로그5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/17182fe9-565a-440f-8644-62b800f0240f)<br/>

POST 실습을 위해 다음과 같은 폼을 작성하고 버튼을 누르면 firebase에 정보가 전달되고 그것을 다시 fetch 해오는 것을 해볼겁니다!

#### ❗️firebase URL 삽입

> `firebase`가 뭔가요? <br/>
> 이 서비스는 구글이 제공하고 코드 작성 없이 사용 가능한 백엔드입니다. <br/>
> 이것은 데이터베이스가 아니며, 데이터베이스에 딸린 백엔드고 완전한 REST API를 제공하는 풀 백엔드 어플리케이션입니다.

`firebase`에서 구글 계정으로 로그인한 후 프로젝트를 아무 이름으로 개설하시면 됩니다. <br/>
그리고 Realtime Database → Start in Test Mode 로 Enable 해주시면 됩니다. <br/>

그러면 간단한 데이터베이스가 만들어진 후 이 데이터베이스와 통신할 URL이 생성됩니다.

> **겉보기엔 프론트엔드와 직접 통신하는 데이터베이스라고 생각하실 수도 있는데 이 URL은 firebase REST API에 대한 URL이며 백그라운드의 데이터베이스와 통신하므로 실제로는 이런 통신 방법을 사용함을 말씀 드립니다.**

```jsx
const response = await fetch("https://swapi.dev/api.films/");
```

이제 fetch해 올 URL은 아래와 같게 됩니다.

```jsx
const response = await fetch(
  "https://react-http-14653-default-rtdb.firebaseio.com/movies.json"
);
```

**위 URL은 각자 생성된 URL을 붙여넣기 바랍니다.**

```jsx
$(nodes.name).json;
```

URL 맨 뒤에 붙은 movies는 마음대로 설정하셔도 됩니다. 데이터베이스에 새로운 노드가 만들어지게 됩니다.

> **이것은 동적 REST API로 서로 다른 세그먼트를 사용해 데이터베이스의 서로 다른 노드들에 데이터를 저장할 수 있게 설정해주는 것입니다.**

**`json` 확장자는 `firebase`의 요구 사항으로, 요청을 전달하려는 URL 끝에 .json을 추가 해줘야만 합니다.**

이제 POST를 위한 작업은 끝났습니다. <br/>
이대로 실행하면 렌더링에서 다른 텍스트가 뜨는데 이것은 상태 코드는 200이지만 데이터가 없기 때문에 받아오지 못한 것입니다.

> POST를 위한 코드를 작성하겠습니다.
> `App.js`

```jsx
<section>
  <AddMovie onAddMovie={addMovieHandler} />
</section>
```

새로운 영화를 추가하기 위한 비동기 함수를 만들어 봅시다.

```jsx
async function addMovieHandler(movie) {
  const response = await fetch(
    "https://react-http-14653-default-rtdb.firebaseio.com/movies.json",
    {}
  );
}
```

비동기 함수로 아까 받아온 URL을 fetch 해옵니다. <br/>
하지만 POST 요청을 해야 하기 때문에 `fetch`에 두 번째 인자를 활용합니다.

```jsx
async function addMovieHandler(movie) {
  const response = await fetch(
    "https://react-http-14653-default-rtdb.firebaseio.com/movies.json",
    {
      method: "POST", // default는 GET
    }
  );
}
```

이렇게 하면 firebase는 리소스를 형성할 것입니다.
그러므로 저장해야 하는 리소스를 만들어줘야 합니다.

이를 위해서 fetch API 구성 객체에서 `body`라는 객체를 추가해줍니다.
**`body`는 자바스크립트 객체가 아닌 JSON 데이터를 필요로 합니다. <br/> 따라서 JSON으로 바꿔주려면 `stringify()`를 사용해야 합니다.**

```jsx
async function addMovieHandler(movie) {
  const response = await fetch(
    "https://react-http-14653-default-rtdb.firebaseio.com/movies.json",
    {
      method: "POST", // default는 GET
      body: JSON.stringify(movie),
      headers: {
        "Content-Type": "application/json",
      },
    }
  );
}
```

headers 키를 추가하고 값으로 객체를 지정하면 됩니다.

> `firebase`에는 위와 같은 헤더는 필요하지 않습니다. <br/>
> 요청을 받는 대다수의 API들은 이러한 헤더를 필요로 합니다. 이 헤더를 통해 어떤 컨텐츠가 전달되는지 알 수 있습니다.

```jsx
async function addMovieHandler(movie) {
  const response = await fetch(
    "https://react-http-14653-default-rtdb.firebaseio.com/movies.json",
    {
      method: "POST", // default는 GET
      body: JSON.stringify(movie),
      headers: {
        "Content-Type": "application/json",
      },
    }
  );
  const data = await response.json();
  console.log(data);
}
```

위처럼 data를 json형식으로 바꿔 써줍니다.
**`firebase`가 전달하는 데이터베이스는 json형식이니까요.**

이제 POST 요청을 보낼 수 있습니다❗️❗️❗️

렌더링 된 폼 형식에 영화 제목-내용-개봉일을 적은 후 `firebase`를 보면 아까 노드의 이름으로 정한 `movies`라는 노드가 생성 되었을 것입니다. <br/>

그 노드 안에는 `firebase`가 자동 생성한 암호화된 ID가 있습니다. 그 안에 이제 우리가 전송한 데이터가 존재합니다.

이제 POST 요청은 정상적으로 작동합니다.

> **하지만❗️** <br/>
> fetch 버튼을 클릭하면 우리가 입력한 영화 정보가 나오지 않습니다❓

`GET`요청에서 data.results.map방식으로 가져왔는데 이제 results는 존재하지 않고 key를 통해 가져옵니다.

**그 이유는 콘솔 창을 보면 실제 영화 데이터는 중첩된 객체로 나타납니다. 배열로 가져오지 않고 객체로 받아온 것이죠.**

```jsx
const loadedMovies = [];

for (const key in data) {
  loadedMovies.push({
    id: key,
    title: data[key].title,
    openingText: data[key].openingText,
    releaseDate: data[key].releaseDate,
  });
}

setMovies(loadedMovies);
```

for루프를 통해 데이터에 있는 모든 키를 확인합니다.
그리고 전달받은 데이터의 키와 값들의 조합에 대해 새로운 객체를 push 해줍니다. <br/><br/>

![블로그6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/57522760-9ddd-4fd3-9e72-57adc6aa1a3a) <br/>

데이터들은 중첩 객체이므로 저렇게 표현해줍니다.

이제 이렇게 완성 하면 내가 `POST` 요청한 영화 정보를 `fetch`할 수 있게 됐습니다.
<br/><br/>

![블로그7](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d0fefcc3-d46f-42f9-9e2e-553e73e97857)

이렇게 말이죠 ㅎㅎ^^💕

이렇게 해서 `GET`요청을 설정하고 데이터를 가져오는 것과 `POST`요청을 통한 데이터 저장에 대해 알아 보았습니다 ㅎㅎ

---

긴 글 읽어주셔서 감사합니다 ~ 💛
