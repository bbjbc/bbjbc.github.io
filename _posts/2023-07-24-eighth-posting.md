---
title: "[Node] Express 템플릿 엔진 EJS"
categories: [node]
tags: [ejs, express, node, template engine]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 여덟 번째 포스팅

안녕하세요! 여덟 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

어제 알바 회식을 다녀왔어요,, 정말 재밌었지만 술을 엄청나게 마셔서 다음날이 정말 힘드네요😱
견뎌내고 또 포스팅을 해봐야죠 ㅎㅅㅎ😻

오늘의 포스팅 내용은 **템플릿 엔진**입니다. 템플릿 엔진의 종류는 여러 가지가 있습니다. <br/>
그 중에서도 **EJS**라는 녀석에 대해 알아봅시다❗️

**[붕라니] Here We Go 💛**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

## 1️⃣ 템플릿 엔진이란?

### 💥 템플릿 엔진(Template Engine)

저번 포스팅의 마지막 파트에서 정적인 HTML 페이지를 반환해보는 것을 해보았습니다. <br/>
하지만 대부분의 상황에서는 정적 HTML 코드만 존재하는 경우는 별로 없으며 서버에서 데이터를 관리하는 경우가 빈번하고 사용자에게 다시 전달하는 HTML 코드에 동적으로 출력하길 원하는 데이터를 서버에 두는 것입니다.

특정 데이터 모델에 따른 결과를 출력하는 소프트웨어 컴포넌트를 말합니다. 즉, **지정된 템플릿 양식과 데이터가 합쳐져서 HTML 출력하는 소프트웨어**입니다.

우리는 클라이언트 사이드 템플릿 엔진을 사용할 것입니다. <br/>
이것은 HTML 형태로 코드 작성이 가능하며 동적인 DOM을 형성 가능하도록 해줍니다.

### 💥 왜 사용하는 거에요?

이전 포스팅에서 `express`를 이용해 요청이 들어온 서버에게 응답을 보내기 위해서는 `res.send()`메소드를 사용하여 웹브라우저에 보일 내용을 작성했습니다. <br/>

하지만, `send`의 인자로 HTML내용이나 일반 HTML 파일로 작성하는 것은 불편하기도 하며 동적이지 않습니다.

템플릿 엔진을 사용하면 템플릿 엔진 파일을 `send` 메소드가 아닌 `render`메소드를 사용하여 바로 렌더링이 가능합니다.

템플릿 엔진을 사용하면 **코드 간소화**가 큽니다. HTML에 비해 간단한 문법을 통해 코드가 간결하게 됩니다!
또한, **높은 재사용성**입니다. <br/>
`ejs`문법을 사용하면 엄청난 재사용성을 보여줄 수 있습니다. <br/>

## 2️⃣ 템플릿 엔진: EJS

템플릿 엔진으로는 여러 가지가 존재합니다. `pug`, `handlebars`, `ejs` 등등.. 하지만 우리는 EJS를 사용할 것입니다.

EJS는 Embedded JavaScript의 약자로 HTML과 javascript 코드 결합이 가능한 템플릿 엔진입니다. <br/>

### ✔️ 문법이 어떻게 되나요?

- <% ... %>: Javascript 코드를 실행합니다.
- <%= ... %>: Javascript 표현식의 결과를 문자열로 출력합니다.
- <%- ... %>: 이스케이스되지 않은 데이터를 출력합니다. HTML태그를 웹브라우저에서 해석하여 보여주고 싶을 때 사용합니다.
- <%# ... %>: 주석 처리 방법입니다.

이 외에도 여러 태그가 존재합니다. 이 태그들은 HTML이 아니니 HTML로 취급하지 말라고 지시하기 위해 사용하는 구문을 말합니다.<br/>
[`EJS 문법`](https://ejs.co/#docs) 더 많은 문법은 왼쪽 링크에 들어가셔서 참고하시면 됩니다🌝

### ✔️ 설치

```jsx
npm install ejs
```

위처럼 ejs를 설치할 수 있습니다.

### ✔️ ejs 구성을 위한 작업

```jsx
const express = require("express");
const app = express();
app.set("view engine", "ejs");
app.set("views", "views");
```

위처럼 작업을 하면 됩니다. `ejs` 자리에 다른 템플릿 엔진을 넣으면 그 엔진에 맞게 적용됩니다. 물론 그 엔진을 설치했다는 가정하에요.<br/>
그리고 `views` 폴더를 `views`로 설정해야 합니다. 현재 저는 `views` 폴더라고 애초에 설정 해놓았기 때문에 마지막 코드가 필요가 없지만 다른 폴더라고 하면 위와 같은 코드를 꼭 작성 해주셔야 합니다. <br/>

설정해야 하는 이유는 express는 템플릿이 `/views` 디렉토리 안에 있다고 가정하고 기본 설정도 그렇게 되어 있기 때문입니다.

### ✔️ ejs 사용 해보기

다음과 같은 컨트롤러가 있습니다.

```jsx
exports.getIndex = (req, res, next) => {
  Product.fetchAll((products) => {
    res.render("shop/index", {
      prods: products,
      pageTitle: "Shop",
      path: "/",
    });
  });
};
```

`render` 메소드는 첫 번째 인자로는 템플릿 엔진 파일을 받을 수 있습니다. `shop/index`라고 하면 shop폴더 안에 있는 index.ejs 파일을 렌더링 하라는 의미입니다. <br/>
두 번째 인자로는 객체를 받을 수 있습니다. <br/>
위와 같이 하면 객체 안에 있는 변수를 ejs태그 안에 사용할 수 있게 됩니다 !

```jsx
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%= pageTitle %></title>
  </head>
  <body>
    <% for (let product of prods) { %>
    <header class="card__header">
      <h1 class="product__title"><%= product.title %></h1>
    </header>
    <% } %>
  </body>
</html>
```

이런식으로 태그 안에 객체를 사용 가능합니다. for문도 위와 같이 사용 가능하며 괄호는 하나씩 나누어서 태그안에 넣으면 됩니다. <br/>
컨트롤러에 객체 넣는 것도 다음에 데이터베이스를 활용하면 더욱 편리하고 간소화 가능하겠죠?

조금 낯설 수도 있어도 조금만 사용하다 보면 쉽게 적용 가능할 것입니다!

태그마다 모양이 살짝 다른 것은 위 문법 파트를 참조하시면 됩니다~

#### 🍬 ejs 재사용?

우리는 프로젝트를 만들면 ejs파일이 엄청나게 많아질 것입니다. 하지만 그 파일 중에서도 겹치는 부분이 많을 수도 있습니다. 그 코드마저 간소화하여 재사용할 수 있습니다!

`view`폴더 안에 `includes`폴더를 생성합니다. <br/>
주로 겹치는 부분은 머리, 중간, 끝으로 크게 나눕니다.(사람마다 다를 수 있음)

그래서 위에 HTML코드를 간소화 해보도록 하겠습니다.

`head.ejs` - 머리 부분

```jsx
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%= pageTitle %></title>
```

`navigation.ejs` - 중간 부분 (여기서는 내비게이션 부분)

```jsx
<header class="card__header">
      <h1 class="product__title"><%= product.title %></h1>
    </header>
```

`end.ejs` - 꼬리 부분

```jsx
 </body>
</html>
```

이렇게 총 세 부분으로 나누었습니다. 이제 이것을 ejs 파일에 적용해보도록 하겠습니다.

```jsx
<%- include('/includes/head.ejs') %>
  </head>
  <body>
    <% for (let product of prods) { %>
    <%- include('/includes/navigation.ejs') %>
    <% } %>
<%- include('/includes/end.ejs') %>
```

이렇게 해서 아주 간소화된 ejs파일이 만들어졌습니다. <br/>
이제 공통적인 부분은 다른 ejs파일에서 재사용도 가능하다는 장점이 드러나게 됩니다 !

---

이렇게 해서 템플릿 엔진과 그 중 하나인 EJS에 대해서 알아보는 시간이었습니다! <br/>
조금 짧고 세부적이지는 않지만 정확한 내용은 공식 사이트에서도 확인이 가능하니까 참고용으로 봐주시면 좋을 것 같습니다🌸<br/>

오늘도 제 포스팅 읽어주셔서 감사합니다💌
