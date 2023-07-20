---
title: "[Node] Express 프레임워크"
categories: [node]
tags: [framework, express, node, javascript]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 일곱 번째 포스팅

안녕하세요! 일곱 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

장마가 조금 사그러 들었어요 ~ 다시 비가 온다고 하고 ~ 벌써 7월의 끝이 보이기 시작했네요,,,
내 방학 왜 계속 사라지는거냐❓ 방학이 훌렁훌렁 지나가는 동안 포스팅 많이 해보기 !

자 오늘은 리액트가 아닙니다. 바로 **Node** 입니다. Node.js에 대해서 배워볼 겁니다.

Node.js의 프레임워크인 **Express.js**에 대해서 알아볼 예정입니다 !

**[붕라니] Here We Go 💛**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

## 1️⃣ Node란 ?

**Node.js는 JavaScript 런타임입니다.**

JavaScript는 브라우저에서 실행되는 언어로 로딩 후에도 페이지와 상호작용이 가능합니다. <br/>
그러므로 상호 UI를 구현하는데 필수적인 언어라고 할 수 있죠.

Node.js는 다른 버전의 JavaScript라고 생각하시면 됩니다. JS를 다른 환경에서 구현하며 코드를 서버에서 실행 가능하도록 해줍니다. <br/>
→ 이것은 서버에서 실행할 웹 애플리케이션을 구현하는데 매우 유용합니다❕

> 내장 HTTP 서버 라이브러리를 포함하고 있어 웹 서버에서 아파치 등의 별도의 소프트웨어 없이 동작하는 것이 가능하며 이를 통해 웹 서버의 동작에 있어 더 많은 통제를 가능하게 합니다.

## 2️⃣ Express란 ?

Standard JavaScript로는 서버 측 논리를 모두 작성하는 것은 정말 복잡합니다. 바디를 추출하려고 직접 데이터 이벤트와 엔드 이벤트를 살피고 버퍼도 생성하는 작업을 하나하나 다 해줘야 합니다.

데이터 취급이나 파싱을 위해 프로젝트 내부에 쉽게 연결되는 다른 패키지를 설치하도록 도와줍니다.

**다른 애플리케이션으로부터 우리 앱의 차별화하는 요소, 고유의 장점에 집중해야 합니다.**<br/>
이러한 무거운 작업들을 모두 프레임워크를 사용해서 처리합니다. 프레임워크는 일련의 도움 함수이며 규칙의 모음이 될 수 있고 클린 코드를 위한 정의 방법 등이 담겨 있습니다. <br/>

이를 **Express**가 도와줍니다. <br/>
이의 장점은 유연하며 특별한 기능을 과도하게 추가하지 않아도 되며 높은 확장성을 지닌다는 것이죠.

**Here We Go❓**

## 3️⃣ Dive Express

```jsx
npm install express
```

위를 통해 express를 설치할 수 있습니다.

```jsx
npm install
```

위에 따라 의존 패키지를 설치합니다.

`Express`에서는 코어 모듈(ex. `path`, `http`, `fs`, `os`, `https`, ...)뿐만 아니라 본인이 만든 라우트를 import 가능합니다.

`Express`를 사용하기 위해서는 import를 해줘야합니다.

```jsx
const express = require("express");
const app = express();
```

리액트와 다르게 import를 상수 선언 하듯이 해주며 `require`을 사용하여 해줍니다.

이제 `app`을 통해서 express의 도움 함수들을 사용 가능합니다.

### ⭕️ 서버 생성

원래 서버 생성을 위해서는

```jsx
const http = require("http");
const server = http.createServer(app);
server.listen(3000);
```

이렇게 여러 줄의 코드로 생성 가능합니다. 하지만 간단하게 1줄로 작성 가능합니다.

```jsx
app.listen(3000);
```

`listen`이라는 함수를 통해 3000번 포트 번호의 로컬 서버를 생성한다는 말입니다. 위보다 훨씬 간단하죠 ?

### ⭕️ 라우터 정의

서버만 키면 뭐하죠? 서버에 전달하는 요청이 있어야죠!<br/>

루트 파일인 `app.js`에서 라우터도 함께 작성하는 것보다는 서버 코드와 라우터 코드는 서로 다른 파일에 작성하는 것이 좋습니다. <br/>
그래야 서버파일이 조금이라도 더 간소화 됩니다!

다른 파일로 와서 라우터를 사용하기 위해 import해줍니다.

```jsx
const express = require("express");
const router = express.Router();
```

이 뒤에는 이제 router.을 사용하여 라우터를 사용 가능합니다.

```jsx
router.use("/", (req, res, next) => {
  res.send("<h1>hello from Express!</h1>");
});
```

`use`함수에는 많은 인자가 들어갈 수 있습니다. <br/>
하지만 우리가 직접적으로 사용할 인자만 써줘도 되겠죠?

첫 인자로는 요청을 보낼 url을 써줍니다. "/"만 써주는 것은 default라 굳이 써주지는 않아도 가능합니다.

그 후 `req`, `res`, `next`의 매개변수를 지닌 콜백함수가 나타납니다. 여기서도 사용하고 싶은 매개변수만 사용해도 무방합니다~

`setHeader()`로도 설정 가능하지만 `send()`로 쓰면 알아서 파악합니다.

그럼 헤더는 `text/html`로 파악하게 됩니다.

```jsx
router.use();
router.post();
router.get();
```

use뿐 아니라 다른 함수를 사용 가능합니다. <br/>
`post`는 post 요청만 받을 수 있도록 사용할 수 있으며 `get`은 get 요청만 받을 수 있도록 제한 가능합니다.

이제 라우터를 서버파일로 전송을 해줘야 합니다.

```jsx
module.exports = router;
```

파일 맨 마지막 줄에 이렇게 작성해주면 `export`되어 다른 파일에서 `import` 가능합니다.

```jsx
const adminRoutes = require("./routes/admin");
app.use(adminRoutes);
```

상수명은 사용자 마음대로 지정하시면 되고 require안에 있는 경로는 상대 경로로 작성하면 됩니다.<br/>
그리고 그 상수를 사용할 수 있게 됩니다.

### ⭕️ HTML 파일 연결

`file.html`

```html
<html>
  <head>
    <title>File</title>
    <link rel="stylesheet" href="/css/style.css" />
  </head>
  <body>
    <h1>Hi, I'm HTML file</h1>
  </body>
</html>
```

다음과 같이 `file.html`을 `views` 폴더 안에 생성해줍니다.

```jsx
res.send("<h1>hello from Express!</h1>");
```

> 응답으로 보내는 것이 하드코딩이고 방대하게 길게 되면 보기에도 안좋고 클린하지 않은 코드이다 보니 파일을 연결하는 방법이 있습니다❗️

```jsx
res.sendFile(path.join(__dirname, "..", "views", "file.html"));
```

위와 같이 `send()`대신 `sendFile()`을 사용하여 파일을 전송하는 함수를 사용하면 됩니다. <br/>
`path`를 사용하기 위해서는 Node.js의 코어 모듈인 path를 import하고 사용하시면 됩니다.

`join`은 마지막에 경로를 출력해줍니다. 여러 세그먼트를 이어 붙여서 경로를 구축 해줍니다. <br/>
`__dirname`은 운영체제의 절대 경로를 프로젝트 폴더로 고정해주는 전역 변수를 칭합니다.

코드를 조금이라도 간소화하고 깔끔하게 작성하고 싶으시면 `util/path.js`를 생성합니다.

```jsx
const path = require("path");

module.exports = path.dirname(require.main.filename);
```

이것은 네비게이션을 위한 헬퍼 함수를 사용한 것입니다. 최상위 main이라고 할 수 있는 서버파일인 `app.js`를 기준으로 경로를 지정하는 것입니다.

```jsx
const rootDir = require("../util/path");
router.get("/", (req, res, next) => {
  res.sendFile(path.join(rootDir, "views", "file.html"));
});
```

이렇게 사용하여 `__dirname`과 `..`의 사용을 줄여 경로를 조금이라도 간편하게 줄여보았습니다.

**이렇게 했는데 css가 적용이 안된다구요?**<br/>
네 맞습니다. css는 적용될 리가 없습니다.

### ⭕️ 정적 파일 다루는 방법

> 정적 파일이 뭐에요?

HTML에서 사용되는 .js, .css, image 파일 등을 나타내는 파일입니다. <br/>

> 어떻게 해결 해야해요?

**바로 `express.static`을 사용하면 됩니다.**<br/>
`public`폴더에 자신이 지정한 css파일을 넣어줍니다. <br/>

```jsx
app.use(express.static(path.join(__dirname, "public")));
```

서버 파일에 이렇게 작성하면 됩니다. `static`은 내장된 미들웨어를 나타냅니다. 정적 파일을 다루죠. <br/>
정적으로 서비스하기 원하는 폴더 경로를 입력하면 됩니다. 그 말은 바로 **읽기 액세스를 허용하고자 하는 폴더**를 말합니다.

이렇게 하고 서버를 실행하면 css가 입혀지며 원하는 html파일이 응답하게 될 것입니다.

### ⭕️ 404 페이지

404 상태 코드. 즉, 오류 페이지를 말합니다. <br/>
요청에 라우터로 등록되지 않은 url을 입력 했을 때 발생할 것입니다.<br/>

이 페이지도 처리해주면 더 좋겠죠?

```jsx
res.status(404).sendFile(path.join(__dirname, "views", "404.html"));
```

이렇게 `status(404)`를 통해 404코드가 떴을 때 어떻게 처리할 것인지 작성하면 됩니다. 저는 `404.html`을 만들어 처리해 주었습니다.

---

Node.js와 이에 사용하는 프레임워크인 Express에 대해 기초적인 부분에 대해서 알아보았습니다❗️<br/>

다음엔 더 깊은 내용에 대해 파고들 수 있도록 공부하도록 하겠습니다! 글 읽어주셔서 감사합니다😌🔚
