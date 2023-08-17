---
title: "[Node] Express Cookie & Session"
categories: [cookie, node]
tags: [express, node, cookie, session]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열네 번째 포스팅

안녕하세요! 열네 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘은 광복절입니다! 78주년을 맞는 광복절입니다. <br>
조국의 독립을 위해 희생하고 헌신하신 순국선열과 애국지사들, 그리고 유가족분들 모두 깊은 감사와 경의를 표합니다🙏
<br>

![태극기](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/116ec416-48a2-4c42-91e3-68c1398cba43)

<br>
오늘의 포스팅 내용은 **Cookie & Session**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ Cookie란?

우리는 ejs 템플릿 엔진을 통해 뷰를 렌더링합니다. 어떤 사용자가 로그인하면 로그인 정보를 어딘가에 저장해서 새로고침이 되어 새 요청을 보내더라도 같은 사용자가 로그인된 상태라는 정보를 가져야 합니다. <br>

요청에 대한 응답과 함께 쿠키를 보내게 됩니다. 리다이렉트할 뷰 이외에도 쿠키를 포함하는 겁니다. 쿠키는 사용자가 인증을 마쳤다는 정보를 저장하고 있습니다. 이 정보를 프론트엔드 환경에 저장하여 이어지는 요청이 쿠키를 포함해 쿠키에 저장된 데이터를 보내게 됩니다.

## 🔐 Cookie 설정하기

[`controllers/auth.js`]

```js
exports.getLogin = (req, res, next) => {
  res.render("auth/login", {
    path: "/login",
    pageTitle: "Login",
    isAuthenticated: isLoggedIn,
  });
};

exports.postLogin = (req, res, next) => {
  req.isLoggedIn = true;
  res.redirect("/");
};
```

위와 같이 로그인과 관련된 컨트롤러를 생성합니다.

```jsx
isAuthenticated: isLoggedIn;
```

위는 뷰에서 렌더링이 될 때마다 로그인의 데이터가 유효하다고 가정하기 위해 true로 설정해 놓은 것뿐입니다.<br>
사용자가 인증 되었다는 정보를 저장하기 위해 요청 객체인 `isLoggedIn`에 저장합니다. 모든 get컨트롤러에 사용자 인증 여부를 전달해야 하기 때문에 정보 저장 필드에 위 정보를 추가해 줍니다. <br>

**하지만❗️**<br>

> `isLoggedIn`에 정보를 저장하고 있어도 Login을 하면 요청에 저장되고 이 정보를 다른 라우트에 대한 요청에 사용해 내비게이션에 쓰이는 프론트엔드 필드인 `isAuthenticated`에 전달합니다. 그러나, 모든 컨트롤러에서 render 함수마다 `isAuthenticated`필드를 프론트엔드로 전달하고 있습니다. 요청에 `isLoggedIn` 정보를 업데이트 한다 해도 응답을 보낼 때 리다이렉트를 통해 응답을 보내는데 이 때 해당 요청이 끝나게 된다는 것입니다.
>
> 요청을 받고 응답을 보내면 그대로 끝이며 데이터는 손실되기 때문에 다른 페이지를 방문하면 그 페이지에서 리다이렉트 되어 새로운 페이지를 요청하기 위한 요청은 새로운 요청입니다. <br> 즉, **리다이렉트 되면 새로운 요청을 만들게 됩니다.**

`isLoggedIn`에 저장된 데이터는 같은 요청을 다루는 동안에만 유효한 것입니다.

<br>

## 🔐 Solution?

쿠키를 사용하면 특정 사용자의 브라우저에 데이터를 저장해서 데이터가 해당 사용자에 맞춤화되어 다른 사용자에 영향을 끼치지 않습니다.

```js
exports.getLogin = (req, res, next) => {
  const isLoggedIn = req.get("Cookie").split("=")[1];
  res.render("auth/login", {
    path: "/login",
    pageTitle: "Login",
    isAuthenticated: isLoggedIn,
  });
};

exports.postLogin = (req, res, next) => {
  res.setHeader("Set-Cookie", "loggedIn=true");
  res.redirect("/");
};
```

요청의 헤더를 설정하기 위해 `setHeader()`를 통해 해결할 수 있습니다. `Set-Cookie`는 쿠키를 설정하는 예약명입니다.

```js
res.setHeader("쿠키 예약명", "쿠키값;옵션객체");
```

쿠키 값은 키-값 쌍으로 이름과 값을 지정할 수 있습니다. <br>

![cookie1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/709f5475-437c-442c-898d-60614a90e801)

<br>

위처럼 설정한대로 쿠키가 나오는 것을 확인할 수 있습니다. (Chrome → Application → Cookies)
<br>

![cookie2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/6ec8609a-fd44-4d5a-b119-ffe4e50043fc)

<br>

또한 다른 페이지를 렌더링 하고 (Network → Request Headers → Cookie)를 보면 위와 같이 나타나는 것을 확인할 수 있습니다. 쿠키가 보내진 것이죠.

```js
const isLoggedIn = req.get("Cookie").split("=")[1];
```

쿠키의 true값만 추출하여 `isAuthenticated`에 넣어주기 위해서는 위와 같이 추출하여 사용합니다. <br>

이제 다른 페이지로 이동하다가 로그인 페이지에서 쿠키를 추출하면 모든 요청에 대한 쿠키가 보내지게 됩니다.

**잠깐만❗️**<br>

```js
const isLoggedIn = req.get("Cookie").split("=")[1] === "true";
```

이렇게 하면 개발자 도구를 열어 false로 바꾸어 조작을 하려 해도 동일한지 확인을 거치니 로그인 조작을 할 수 없게 됩니다. <br>
**쿠키 값을 조작해서 로그인하는 것을 허용해서는 안됩니다.**

<br>

## 🔐 옵션 객체

옵션 객체를 통해서 더 많은 것들을 설정 가능합니다.

`res.setHeader()` 함수는 HTTP 응답 헤더에 값을 설정하는 데 사용되는 Node.js의 메소드 입니다.

```js
res.setHeader("쿠키 예약명", "쿠키값;옵션객체");
```

위에서 보았듯 이런식으로 옵션 객체를 함께 전달 가능합니다.

- **Domain(도메인)**
  - 쿠키가 전송될 수 있는 도메인을 지정할 수 있습니다.
- **Path(경로)**
  - 쿠키가 어떤 경로에서 유효한지를 지정합니다.
- **Expires(만료일)**
  - 쿠키의 유효 기간을 설정합니다. Date 객체 형태로 지정되며, 쿠키는 해당 날짜 이후에 만료됩니다.
- **Max-Age(최대 유효 기간)**
  - 쿠키의 최대 지속 시간을 초 단위로 설정할 수 있습니다. "쿠키 생성 시점 + Max-Age" 이후에 쿠키가 만료됩니다.
- **Secure(보안)**
  - 이 값이 true인 경우, 쿠키는 HTTPS 연결을 통해서만 전송됩니다.
- **HttpOnly(HTTP 전용)**
  - 이 값이 true인 경우, JS를 통한 접근을 차단하고 HTTP 요청을 통해서만 쿠키에 접근 가능합니다.
- **SameSite(동일 사이트 제한)**
  - 쿠키의 SameSite 속성을 설정합니다. 이것은 CSRF(Cross-Site Request Forgery) 공격을 방지하기 위해 사용되며 'Strict', 'Lax', 'None' 중 하나의 값으로 설정 가능합니다.

```js
const options = {
  domain: "example.com",
  path: "/",
  expires: new Date(Date.now() + 24 * 60 * 60 * 1000), // 1 day
  secure: true,
  httpOnly: true,
  sameSite: "Strict",
};

res.setHeader(
  "Set-Cookie",
  "myCookie=myValue; " +
    Object.entries(options)
      .map(([key, value]) => `${key}=${value}`)
      .join("; ")
);
```

위와 같이 옵션 객체를 포함하여 원하는 쿠키 옵션을 설정할 수 있습니다.

---

# 2️⃣ Session이란?

뷰는 노드의 코드가 존재하는 서버와 상호작용하며 로그인을 진행합니다. 사용자 인증 정보를 프론트엔드(뷰)에 저장하는 대신 이젠 **세션**이라고 하는 백엔드에 저장합니다.

이것은 동일한 사용자 전용이며 다른 사용자가 데이터를 열람하거나 역할을 가장할 수 없으며 본인만 인증될 수 있습니다. 이를 위해 이제 서버에 저장해야 합니다.

클라이언트는 서버에게 자신이 소속된 세션을 알려야 하는데 세션은 메모리나 DB에 저장된 입력값에 불과합니다. 그래서 세션의 ID를 저장하는 쿠키를 활용합니다. 저장된 쿠키 값이 DB의 특정 ID와 관련되어 있다는 것을 서버 측에서만 확인할 수 있도록 암호화된 방식으로 저장하기 때문에 보안성이 높은 방법으로 쿠키에 안전한 값이 저장되며 다른 세션으로 간주할 수 없습니다.

**세션은 서버 측에 저장되며 쿠키는 클라이언트 측에 저장되는 것입니다❗️**

## 🔑 Session 패키지 설치

```jsx
npm install --save express-session
```

위와 같이 패키지를 설치해 줍니다.

<br>

## 🔑 Session 미들웨어 설정

```jsx
const session = require("express-session");

app.use(
  session({ secret: "my secret", resave: false, saveUninitialized: false })
);
```

패키지 설치 후 위와 같이 session 선언 하고 미들웨어 사용을 할 수 있습니다. <br>
여기서도 옵션 객체가 사용되며 세션의 동작과 설정을 제어하는 데 사용할 수 있습니다.

- **secret(비밀 키)**
  - 세션 데이터를 암호화하기 위한 비밀 키입니다. 이 값은 세션 데이터를 보호하기 위해 사용되며, 유출되면 안됩니다.
- **resave(재저장 여부)**
  - 세션 데이터가 변경되지 않았더라도 세션 저장소에 다시 저장할지 여부를 결정합니다. 보통 'false'로 설정하여 불필요한 세션 저장소의 접근을 방지합니다.
- **saveUninitialized(미초기화 세션 저장 여부)**
  - 초기화되지 않은 세션을 저장할지 여부를 결정합니다. 보통 'false'로 설정하여 세션을 사용하기 전에 반드시 초기화되도록 합니다.
- **cookie(쿠키 설정)**
  - 세션 쿠키와 관련된 설정을 지정하는 객체입니다. 위 쿠키의 옵션 객체에서 사용 가능한 것들을 포함합니다.
- **name(세션 쿠키 이름)**
  - 생성되는 세션 쿠키의 이름을 지정합니다.
- **store(세션 저장소)**
  - 세션 데이터의 영구 저장을 위한 저장소를 지정할 수 있습니다. 기본적으로 메모리 저장소가 사용되지만, 데이터베이스나 외부 저장소를 사용 가능 합니다.
- **proxy(프록시 환경에서의 세션 신뢰)**
  - 'proxy'옵션을 'true'로 설정하면 프록시 환경에서 신뢰할 수 있는 프록시 설정이 있는 경우 프록시 헤더를 신뢰하고 세션 데이터의 IP 주소를 업데이트 합니다.
- **rolling(롤링 세션)**
  - 'rolling' 옵션을 'true'로 설정하면 매 요청마다 세션 쿠키의 만료 시간이 갱신되며 사용자 활동이 지속되는 동안 세션은 계속 유지됩니다.

<br>

## 🔑 Session 미들웨어 사용

```js
exports.postLogin = (req, res, next) => {
  //   res.setHeader("Set-Cookie", "loggedIn=true");
  req.session.isLoggedIn = true;
  res.redirect("/");
};
```

기존에 있던 쿠키를 지우고 session을 활용하기 위해 위와 같이 설정합니다. 원하는 이름을 사용하면 됩니다. <br>

![cookie3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/8907680d-cc9c-4e00-89d9-7992f3640fa5)

<br>

이제 다시 확인해보면 새로운 쿠키가 위와 같이 보입니다. session id를 뜻하는 connect.sid 쿠키입니다.

```js
exports.getLogin = (req, res, next) => {
  //   const isLoggedIn = req.get("Cookie").split("=")[1];
  console.log(req.session.isLoggedIn);
  res.render("auth/login", {
    path: "/login",
    pageTitle: "Login",
    isAuthenticated: true,
  });
};
```

위와 같이 해주면 `true`가 로깅될 것입니다. 세션 내부에 `isLoggedIn`이 저장되었다는 증거입니다. 다른 페이지에 들어가도 여전히 서버 측 세션에 저장하고 메모리에 저장하기 때문에 오류가 발생하지 않습니다. <br>

![cookie4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/2a2abf13-d246-490d-86c0-e18d5b9b0de6)

<br>

위는 크롬이 아닌 엣지에서 들어간 화면입니다. 이렇게 하면 `undefined`가 로깅 됩니다. 브라우저가 다르므로 사용자가 달라진 셈이라 엣지 사용자는 서버에 활성화된 세션이 없는 것입니다.

사용자를 식별하기 위해 쿠키가 필요하지만 민감한 정보는 서버에 저장되어 인증에 있어 아주 중요한 요소입니다.

<br>

## 🔑 MongoDb를 통한 세션 저장

세션은 메모리에 저장되는데 메모리가 infinite하지 않는다는 것입니다. 모든 정보를 메모리에 저장한다면 메모리가 빠르게 오버플로우 되기 때문이죠. 따라서 MongoDb를 통해 저장해보도록 하겠습니다.

### ⛺️ MongoDb-session 설치

```jsx
npm install --save connect-mongodb-session
```

위와 같이 Express 세션 패키지가 데이터를 db에 저장하게 사용할 수 있습니다.

<br>

### ⛺️ MongoDb-session 작업

```js
const session = require("express-session");
const MongoDBStore = require("connect-mongodb-session")(session);
```

`express-session`에서 임포트하는 세션 객체는 함수로 전달되며 해당 함수 호출의 결과는 `MongoDBStore`에 저장됩니다.<br>

```js
const MONGODB_URI = "YOUR_URI";

const store = new MongoDBStore({
  uri: MONGODB_URI,
  collection: "sessions",
});
```

어느 데이터베이스 서버에 저장할지 정하기 위해 기존에 있던 URI를 사용합니다.

```js
app.use(
  session({
    secret: "my secret",
    resave: false,
    saveUninitialized: false,
    store: store,
  })
);
```

세션 초기화 부분에 store옵션을 설정하여 store 객체를 넣어줍니다.<br>

![session](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/5dd7208f-ec9d-4592-ac4f-2f7c15fbe8fa)

<br>
이제 Compass를 통해 확인하면 위와 같이 `sessions` 컬렉션에 있는 쿠키에 관한 정보들을 확인할 수 있습니다.

**세션은 응답을 매번 전달할 때마다 잃지 않으며 타인에게 보여서는 안되는 사용자 관련 데이터를 대상으로 사용합니다❗️**

---

# 3️⃣ 쿠키 삭제

[`controllers/auth.js`]

```jsx
exports.postLogout = (req, res, next) => {
  req.session.destroy((err) => {
    console.log(err);
    res.redirect("/");
  });
};
```

[`routes/auth.js`]

```js
const express = require("express");

const authController = require("../controllers/auth");

const router = express.Router();

router.post("/logout", authController.postLogout);
```

위와 같이 로그아웃을 통한 쿠키를 삭제 하기 위해 설정해줍니다. 세션 객체에 접근하여 `destroy`를 호출합니다. 이것은 우리가 사용하고 있는 세션 패키지가 제공하는 메소드입니다. `req.session`은 우리가 해당 세션을 파괴했으므로 더 이상 사용할 수 없게 됩니다.<br>

[로그인]
![cookie5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/66161af7-9983-4452-83b3-154c1f9d1713)

<br>
[로그아웃 후 재로그인]
![cookie6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/c5c31510-7db9-4773-9786-afdd02f8bf8a)

<br>
로그인을 하면 세션 쿠키가 설정되고 로그아웃을 해도 세션 쿠키가 아직 존재합니다! 하지만 대응하는 세션이 발견되지 않기 때문에 문제가 되지 않습니다. 다시 로그인하면 갱신 세션 쿠키가 덮어씌우기 때문에 문제가 발생하지 않습니다.

---

이번 포스팅에서는 쿠키와 세션에 대해 알아보았습니다.

|                       쿠키                       |                        세션                        |
| :----------------------------------------------: | :------------------------------------------------: |
|          클라이언트 측 데이터 저장 우수          |                서버에 데이터를 저장                |
| 민감한 데이터 저장 X (보여지고 조작 가능성 높음) |        요청을 통해 남는 데이터 저장에 우수         |
|               세션과 어우러져 작업               | 서버에서 세션을 저장하여 다른 저장소에서 사용 가능 |
|    쿠키에 정보가 있어 서버 요청 시 속도 빠름     |       서버에 정보가 있기에 비교적 느린 속도        |

포스팅 읽어 주셔서 감사합니다💛
