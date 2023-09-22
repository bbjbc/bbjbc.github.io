---
title: "[Node] WebSocket의 이해"
categories: [node]
tags: [express, websocket, node]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열여덟 번째 포스팅

안녕하세요! 열여덟 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

드디어 학교 축제가 끝났습니다. 축제 동안 술을 퍼마시느라 너무 고생했다 내 간아,,😩<br>
이제 다음주에 추석 끝나고부터는 찐 시험기간😱<br>
**다 들 화 이 팅💜**

오늘의 포스팅 내용은 **웹 소켓**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ 웹 소켓이란?

**웹 소켓은 실시간 웹 서비스를 구축 가능케 하는 프로토콜의 일종입니다.**

클라이언트는 브라우저고 서버는 이전에도 구축했던 Node 애플리케이션입니다. 지금까지 항상 클라이언트에서 요청을 보냈었죠? 서버에서 이 요청을 기다리고 다양한 종류의 요청을 처리하기 위해서 라우트를 설정 했습니다. <br>
**즉** 먼저 요청한 뒤 응답이 이루어졌습니다.

인터넷에 존재하는 많은 소스들이 이러한 접근법을 통해 사용합니다. 클라이언트에서 정보를 가져오며 서버에 무엇을 원한다고 알리는 것입니다. <br>

하지만 서버에서 클라이언트로 변경사항이 생겨서 보내는 것은 못할까요❓<br>
서버에서 무엇인가 변경 되어 클라이언트에게 알리고 싶으면 어떻게 하면 좋을까요❓<br>

![websocket](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/905758ce-7fc2-44b3-9d31-2239732c0c9b)
<br>

**HTTP 대신 웹 소켓을 사용하면 됩니다❗️**<br>
지금까지 HTTP라는 프로토콜로 요청을 전달하고 응답을 받았습니다. 웹 소켓은 HTTP를 토대로 구축되며 수립됩니다. HTTP는 웹 소켓으로 업그레이드하는 HTTP 핸드쉐이크를 사용합니다. 웹 소켓은 단순히 데이터 교환에만 관여합니다. <br>
즉, 이 웹 소켓은 능동적으로 관리할 필요가 없으며 요청과 응답, push data를 정의합니다.

클라이언트에서 서버로 데이터를 보내는 것도 가능하지만 서버에서 클라이언트로 데이터를 푸시할 수 있다는 것입니다. HTTP나 웹 소켓 둘 중 하나만 택하는 것이 아니며 둘 다 함께 사용합니다.<br>

![websocket2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/fcefbba5-5736-4472-a286-6f3487e1fdd2)
<br>

웹 소켓을 추가하는 경우에는 다양한 방법이 있습니다. 가장 유명한 패키지 중 하나가 **socket.io** 입니다. <br>
이것은 웹 소켓을 사용하며 해당 프로토콜에 관한 많은 편의 기능을 제공하여 클라이언트가 서버에 들어간 웹 소켓 채널을 아주 쉽게 생성하고 사용할 수 있도록 도와주는 패키지입니다.

웹 소켓 기술과 프로토콜을 사용하고 배후에서 설정을 대신 해줍니다.

---

# 2️⃣ 웹 소켓 설정하기

socket.io는 서버와 클라이언트 모두에 추가해야 합니다. 클라이언트 서버가 웹 소켓을 통해 소통하므로 해당 커뮤니케이션을 FE, BE에 구축해야 합니다.

## ✅ 서버 측(node) 설정

```js
npm install --save socket.io
```

서버 측에 node 프로젝트로 설치 됩니다.

> **사용은 어떻게 할까요❓**

서버를 시작할 때 구동되는 파일인 app.js에 설정합니다.
데이터베이스에 접속해서 서버를 시작한 뒤에 socket.io 연결을 설정합니다.

```js
mongoose
  .connect("MONGODB_URI")
  .then((result) => {
    const server = app.listen(8080);
    const io = require("socket.io")(server);
    io.on("connection", (socket) => {
      console.log("Client connected");
    });
  })
  .catch((err) => console.log(err));
```

listen 메소드는 실제로 새로운 노드 서버를 반환하여 상수에 저장 가능합니다. server 상수를 반환받은 함수 다음에 소괄호에 넣어주고 서버로 전달합니다. <br>
이 서버(8080포트)가 HTTP를 사용하므로 해당 HTTP 서버를 사용해 HTTP를 기반으로 쓰는 웹 소켓 연결을 구축하게 됩니다. 이제 io 상수를 사용하여 리스너를 구축합니다. 새로운 연결을 대기하여 새 클라이언트가 연결될 때마다 클라이언트와 연결되어 실행 할 수 있습니다.<br>
on 메소드는 현재 접속되어 있는 클라이언트로부터 메시지를 수신하기 위해 사용하는 것입니다.

즉 연결된 서버와 클라이언트 사이의 연결부에 해당하는 것입니다.

<br>

## ✅ 클라이언트 측(react) 설정

```js
npm install --save socket.io-client
```

클라이언트에서 실행될 코드라 이름이 다릅니다.

```js
import openSocket from "socket.io-client";
...
openSocket("http://localhost:8080");
```

소켓 연결하고 싶은 파일에 import 한 후 함수를 사용하여 연결합니다. 서버를 구축했던 서버 url을 정의하면 됩니다. <br>

![websocket3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/9c323023-68e4-4e3a-a778-55f6ead8be3d)<br>

socket.io를 통해 클라이언트와 백엔드 사이에 연결이 구축 되었습니다.

## ❌ CORS 오류 식별

![websocket4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/a7900f49-8351-417d-bcd1-5a3d8ce24fc1)<br>

위와 같이 CORS 오류가 식별 되었습니다. (위 'Client connected' 메시지는 이 CORS 오류를 해결한 후 나타난 사진임을 말씀 드립니다.)

웹 소켓을 사용할 때 CORS 오류가 계속 발생하는 경우, 웹 소켓 연결에 대한 CORS 설정을 다시 확인해야 합니다. 웹 소켓은 HTTP 프로토콜이 아닌 별도의 프로토콜을 사용하기 때문에 HTTP 요청과는 다른 방식으로 CORS를 처리해야 합니다.

- WebSocket Handshake
  웹 소켓 연결은 일반 HTTP 요청과 다르며, 핸드쉐이크 프로토콜을 사용하여 열립니다. 따라서 CORS 헤더가 HTTP 요청에 대한 것이 아니라 웹 소켓 핸드쉐이크에 대해 올바르게 설정 되어야 합니다.
- WebSocket Handshake에서 CORS 처리
  웹 소켓 핸드쉐이크 요청에 대한 CORS 처리는 별도의 CORS 미들웨어가 필요합니다. 서버 코드에서 웹 소켓 핸드쉐이크를 처리하는 부분에서 CORS 관련 헤더를 설정해야 합니다.

> **HTTP 프로토콜에서 CORS 처리**

```js
app.use((req, res, next) => {
  res.setHeader("Access-Control-Allow-Origin", "*");
  res.setHeader("Access-Control-Allow-Credentials", "true");
  res.setHeader(
    "Access-Control-Allow-Methods",
    "OPTIONS, GET, POST, PUT, PATCH, DELETE"
  );
  res.setHeader("Access-Control-Allow-Headers", "Content-Type, Authorization");
  next();
});
```

기존에는 서버 측에서 이렇게 CORS에 대해 처리했는데 이것은 HTTP 프로토콜에서 CORS를 처리하는 방식입니다.

> **웹 소켓 프로토콜에서 CORS 처리**

express에서는 CORS 문제를 해결하기 위해 미들웨어를 사용할 수 있습니다. 'cors' 패키지를 설치하고

```js
app.use(cors());
```

이렇게 미들웨어를 적용 후 아래와 같은 코드를 작성하면 됩니다.

```js
const server = app.listen(8080);
const io = require("socket.io")(server, {
  cors: {
    origin: "http://localhost:3000", // 클라이언트 주소
    methods: ["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"],
  },
});
```

socket.io 서버 설정에서 CORS 관련 옵션을 추가하면 됩니다. 이렇게 하면 CORS 오류를 해결할 수 있습니다. <br>
HTTP와 웹 소켓은 서로 다른 프로토콜이며, CORS를 처리하는 방식도 조금 다르기 때문에 각각의 설정이 필요합니다.

---

# 3️⃣ 추가 POST 요청 동기화

io 객체의 재사용을 위해서 socket.js를 생성한 후 이를 import하여 사용할 겁니다.

```js
let io;

module.exports = {
  init: (httpServer, corsOptions) => {
    io = require("socket.io")(httpServer, {
      cors: corsOptions,
    });
    return io;
  },
  getIO: () => {
    if (!io) {
      throw new Error("Socket.io not initialized!");
    }
    return io;
  },
};
```

이렇게 하면 다수의 파일에 걸쳐 io 연결을 공유할 방법을 확보하였습니다.

```js
const io = require("../socket");

exports.createPost = async (req, res, next) => {
  ...
  io.getIO().emit("posts", { action: "create"}); // emit은 모든 사용자에게 발신, broadcast는 이 요청이 발신된 사용자 외의 모든 사용자에게 발신함
  ...
}
```

이제 io 객체를 app.js에서 설정한 연결을 가져오기 위해 getIO를 호출합니다. <br>
클라이언트가 서버에게 메시지를 전송할 때는 `on()`메소드를 사용했었는데 서버가 클라이언트에게 메시지를 전송할 때는 `emit()`메소드를 사용할 수 있습니다.

> `io.emit`<br>
> 서버가 현재 접속해있는 모든 클라이언트에게 이벤트 전달<br>
>
> ```js
> io.emit("event_name", msg);
> ```

msg 부분에는 데이터를 포함한 객체를 넣을 수 있습니다. 무슨 일이 일어났는지 클라이언트에게 알리기 위해 action 키를 정의합니다.

이제 클라이언트 측 코드를 봅시다. 이벤트에 반응할 수 있도록 해야겠죠?

```js
const socket = openSocket("http://localhost:8080");
socket.on("posts", (data) => {
  if (data.action === "create") {
    ...
  }
});
```

이 애플리케이션의 경우에서 action이 create라면 포스트가 추가되는식으로 설정하면 되겠죠.

이렇게 하면 다른 클라이언트에서도 포스팅이 추가 되는 방식으로 보여질 것입니다. 이것이 socket.io에서 두 클라이언트에 연결을 구축하여 우리가 프론트엔드에 추가한 코드를 통해 업데이트 됩니다.

---

자 이번 포스팅은 웹 소켓이 무엇이며 설정 방법, socket.io 사용법 그리고 서버에서 클라이언트로 정보를 푸시할 때 사용하는 메서드 등에서 알아보았습니다. <br>
웹 소켓은 HTTP에 기반한 것으로 HTTP 핸드쉐이크가 설정되므로 Node.js나 Express 같은 패키지로 설정된 HTTP 서버를 실행하고 있어야 합니다.

오늘도 포스팅 읽어 주셔서 감사합니다😸
