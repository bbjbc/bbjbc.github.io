---
title: "[Node] Express Authentication"
categories: [authentication, node]
tags: [express, node, authentication]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열다섯 번째 포스팅

안녕하세요! 열다섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

개강이 진짜 수욱 다가왔어요. 아직 수강신청 마무리를 못했는데 다음주에 꼭 다 잡고 싶어요**!!** <br>
다들 개강해도 열심히 공부하며 살아갑시다❗️ **VAMOS**👊

오늘의 포스팅 내용은 **Authentication**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ Authentication이란?

우리가 다루는 애플리케이션에서 제품과 로그인 사용자를 연결하기 위해서는 인증 절차가 필요합니다. <br>
로그인하지 않은 익명의 웹 방문자와 로그인 한 사용자를 구별할 수 있도록 해야합니다. (shop 웹사이트 같은 경우에서는 장바구니 및 구매 등과 같은 로그인한 회원을 대상으로만 가능하기 때문)
따라서 알맞은 워크플로우와 뷰 그리고 백엔드 논리로 페이지에 방문하는 사람들이 가입하고 로그인할 수 있도록 해야합니다.

---

# 2️⃣ 인증 플로우

## 🔒 인증 구현 절차

사용자가 있고 백엔드로 서버 측 코드와 데이터베이스가 있습니다. 사용자는 서버에 로그인 요청을 보내면 서버에서 로그인 정보가 유효한지 데이터베이스에서 정보를 가진 사용자가 존재하는지 확인합니다.

그렇다면? 사용자에 대한 세션을 생성하여 세션이 사용자를 인식할 겁니다. 세션이 없다면 로그아웃 될 것이고요. 요청이 단독적으로 실행되어 서로 알 수 없기 때문에 세션을 통해 요청을 연결합니다. 그러므로 세션을 생성하는 것이죠.

상태 코드 200이라는 OK 응답과 함께 반환되어 세션에 속한 쿠키를 클라이언트에 저장하면 세션이 구축됩니다. 모든 요청과 쿠키가 함께 발송되는데 서버에서 쿠키와 세션을 연결해 세션에서 사용자의 로그인 여부를 확인하여 이제 사용자는 제한된 라우트도 방문 가능합니다.

<br>

## 🔒 인증 플로우 구현

![authentication2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/d063e144-22e7-41b3-adf3-2df05c05a724)
<br>

위와 같은 signup 폼이 존재합니다. 여기서 회원가입을 통한 회원 생성 과정을 보여드리겠습니다.

```js
exports.postSignup = (req, res, next) => {
  const email = req.body.email;
  const password = req.body.password;
  const confirmPassword = req.body.confirmPassword;
  User.findOne({ email: email })
    .then((userDoc) => {
      if (userDoc) {
        return res.redirect("/signup");
      }
      const user = new User({
        email: email,
        password: password,
        cart: { items: [] },
      });
      return user.save();
    })
    .then((result) => {
      res.redirect("/login");
    })
    .catch((err) => {
      console.log(err);
    });
};
```

위와 같이 `postSingup` 컨트롤러를 통해 회원가입을 완료할 수 있습니다. 정확한 기능은 아니지만 인증 플로우를 위해서 단순하게 보입니다. <br>
User모델을 통해 같은 이메일을 가진 사용자를 생성하지 않기 위해 한 가지의 이메일만 찾아도 됩니다. 동일한 이메일을 가진다면 일단 리다이렉트만 띄웁니다. <br>
원래 User모델에는 name이 있었는데 일단 signup폼을 충족하기 위해서 name을 빼고 새로운 사용자를 생성해 줍니다.

```js
User.findOne().then((user) => {
  if (!user) {
    const user = new User({
      name: "Chan",
      email: "chan@test.com",
      cart: {
        items: [],
      },
    });
    user.save();
  }
});
```

원래 `app.js`에서 더미 사용자를 하드코딩으로 만들어 줬었는데 이제 인증을 통한 사용자를 생성할 수 있으니 위는 삭제해줍니다. <br>

![authentication3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/2aa55560-9851-4367-b56c-b758b2b3bb85)

<br>
위와 같이 compass에서도 회원가입한 이메일과 패스워드, 카트가 생성된 것을 확인할 수 있습니다.

하지만 위는 치명적인 보안상의 문제점이 존재합니다. 바로 패스워드가 일반적인 텍스트로 보이기 때문이죠. 만약에라도 데이터베이스가 손상되거나 누군가가 보게 된다면 위험하기 때문에 암호화를 해야 합니다.
따라서 패스워드를 단방향으로 해시화하고 데이터베이스에도 이메일만 보이게 해야합니다.

### 🔑 패스워드 암호화

#### 💡 암호화 패키지 설치

```js
npm install --save bcryptjs
```

이것은 암호화를 돕는 패키지로 비밀번호를 암호화 하기 위함입니다.
<br>

#### 💡 패키지 적용

```js
const bcrypt = require("bcryptjs");

User.findOne({ email: email })
  .then((userDoc) => {
    if (userDoc) {
      return res.redirect("/signup");
    }
    return bcrypt.hash(password, 12).then((hashedPassword) => {
      const user = new User({
        email: email,
        password: hashedPassword,
        cart: { items: [] },
      });
      return user.save();
    });
  })
  .then((result) => {
    res.redirect("/login");
  })
  .catch((err) => {
    console.log(err);
  });
```

`hash()`메소드는 해시화 하고 싶은 문자열을 첫 번째 값으로 가집니다. 두 번째 인수로는 sort 값으로 몇 차례의 해싱을 적용할 것인지 지정합니다. 높으면 오래 걸리지만 더 안전합니다. 위처럼 12 정도면 높은 보안 성능으로 간주됩니다.

사용자 생성은 해싱된 패스워드를 토대로 생성해줍니다.
<br>

![authentication4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/79304c9c-6f65-41a0-9c90-833d63c2fdc6)

<br>
위와 같이 패스워드가 해시화 되어 나타나고 패스워드를 재구성하거나 해독할 수 없을 것입니다. 이메일의 경우 해독할 수 없기에 암호화를 하지 않는 것입니다.

---

# 3️⃣ 로그인

기존 `postLogin` 라우트는 더미 사용자를 사용하였고 이번에는 email정보를 통해 회원인지 확인 후 로그인을 성공시킬 것이며 세션을 생성합니다.

```js
exports.postLogin = (req, res, next) => {
  const email = req.body.email;
  const password = req.body.password;
  User.findOne({ email: email })
    .then((user) => {
      if (!user) {
        return res.redirect("/login");
      }
      bcrypt
        .compare(password, user.password)
        .then((doMatch) => {
          if (doMatch) {
            req.session.isLoggedIn = true;
            req.session.user = user;
            return req.session.save((err) => {
              console.log(err);
              res.redirect("/");
            });
          }
          res.redirect("/login");
        })
        .catch((err) => {
          console.log(err);
          res.redirect("/login");
        });
    })
    .catch((err) => {
      console.log(err);
    });
};
```

`findOne()` 메소드를 사용하여 특정 이메일을 가진 사용자를 찾습니다. 사용자가 없다면 리다이렉트를 해줍니다.

패스워드는 hash 형식으로 저장되어 있으므로 bcrypt 패키지를 사용합니다. 해시화된 패스워드는 되돌릴 수 없습니다. 사용자가 입력한 패스워드를 bcrypt로 보내서 해시된 값과 비교하도록 합니다. 패스워드가 올바른지 판단하기 위해 해시화된 패스워드와 입력한 패스워드를 비교해줍니다. 비교를 위해 `compare()` 메소드를 사용합니다. 이것은 프로미스를 반환하며 then 블록에는 성공했는지 검증을 합니다.

`doMatch`가 참이면 입력한 패스워드가 일치하므로 세션을 생성하며 홈화면으로 리다이렉트 됩니다. 일치하지 않는다면 login 페이지로 리다이렉트 되겠죠.<br>

![authentication5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/a12d4862-a6d5-4c65-93f9-4f6d5e98bc74)

<br>
올바르지 않은 이메일과 비밀번호를 사용하면 세션이 생성되지 않았습니다. <br>

---

# 4️⃣ 라우트 보호

로그인 시 사용할 수 있는 메뉴가 있고 회원이 아닐 시 사용할 수 있는 메뉴가 있습니다. 하지만 현재 주소창에 로그인 시에만 사용 가능한 메뉴를 검색하면 로그인이 되어 있지도 않는데 들어가지게 됩니다. 이 문제를 해결하고자 라우트를 보호해야 합니다.

[`middleware/is-auth.js`]

```js
module.exports = (req, res, next) => {
  if (!req.session.isLoggedIn) {
    return res.redirect("/login");
  }
  next();
};
```

새로운 미들웨어 폴더에 로그인 세션이 존재하는지 검사합니다. 이제 이 미들웨어를 사용하여 라우트에 대해 핸들러를 원하는만큼 추가하면 됩니다.

```js
const isAuth = require("../middleware/is-auth");
router.get("/add-product", isAuth, adminController.getAddProduct);
```

위와 같이 import 해준 후 인수에 추가할 수 있습니다. 파싱되는 것은 좌에서 우로 진행되기 때문에 로그인 세션이 있다면 `is-Auth.js`의 `next()`를 통해 다음 핸들러가 실행되지만 없다면 실행되지 않으며 리다이렉트 되겠죠?

이렇게 라우트 보호 기능을 갖춤으로써 **서버 측 세션을 활용해 인증 상태를 검사**할 수 있게 되었습니다. 이것은 사용자가 조작할 수 있는 방법이 없으므로 일부 라우트나 메소드는 로그인된 사용자들만 사용 가능합니다.

---

# 5️⃣ CSRF 공격

> **CSRF** - **C**ross-**S**ite **R**equest **F**orgery<br> **사이트 간 요청 위조**를 뜻합니다.

이것은 세션을 악용하고 애플리케이션 사용자를 속여 악성코드를 실행하도록 하는 특수한 공격 방법 및 접근법을 말합니다.

> **원리 ❗️**<br>
> CSRF 공격에서는 사용자를 속여서 가짜 사이트로 이동하게 합니다. 이메일로 링크를 보내는 방법 등으로 이루어집니다. 해당 사이트는 원본 페이지처럼 보이지만 다른 페이지인 것이죠. 이 사이트엔 실제 페이지로 연결된 링크도 있고 거기로 요청을 보내게 됩니다. POST 요청을 보내는 폼을 목적지가 다른 곳으로 보내도록 하는 필드를 추가할 수 있습니다. 사용자는 당연히 알아채기 어렵습니다.
>
> **이 방법이 왜 통할까요❓**<br>
> 해당 유저에 대한 유효한 세션이 있으므로 사이트 서버에 무엇을 보내면 사용자에 대해 세션이 활용되는데 이 부분은 사용자에게 보이지 않지만 사이트 서버가 사용되므로 전송에 유효한 세션이 활용되고 요청이 받아들여지게 됩니다. 이는 세션을 훔치는 공격 방법으로 로그인 사실을 악용하여 알아채지 못하는 요청을 보냅니다.

**어떻게 방어할까요❓**<br>

우리의 애플리케이션이 렌더링한 뷰로 작업할 때에에만 우리의 세션을 사용 가능하도록 하면 됩니다. 이렇게 하면 실제 페이지와 비슷해 보이지만 가짜인 페이지에서 세션을 사용할 수 없습니다. 이러한 기능을 추가하기 위해서 **CSRF 토큰**을 사용합니다.

## 🔦 CSRF 토큰 사용

### 💸 CSRF 패키지 설치

```js
npm install --save csurf
```

이는 CSRF 토큰이라는 것을 생성하게 해주는 Express 패키지입니다.

> 기본적으로 토큰, 즉 문자열 값으로 백엔드에서 실행되어 뭔가를 주문하는 등의 보호가 필요한 민감한 작업을 수행하고 사용자의 상태를 변경하는 모든 요청에 대해서 이것을 폼이나 페이지에 내장시킬 수 있습니다. 이러한 토큰을 뷰와 서버에 포함시키면 이 패키지가 들어오는 요청이 유효한 토큰을 가지고 있는지 검사하게 됩니다.
>
> 가짜 사이트가 백엔드로 요청을 보내서 이론적으로 세션을 사용할 수 있지만 이러한 요청에는 토큰이 빠져있고 토큰은 무작위 해시 값이고 하나의 값만이 유효하므로 토큰을 추측할 수도 없습니다. 또한 렌더링 할 때마다 새로운 토큰이 생성되므로 가로챌 수도 없습니다!

<br>

### 💸 미들웨어를 통한 토큰과 인증 추가

이런 토큰 값과 인증 상태를 렌더링할 모든 페이지에 추가하기 위해서 `express.js`의 도움을 받아야 합니다.<br>
미들웨어를 통해 Express가 제공하는 특수 기능을 사용합니다. `response`에서 `locals`라는 특수한 필드에 접근 가능한데 이것은 뷰에 입력할 로컬 변수를 설정할 수 있도록 해줍니다.

```js
app.use((req, res, next) => {
  res.locals.isAuthenticated = req.session.isLoggedIn;
  res.locals.csrfToken = req.csrfToken();
  next();
});
```

위와 같이 미들웨어를 추가합니다. 로컬 변수를 통해 인증 정보와 토큰에 접근하여 모든 요청에 대해 렌더링되는 뷰에서 위 두 필드가 설정됩니다.

<br>

### 💸 CSRF 패키지 사용

```js
const csrf = require("csurf");

const csrfProtection = csrf();

app.use(csrfProtection);
```

csrf함수로 실행하여 `csrfProtection`을 초기화합니다. csrf함수 안에는 객체를 입력할 수 있으며 기본 값은 세션입니다. 우리는 세션을 이용할 것이니 디폴트 값입니다. 이렇게 하면 `csrfProtection`을 사용할 수 있게 되었습니다.

실제로 적용하기 위해서는 뷰에 추가해야 할 것이 있습니다. 이 패키지는 일반적으로 POST 요청을 통해 데이터를 변경하는 get 이외의 모든 요청에 대해 뷰에 csrf 토큰이 있는지 확인합니다. 그러므로 뷰에서 토큰에 접근을 해야합니다.

```js
<input type="hidden" name="_csrf" value="<%= csrfToken %>" />
```

POST 폼이 존재하는 버튼 코드 근처에 위와 같은 코드를 넣어줍니다. 실제 입력값이 렌더링 되지 않도록 hidden 상태로 input을 추가하면 이 값은 이제 ejs의 도움을 받아 출력되는 value가 됩니다. 이것이 우리의 csrf토큰이 됩니다. 그리고 name을 가진 입력값이므로 추가한 패키지가 name을 찾으려 할 것이기 때문에 name도 추가해야 합니다. <br>

![authentication6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/650dc1af-0a78-4275-883f-5bf8a1028344)

<br>
위와 같이 value에 토큰 해시값이 생긴 것을 볼 수 있습니다.

---

자 이번 포스팅에서는 로그인 및 회원가입에서의 인증과 관련한 절차와 보호 방법에 대해서 알아보았습니다.

|                           인증                           |                                     보안                                      |
| :------------------------------------------------------: | :---------------------------------------------------------------------------: |
|           페이지 방문자가 모든 것을 볼 수 없음           |                    패스워드는 해시된 형태로 저장되어야 함                     |
|      서버 측에서 인증을 수행하고 세션 기반으로 구축      | CSRF 공격은 실질적 문제이므로 구축하는 모든 애플리케이션에 CSRF 보호가 필요함 |
| 컨트롤러 작업 전에 로그인 상태를 확인하여 경로 보호 가능 |           더 나은 사용자 환경을 위해 플래시 메시지를 통해 표현 가능           |

오늘도 포스팅 읽어 주셔서 감사합니다✨
