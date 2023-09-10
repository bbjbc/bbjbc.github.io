---
title: "[Network] REST API 이해"
categories: [network, javascript]
tags: [express, javascript, rest api]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열일곱 번째 포스팅

안녕하세요! 열일곱 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

결국 수강신청 대성공 했어요 ㅎㅎㅎㅎ 18학점을 잡았다는 말이에용 ㅎㅎ <br>
오픈소스과목은 어려울 것으로 예측되어 다른 과목을 잡았어요 :)😝 <br>

오늘의 포스팅 내용은 **REST API**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ REST API란?

## 🎈 사용 이유

> **REST** - **Re**presentational **S**tate **T**ransfer<br>
> 간단히 말하면 사용자 인터페이스 대신 데이터 전송을 의미합니다.

많은 최신 웹 애플리케이션들은 많은 HTML 콘텐츠를 포함하지 않은 초기 HTML 페이지만 가져오는 대신 JavaScript 스크립트 파일을 많이 로딩하며 스크립트 파일이 백엔드 RESTful API에 도달해 사용자 인터페이스에 재렌더링해야 하는 데이터만 가져오도록 합니다.

클릭만 하면 새로 고침을 기다릴 필요 없이 한 페이지에 머물면서 렌더링 되는 데이터만 배후에서 바뀌고 사용자 인터페이스는 브라우저 측 JavaScript를 통해 렌더링됩니다.

HTML 대신 데이터만 전송하고 클라이언트 및 프론트엔드가 모바일 앱이나 SPA든 전송받은 데이터로 작업하도록 하는 것입니다. 분리된 프론트엔드를 구축해야 하는 애플리케이션에서 이 REST API가 좋은 해결책이 될 것입니다❗️<br/>
또한 응답과 요청 데이터만 바뀌고 전체적인 서버논리는 바뀌지 않습니다. 응답이나 데이터 측면에서나 다를 뿐이고 서버에서 일어나는 일은 변화가 없다는 것입니다.

<br>

## 🎈 원리

서버와 클라이언트가 있을 때 클라이언트는 모바일 앱이나 SPA이고 서버에 API를 구축합니다. 이 때 여러 클라이언트를 위해 하나의 API를 사용할 수 있습니다. <br>
기존 웹 앱 등 애플리케이션에 서비스 API를 추가하거나 서비스를 판매하기 위해 직접 서비스 API를 구축할 수도 있습니다. 이처럼 대부분의 경우에 클라이언트와 서버 사이에 데이터를 교환합니다. 어떤 형식으로 교환할까요?

HTML 외에도 요청과 응답에 부착할 수 있는 다양한 데이터 유형이 있습니다. 일반 text, XML, JSON 등 입니다.

1. **HTML**<br>
   EJS 뷰를 렌더링할 때 브라우저에 HTML 코드를 보내 뷰가 서버에 렌더링 되도록 하고 렌더링 과정의 결과가 HTML 페이지인 HTML 코드였습니다. 이렇게 데이터를 보낸 것처럼 HTML 코드는 데이터와 구조를 포함합니다. <br>
   구조와 디자인에 추가할 HTML 요소와 CSS 스타일 등이 있고 요소들 사이에는 데이터가 있습니다. 이들이 정리되어 있어도 구조화하는 방법은 정해져 있지 않기 때문에 불필요하게 분석이 어려워집니다.

2. **Plain Text**<br>
   일반적인 텍스트는 데이터만 포함하고 구조나 디자인 요소는 없으므로 UI를 구축할 수는 없습니다. 일반 텍스트로 데이터를 전달하는 경우는 컴퓨터가 이해하기 어렵기 때문에 불필요하게 분석이 어렵습니다. 그러므로 데이터 교환에 좋은 방법이 절대 아닙니다.

3. **XML**<br>
   일반 텍스트보다 컴퓨터가 읽기 쉽습니다. 명확한 구조를 정의할 수도 있지만 특별한 XML parser가 필요합니다. 요소를 전송하는 데이터에 일종의 오버헤드를 추가해 핵심 데이터가 아니라 데이터를 읽기 위해 추가적으로 텍스트도 많이 필요합니다.

4. **JSON**<br>
   가장 훌륭한 데이터 형식인 JSON입니다. 이것은 UI를 가정하지 않고 기계가 읽을 수 있습니다. XML보다 간결해서 JS로 변환하기 쉽다는 점이 서버에서 작업하거나 브라우저의 JS 작업 시 큰 이점이 될 수 있습니다. <br>
   따라서 데이터만 전송하고 싶을 때 가장 좋은 형식이며 API와 소통하는 데 흔히 사용합니다.

다른 데이터 형식들은 JSON 만큼 데이터 전송에 적합하지 않으므로 앞으로는 JSON을 사용할 것입니다.

<br>

## 🎈 라우팅

클라이언트에서 서버로 어떻게 요청을 보낼까요?<br>
지금까지는 HTML에 링크를 추가하거나 폼에 버튼을 추가하여 액션과 메소드를 정의했습니다.

REST API 또한 요청과 함께 HTTP 메소드로 서버에 있는 경로를 보냅니다. 서버 측 라우팅에 경로를 정의하고 들어오는 요청을 기다리며 경로가 처리할 HTTP 메소드를 정의해서 모든 경로에 아무 요청이나 도달하지 않도록 합니다. 브라우저 작업 시 요청은 클라이언트로부터 비동기식 JavaScript를 통해 보내집니다.

```js
POST /post
GET /posts
GET /posts/:postId
```

위와 같은 것들을 API 엔드 포인트라고 부릅니다. POST나 GET 같은 HTTP 메소드와 각 경로를 말하는 것입니다. <br>
REST API에 엔드 포인트를 정의하고 요청이 엔드 포인트에 도달했을 때 서버에 실행할 논리를 정의합니다.

<br>

## 🎈 HTTP 메소드

HTTP 메소드는 GET이나 POST 이외에도 많이 존재합니다. 브라우저에 있는 JavaScript는 다루지 않는 경우에는 GET이나 POST만 사용 가능합니다. 브라우저 혹은 HTML 요소가 기본적으로 알고 있는 기본적인 메소드인 것이죠.

JavaScript를 통해 비동기식 요청을 처리할 때나 HTTP 클라이언트를 사용할 때는 다양한 HTTP 메소드를 사용 가능합니다.

- **GET**<br>
  Get a Resource from the Server
- **POST**<br>
  Post a Resource to the Server (i.e. create or append Resource)
- **PUT**<br>
  Put a Resource onto the Serve (i.e. create or overwrite a Resource)
- **PATCH**<br>
  Update parts of an existing Resource on the Server
- **DELETE**<br>
  Delete a Resource on the Server

<br>

## 🎈 REST API 핵심 원칙

1. **Uniform Interface** <br>
   Clearly defined API endpoints with clearly defined request + response data structure<br>
2. **Stateless Interactions**<br>
   Server and client don't store any connection history, every request is handled seperately
3. **Cacheable**<br>
   Servers may set caching headers to allow the client to cache responses
4. **Client-Server**<br>
   Server and client are separated, client is not concerned with persistent data storage
5. **Layered System**<br>
   Server may forward requests to other APIs
6. **Code on Demand**<br>
   Executable code may be transferred from server to client

<br>

- **일관된 인터페이스 원칙**<br>
  API에 명확히 정의된 API 엔드 포인트를 가져야 한다는 것을 의미합니다. 엔드 포인트는 명확하게 정의된 요청 및 응답 데이터 구조를 가진 HTTP 메소드 및 경로의 조합입니다. <br>
  API는 예측 가능해야 하고 공개된 경우에는 문서화가 잘 되어 있어야 합니다. 또한, 엔드 포인트에 도달하면 일어나는 일은 시간이 지나도 바뀌지 않아야 하며 예측 가능하고 명확하게 정의 되어야 합니다❗️
- **무상태 상호작용 원칙**<br>
  이것은 인증에 있어 중요한 개념입니다. REST API 구축 시 클라이언트와 서버는 완전히 분리되어 히스토리를 공유하지 않습니다. 따라서 연결 히스토리가 저장되지 않고 들어오는 모든 요청에 대해 사전 요청을 보내지 않은 것으로 처리하므로 어떤 세션도 사용하지 않습니다. <br>
  서버가 각 요청을 직접 들여다보게 되며 클라이언트를 전혀 신경 쓰지 않습니다. API를 FE에서 개발하기 위해 같은 서버에서 실행하게 되더라도 둘은 분리된 상태로 따로 작동하여 데이터만 주고 받습니다. <br>
  따라서 새로운 엔드 포인트를 설정할 때마다 이전 요청과 독립적으로 기능하는지 확인해야만 합니다.
- **캐싱 원칙**<br>
  이것은 REST API에서 헤더를 전송해 클라이언트에게 응답의 유효 기간을 알려줌으로써 클라이언트가 응답을 캐싱 가능하도록 해주는 기능입니다.
- **계층형 시스템**<br>
  클라이언트가 API에 요청을 보낼 때 해당 요청을 받은 서버가 요청을 즉시 처리하는 대신 다른 서버로 전달하거나 분배할 수 있다는 것입니다.

---

# 2️⃣ 요청 및 응답 보내기

REST API는 HTML을 렌더링하지 않고 데이터를 통해 렌더링한다고 했었죠.

[controllers/feed.js]

```js
exports.getPosts = (req, res, next) => {
  res.status(200).json({
    posts: [{ title: "First Post", content: "This is the first post!" }],
  });
};
```

`res.render`를 더 이상 사용하지 않고 json 메소드를 사용해 줍니다. 더미 데이터를 넣어서 json 형태로 렌더링 되도록 합니다.

[routes/feed.js]

```js
const express = require("express");

const feedController = require("../controllers/feed");

const router = express.Router();

// GET /feed/posts
router.get("/posts", feedController.getPosts);

module.exports = router;
```

[app.js]

```js
const express = require("express");

const feedRoutes = require("./routes/feed");

const app = express();

app.use("/feed", feedRoutes);

app.listen(8080);
```

이제 서버 실행을 하여 `localhost:8080/feed/posts`로 들어갑니다. <br>

![rest](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/9e15a0f6-1400-4c72-83d3-dbcafb9970ac)<br>

위와 같이 json 형식의 데이터가 렌더링된 것을 확인할 수 있습니다.

하지만 이렇게 직접 액세스하는 것은 번거롭고 계획했던 것이 아닙니다. 브라우저에 이를 입력하지 않고도 빠르고 쉽게 REST API를 테스트하는 방법이 있습니다.

브라우저에 입력 불가능한 post 라우트를 정의합니다.

[post Handler]

```js
exports.createPost = (req, res, next) => {
  const title = req.body.title;
  const content = req.body.content;
  res.status(201).json({
    message: "Post created successfully!",
    post: { id: new Date(), title: title, content: content },
  });
};
```

[routes]

```js
// POST /feed/post
router.post("/post", feedController.createPost);
```

상태 코드 201은 클라이언트에게 리소스 생성 성공을 알리는 코드입니다.<br>
이제 데이터를 파싱해야 합니다. bodyParser를 통해 설정하였지만 우리는 json 데이터로 작업하고 있습니다. 우리가 json 데이터를 반환하듯 클라이언트도 포함하는 요청을 통해 API와 통신하게 됩니다.

그래서 요청과 응답 두 곳에서 json 데이터 형식을 사용합니다.

```js
const bodyParser = require("body-parser");

app.use(bodyParser.urlencoded());
```

전에 했듯 `urlencoded`를 호출하여 초기 설정 했었습니다. 이것은 `x-www-form-urlencoded` 형식의 데이터를 담고 있는 데이터 포맷 또는 요청에 대해서는 좋은 방법입니다. 그러나 form 데이터가 필요하지 않습니다. 왜냐하면 REST API에서는 폼을 사용하지 않기 때문이죠.

form 데이터가 없는 대신 json 메소드로 bodyParser를 사용해서 들어오는 요청으로부터 json 데이터를 분석하게 됩니다. 이것은 application/json에 사용하기 좋은 방식으로 헤더에서 볼 수 있습니다.

```js
app.use(bodyParser.json());
```

body에서 추출하기 위해 위와 같은 미들웨어가 필요합니다. 들어오는 요청이 body 필드에 추가되는 것이죠.

이제 이것을 테스트 해보는 매우 편리한 특수 도구가 있습니다. 그것은 바로~ **Postman**입니다~
이것은 매우 유용한 API 개발 도구입니다.<br>

![rest1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/4bad48c2-c00b-4d41-850f-8f6e58048122)<br>

여기에 url을 입력하여 HTTP 메소드를 선택하여 요청을 보낼 수 있습니다. 또한 원하는 헤더를 추가해서 아래 영역에서 응답을 확인할 수 있습니다.

아까 추가한 post 라우트로 도달 시켜 보겠습니다. <br>

![rest2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/c7f6dd47-5087-4e5a-a2a4-c786b457b4c3)<br>

위쪽 body/json 탭에서 json 데이터를 작성 가능합니다. 필요한 `title`, `content`를 추가 해주고 send 해주면 아래에 응답을 받은 것을 볼 수 있습니다. <br>
컨트롤러 액션에서 message와 생성된 post로 정의된 응답인 것입니다. 사진 아래가 수신 클라이언트에 대해 일반적으로 사용하게 될 데이터입니다.

필요한 추가 헤더 및 본문을 입력해서 모든 엔드포인트를 테스트할 수 있으며 API를 테스트하는데 필요한 것을 갖추게 됩니다.

---

# 3️⃣ CORS

> **CORS** - **C**ross-**O**rigin **R**esource **S**haring<br>
> 이것은 브라우저에서 기본적으로 허용되지 않습니다.

## 💡 CORS 오류

![cors](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/84d78add-7bf5-4726-b1dc-704ad57858a4)<br>

CORS 오류를 보기 위해서 위와 같이 코드펜에서 다음과 같이 작성해줍니다. <br>
fetch 키워드로 최신 브라우저에 내장된 fetch API를 사용하여 요청을 보내고자 하는 위에서 정의한 라우트를 작성합니다. 위와 같이 작성 후 버튼을 클릭하고 콘솔창을 확인하면 이렇게 오류가 확인됩니다. <br>

![cors2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/f6e5e0c0-58cc-49b7-b507-9ffd6b3805be)<br>

위와 같이 No 'Access-Control-Allow-Origin' headers is present 오류가 발생합니다.

이는 최신 웹 앱이나 SPA에서 흔히 보이는 오류로 혼동을 일으킬 수 있습니다.

<br>

## 💡 CORS 해결법

> **해결법 ❓**

서버와 클라이언트가 있고 같은 도메인, 예를 들어 localhost:8080 에서 실행 중이라고 가정합니다. 같은 서버에서 실행 중이라면 문제없이 요청과 응답을 전송 가능합니다. 지금까지 HTML 파일을 통해 서버에서 렌더링 하여 요청을 보낸 서버와 같은 서버에서 서비스 되었기 떄문에 문제없이 잘 작동한 것이었습니다.

그러나 클라이언트 서버가 localhost:3000이고 서버 측은 8080인 다른 도메인에서 실행 중이라면 문제가 발생하는 것입니다.<br>
도메인 간, 서버 간 그리고 이 단어 뜻대로 출처 간 리소스 공유를 막는 브라우저 보안 장치 때문에 CORS 오류가 발생하게 됩니다.

다양한 클라이언트에게 서버 데이터를 제공해야 하고 이런 클라이언트들은 API가 실행되는 서버와 다른 서버를 사용할 것입니다. 따라서 CORS 오류를 해결해야 하며 위와 같은 경우에서는 코드펜에서 실행되는 브라우저에 현재 서버에서 전송하는 응답을 받아도 된다고 알려야 합니다.

💨 **여기서 중요한 점**<br>
오류를 브라우저 측 JavaScript 코드에서 해결하려고 하지만 이것은 불가능하며 서버에서만 해결 가능합니다❗️<br>
**서버 측 코드에서 특수 헤더를 설정하면 됩니다.**

```js
app.use(bodyParser.json()); // application/json

app.use((req, res, next) => {
  res.setHeader("Access-Control-Allow-Origin", "*");
  res.setHeader(
    "Access-Control-Allow-Methods",
    "GET, POST, PUT, PATCH, DELETE"
  );
  res.setHeader("Access-Control-Allow-Headers", "Content-Type, Authorization");
  next();
});

app.use("/feed", feedRoutes);
```

bodyParser와 라우트 사용 사이에 작성 해주면 됩니다. <br>
일반적인 미들웨어를 생성해서 `setHeader`를 통해 응답에 헤더를 추가할 수 있습니다.

- **Access-Control-Allow-Origin**<br>
  이 헤더는 서버에 엑세스를 허용할 url을 작성하면 됩니다. \*을 통해 모든 클라이언트의 엑세스를 허용 가능합니다.
- **Access-Control-Allow-Methods**<br>
  이 헤더는 특정 출처에서 컨텐츠, 즉 데이터에 액세스 가능하도록 허용합니다. 특정 HTTP 메소드를 허용할 것이며 클라이언트에 어떤 메소드가 허용되는지 알립니다. 전부 허용할 필요는 없으며 외부에서 사용 가능하도록 하려는 메소드만 허용하면 됩니다.
- **Access-Control-Allow-Headers**<br>
  클라이언트가 요청에 설정할 수 있는 헤더를 지정해 줍니다. `Content-Type` 헤더와 `Authorization` 헤더는 반드시 지정해야 합니다. 그래야만 클라이언트가 헤더에 추가 인증 데이터를 포함한 요청을 보낼 수 있기 때문입니다.

위와 같은 CORS 헤더를 추가하면 <br>

![cors3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/25533bd4-6e08-45b6-9603-4833cf9ff1d8)<br>

제대로 작동하는 것을 확인할 수 있습니다.

<br>

💨 **POST 요청 보내기**<br>

```js
postButton.addEventListener("click", () => {
  fetch("http://localhost:8080/feed/post", {
    method: "POST",
    body: {
      title: "A Codepen Post",
      content: "Created via Codepen",
    },
  })
    .then((res) => res.json())
    .then((resData) => console.log(resData))
    .catch((err) => console.log(err));
});
```

코드펜에 다음과 같이 post요청 버튼을 눌렀을 때 생기는 코드를 작성합니다.<br>

![cors4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/8601baee-43ce-44db-836b-3b22b7cd4099)<br>

버튼을 클릭하면 위와 같이 콘솔됩니다. 하지만 title과 content는 뜨지 않는데요. 이것은 `Content-Type`이 현재 text로 되어 있기 때문입니다. <br>

![cors5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/038f7f61-88af-41a7-9a25-7f645a31451a)<br>

이것은 application/json이어야 하며 요청 페이로드도 json데이터가 아니기 때문에 title과 content가 뜨지 않는 것입니다. 현재 JavaScript 객체인데 이는 전송이나 처리가 불가능하기 때문에 위 코드를 바꿔 주어야 합니다.

```js
postButton.addEventListener("click", () => {
  fetch("http://localhost:8080/feed/post", {
    method: "POST",
    body: JSON.stringify({
      title: "A Codepen Post",
      content: "Created via Codepen",
    }),
    headers: {
      "Content-Type": "application/json",
    },
  })
    .then((res) => res.json())
    .then((resData) => console.log(resData))
    .catch((err) => console.log(err));
});
```

JSON.stringify는 JavaScript 객체를 json 형태로 변환해 주는 것이기 때문에 `title`과 `content`를 json 데이터로 변경 해줍니다. 그리고 헤더에 'application/json'을 추가해 주면 됩니다. <br>

![cors6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d39da994-421c-4d3c-8235-e7c3eccab111)<br>

이제 데이터가 제대로 전송 및 추출되어 생성된 게시물에 `title`과 `content`가 표시됩니다.

---

|                            REST 개념                             |
| :--------------------------------------------------------------: |
|                  데이터 중심이며 UI 논리 교환 X                  |
|                엔드 포인트를 노출시키는 노드 서버                |
|         요청 및 응답 모두에서 JSON 형식으로 데이터 교환          |
|                        HTML 페이지 반환 X                        |
| 클라이언트에서 완전히 분리되어 연결 히스토리를 공유하거나 저장 X |
|       JSON 형식으로 데이터 첨부해야 하며 헤더로 알려야 함        |
|              CORS 헤더를 설정하여 데이터 사용 알림               |

여기까지 REST API에 대해서 이해 해보았습니다.

오늘도 포스팅 읽어 주셔서 감사합니다👊
