---
title: "[Node] Express MVC Pattern"
categories: [node]
tags: [express, node, mvc]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 아홉 번째 포스팅

안녕하세요! 아홉 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

장마가 끝나고 엄청난 폭염이 찾아왔어요,,😓 정말 끔찍하게 더운 날씨가 이어지고 있어요 ㅠㅠ 다들 더위 조심하시고 시원한 곳에서 공부합시다😂

오늘의 포스팅 내용은 **MVC 패턴**입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[붕라니] Here We Go 💛**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

## 1️⃣ MVC란?

관심사를 분리하는 것입니다. 코드마다 각기 다른 기능을 가지고 있기 때문에 어떤 코드가 어떤 작업을 책임지고 있는지 확실히 분리하기 위함이죠.

MVC는 약자입니다. **M**odel **V**iew **C**ontroller를 뜻합니다.

![mvc2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/64dc91e7-8491-495f-ad78-7b29d2470ce3)

### 💫 View

**뷰(View)는 사용자에게 보여지는 화면을 말해요.**<br/> 그래서 이 폴더 안에는 저번 시간에 배웠던 `ejs` 파일을 넣어줍니다. 물론 ejs파일 뿐 아니라 html 외에도 다양한 템플릿 엔진을 사용 가능합니다. <br/>

- 사용자가 볼 수 있는 화면
- 논리가 너무 많이 들어가서는 안됨
- 모델, 컨트롤러와 같이 다른 구성요소들을 몰라야 함

### 💫 Model

**모델(Model)은 기본적으로 객체나 데이터를 나타내는 코드를 말합니다.**<br/>
데이터를 주고 받으며 데이터 관련 작업을 할 수 있습니다.<br/>

- 데이터를 표현하는 역할
- 데이터를 관리하는 역할 (saving, fetching, ...)
- 메모리, 파일, 데이터베이스에서 데이터를 관리하는지 여부는 중요하지 않음
- 데이터 관련 로직 포함

### 💫 Controller

**컨트롤러(Controller)는 모델과 뷰 사이의 연결점이라고 할 수 있습니다.**<br/>
뷰는 애플리케이션 로직과 관련이 없고 모델은 데이터를 저장하고 가져오는 역할을 하기 때문에 컨트롤러가 모델과 같이 데이터를 저장하거나 저장 프로세스를 유발할 수 있습니다. 또한 뷰에서 가져온 데이터를 전달하는 경우도 마찬가지로 컨트롤러가 돕습니다. **중개인**과 같은 역할이라고 생각하시면 쉽습니다.

- 모델과 뷰를 잇는 중개인
- 애플리케이션의 메인 로직을 담당
- 모델, 뷰에 대해 알고 있어야 함
- 사용자가 데이터를 수정하는 것에 대한 '이벤트' 처리 부분

## 2️⃣ MVC 패턴 사용

### ➿ Model 적용

`model/product.js`

```js
const products = [];
module.exports = class Product {
  constructor(title) {
    this.title = title;
  }

  save() {
    products.push(this);
  }

  static fetchAll() {
    return products;
  }
};
```

위와 같은 상품에 대한 로직을 만 수 있습니다. 위 클래스는 컨트롤러에서 import하여 사용 가능합니다. <br/>

#### 🔍 static 메소드?

위 `fetchAll()` 메소드가 `static`인 이유는 해당 메소드가 클래스의 인스턴스를 생성하지 않고도 호출할 수 있기 때문입니다. <br/>

위 `Product` 클래스는 상품 정보를 다루는 모델로 사용합니다. `fetchAll` 메소드는 모든 상품 정보를 가져오는 역할을 합니다. 모든 상품 정보를 얻기 위해서는 특정 인스턴스가 필요하지 않기 때문입니다. 즉, 상품 목록은 모든 인스턴스에서 공유되고 동일한 데이터를 반환해야 하기 때문에 인스턴스를 생성하지 않고도 접근 가능해야 하기 때문에 `static` 정적 메소드로 구현 되었습니다.

### ➿ Controller 로직

`controllers/admins.js`

```js
const Product = require("../models/product");

exports.getAddProduct = (req, res, next) => {
  res.render("admin/add-product", {
    pageTitle: "Add Product",
    path: "/admin/add-product",
  });
};

exports.postAddProduct = (req, res, next) => {
  const title = req.body.title;
  const product = new Product(title);
  product.save();
  res.redirect("/"); // 응답을 줘야하니 리다이렉트 해줌
};
```

위는 컨트롤러 로직입니다. `getAddProduct`를 export하여 `route`에서 사용할 수 있습니다. <br/>

> 참고로 `render`할 때 ejs파일은 [`[Node] Express 템플릿 엔진 EJS`](https://bbjbc.github.io/node/eighth-posting/) 을 참고하시기 바랍니다.

`controllers/shop.js`

```js
const Product = require("../models/product");

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

하나의 컨트롤러 폴더에 모든 컨트롤러가 들어갈 수 있지만 여러 개의 컨트롤러가 존재하면 헷갈리고 코드 수도 길어져 가독성이 떨어지므로 각 테마에 따라 파일을 분리하여 사용하는 것을 추천드립니다.

#### 🔍 Controller 사용

위에서 만든 컨트롤러를 `routes.js`에서 사용 가능하도록 import하여 전달해줍니다.

`routes/shop.js`

```js
const express = require("express");

const shopController = require("../controllers/shop"); // 상대 경로를 사용함

const router = express.Router();

router.get("/", shopController.getIndex);
```

마지막 라인처럼 이동할 경로 작성 후 해당 컨트롤러에서 사용할 컨트롤러를 함께 작성해줍니다.

`routes/admin.js`

```js
const express = require("express");

const adminController = require("../controllers/admin");

const router = express.Router();

// /admin/add-product => GET
router.get("/add-product", adminController.getAddProduct);

// /admin/add-product => POST
router.post("/add-product", adminController.postAddProduct);
```

요청 방식에 따라서 작성할 수 있습니다.

---

오늘은 **MVC 패턴**에 대해서 알아보았습니다. <br/>
데이터 및 논리 제어를 구현하는데 널리 사용되는 소프트웨어 디자인 패턴인만큼 더 나은 업무의 분리와 향상된 관리를 제공하니까 **꼭** 알아야 합니다❗️

오늘도 글 읽어주셔서 감사합니다🎵
