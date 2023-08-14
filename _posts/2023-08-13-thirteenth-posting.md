---
title: "[Node] Express + Mongoose"
categories: [mongodb, node]
tags: [express, node, database, nosql, mongoose]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열세 번째 포스팅

안녕하세요! 열세 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

수강신청은 망했고 개강은 다가오고 8월은 반이나 지났고 ~
악재의 연속 아닐까요💢 <br>**그래도,, 화이팅..👍👌**

오늘의 포스팅 내용은 **Mongoose**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ Mongoose란?

Mongoose는 **ODM**입니다. <br/>

> **ODM** **===** **O**bject-**D**ocument **M**apping Library

ODM은 데이터베이스와 객체지향 프로그래밍 언어 사이에 호환되지 않는 데이터를 변환하는 프로그래밍 기법입니다.
즉, MongoDB에 있는 데이터를 애플리케이션에서 JS 객체로 사용할 수 있도록 해줍니다.

Sequelize는 ORM(Object Relational Mapping Library) 였습니다. <br>
둘의 차이점은 MongoDB가 단순히 관계형 데이터베이스가 아니라 문서형 데이터베이스로 문서 관점으로 실행되는 **ODM**이라는 것입니다. 하지만 둘 다 애플리케이션에 데이터와 사용자 개체가 있으며 이를 컬렉션(테이블)에 저장한다는 점은 같습니다. <br>

Sequelize처럼 Mongoose는 모델을 정의해서 모든 쿼리가 배후에서 작성되도록 돕습니다. Mongoose의 핵심은 스키마와 모델을 다루어 데이터가 어떻게 보일지 정의한다는 것입니다.

---

# 2️⃣ Mongoose 준비

## ▶️ 설치

```jsx
npm install --save mongoose
```

<br>

## ▶️ 데이터베이스 연결

Mongoose가 배후에서 유틸리티 및 연결 관리를 관할하기 때문에 `app.js`에서 불러와 바로 사용 가능합니다.

```jsx
const mongoose = require("mongoose");
mongoose
  .connect("URI")
  .then((result) => app.listen(3000))
  .catch((err) => {
    console.log(err);
  });
```

`URI`은 본인의 MongoDB 클러스터 URI을 입력하면 됩니다. 이렇게 하면 연결에 대한 준비를 마쳤습니다.

---

# 3️⃣ Mongoose CRUD

## ➰ 모델 CREATE 및 스키마 사용

```jsx
const mongoose = require("mongoose");

const Schema = mongoose.Schema;

const productSchema = new Schema({
  title: {
    type: String,
    required: true,
  },
});
```

mongoose를 import한 후에 스키마를 사용하기 위해 스키마 선언을 해준 뒤 제품 스키마에 적용을 위한 생성자에 JS 객체를 전달하고 이 객체에는 제품이 어떻게 보여야 하는지 정의하면 됩니다.

**하지만❗️**<br>
MongoDB는 Schemaless라고 했습니다. 하지만 지금 스키마를 생성하는 이유는 무엇일까요?

> 특정 스키마에 한정되지 않는 유동성이 있는 반면 다루는 데이터에는 특정 구조가 있을 것이기 때문입니다. 따라서 Mongoose는 사용자가 데이터에만 집중하도록 돕기 위하여 사용자의 데이터가 어떻게 생겼는지 알아야 하고 이 때문에 데이터 구조에 대한 스키마를 정의해야 하는 것입니다. 하지만 반드시 필요하지는 않습니다. 유연성은 줄지만 다음에 애플리케이션에서 오류를 발생시킬 가능성이 있으므로 일종의 스키마를 가지는 것이 좋습니다.

Mongoose를 사용하면 데이터 작업을 할 수 있는 구조를 갖추게 됩니다. 유연성을 일부 포기하는 대신 몇 가지의 이점을 얻기로 하는 것입니다.

```jsx
module.exports = mongoose.model("Name", productSchema);
```

mongoose의 모델을 내보내기 위해 위와 같이 사용합니다. `model()`함수는 mongoose가 배후에서 스키마에 연결하는 것을 돕습니다. 안에는 사용하고자 하는 모델의 이름을 넣어주면 됩니다. 첫 번째 인수에 작성한 것이 소문자로 바뀌며 복수형으로 변화하여 컬렉션 이름으로 저장됩니다. 두 번째 인수는 스키마로 위에서 정의한 것을 작성해 주면 됩니다.

<br>

## ➰ 제품 READ

### 💠 모든 제품 가져오기

```jsx
exports.getProducts = (req, res, next) => {
  Product.find()
    .then((products) => {
      console.log(products);
      res.render("shop/product-list", {
        prods: products,
        pageTitle: "All Products",
        path: "/products",
      });
    })
    .catch((err) => {
      console.log(err);
    });
};
```

mongoose에서는 `fetchAll()` 메소드가 없기 때문에 내장 메소드인 `find()`메소드를 사용합니다. mongoose에서는 조금 다르게 동작합니다. 커서 대신 products를 줍니다. 따라서 `cursor()`를 호출하여 커서에 접근할 수 있습니다.
<br>

![mongoose1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/9bf6d362-5d90-4b77-8347-e68f8305803f)
<br>

Mongoose에서 find()를 사용하면 자동으로 배열을 출력하게 됩니다.

<br>

### 💠 모든 제품 가져오기

```jsx
exports.getProduct = (req, res, next) => {
  const prodId = req.params.productId;
  Product.findById(prodId)
    .then((product) => {
      res.render("shop/product-detail", {
        product: product,
        pageTitle: product.title,
        path: "/products",
      });
    })
    .catch((err) => console.log(err));
```

mongoose는 `findById()`메소드를 지원합니다. findById에 문자열을 전달하면 mongoose가 알아서 ObjectId로 변환합니다. Mongoose를 사용하는 것만으로 이렇게 많은 이점이 있습니다.

<br>

## ➰ 제품 UPDATE

```jsx
exports.postEditProduct = (req, res, next) => {
  const prodId = req.body.productId;
  const updatedTitle = req.body.title;
  const updatedPrice = req.body.price;
  const updatedImageUrl = req.body.imageUrl;
  const updatedDescription = req.body.description;
  Product.findById(prodId)
    .then((product) => {
      product.title = updatedTitle;
      product.price = updatedPrice;
      product.description = updatedDescription;
      product.imageUrl = updatedImageUrl;
      return product.save();
    })
    .then((result) => {
      console.log("UPDATED PRODUCT!");
      res.redirect("/admin/products");
    })
    .catch((err) => console.log(err));
};
```

**CRUD 기능**을 완성하기 위해 업데이트를 진행합니다. 새로운 제품 생성자를 통해 `save()`를 호출하는 대신 prodId를 통해 가져옵니다. <br>

![mongoose2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/ae29b24d-92b4-4edd-a1f8-418d46c9796a)
<br>

> <i>Saves this document by inserting a new document into the database if document.isNew is true, or sends an updateOne operation with just the modified paths if isNew is false.</i>

`product`가 데이터를 가진 JS 객체가 아닌 Mongoose 객체이기 때문에 save 메소드를 사용할 수 있습니다. 기존에 있던 객체에 save()를 호출하면 새로운 객체로 저장되지 않고 변경 사항이 저장되기 때문에 배후에서 자동으로 업데이트가 됩니다.

<br>

## ➰ 제품 DELETE

```jsx
exports.postDeleteProduct = (req, res, next) => {
  const prodId = req.body.productId;
  Product.findByIdAndRemove(prodId)
    .then((result) => {
      console.log("DESTROYED PRODUCT");
      res.redirect("/admin/products");
    })
    .catch((err) => console.log(err));
};
```

`findByIdAndRemove()`메소드는 Mongoose 내장 메소드로 문서를 제거하는 역할을 합니다.

---

# 4️⃣ User 스키마 사용 및 관계 설정

## ☑️ User 스키마 사용

```jsx
const mongoose = require("mongoose");

const Schema = mongoose.Schema;

const userSchema = new Schema({
  name: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  cart: {
    items: [
      {
        productId: {
          type: Schema.Types.ObjectId,
          required: true,
        },
        quantity: { type: Number, required: true },
      },
    ],
  },
});

module.exports = mongoose.model("User", userSchema);
```

userSchema를 다루기 위해 moongose 패키지에 있는 스키마를 호출합니다. `productId`는 스키마로부터 가져와야 하는데 여기엔 Types 필드가 있으며 ObjectId 등 특별한 유형이 있습니다.

```jsx
const User = require("./models/user");

app.use((req, res, next) => {
  User.findById("ID")
    .then((user) => {
      req.user = user;
      next();
    })
    .catch((err) => console.log(err));
});

mongoose
  .connect("YOUR URI")
  .then((result) => {
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
    app.listen(3000);
  })
  .catch((err) => {
    console.log(err);
  });
```

user의 존재 여부에 따라 user가 존재하는지 `User.findOne`을 통해 확인하고 `findOne`에 인수를 제공하지 않으면 발견하는 첫 user를 반환합니다.

<br>

## ☑️ Mongoose 관계 설정

```jsx
userId: {
    type: Schema.Types.ObjectId,
    ref: "User",
    required: true,
  }
```

product는 user에게 할당 되어야 하기 때문에 `product`모델에서 위와 같은 사항이 추가 되어야 합니다.<br>
여기서 특수한 것을 설정해 주어야 하는데 `ref`은 문자열을 가져다가 Mongoose에게 해당 필드의 데이터에 실제로 연관된 다른 Mongoose 모델이 무엇인지를 알려줍니다. `ref` 다음에는 연관 짓고자 하는 모델의 이름을 사용하면 됩니다.

```jsx
productId: {
          type: Schema.Types.ObjectId,
          ref: "Product",
          required: true,
        }
```

또한 productId를 저장하는 user 모델에 참조를 추가하고 product를 참조할 수도 있으니 `user`모델에도 위와 같이 참조를 추가해줍니다. <br>

**이렇게 ref를 통해서 상관관계를 설정할 수 있습니다.**

---

# 5️⃣ Mongoose Populate() & Select()

## ⭕️ Populate()

> **Population이란?** <br>
> 문서의 지정된 경로를 다른 컬렉션의 문서로 자동 대체하는 프로세스입니다. 단일 문서, 여러 문서, 일반 개체, 여러 개의 일반 개체 또는 쿼리에서 반환된 모든 개체를 채울 수 있습니다.

```jsx
exports.getProducts = (req, res, next) => {
  Product.find()
    // .populate("userId", "name")
    .then((products) => {
      console.log(products);
      res.render("admin/products", {
        prods: products,
        pageTitle: "Admin Products",
        path: "/admin/products",
      });
    });
};
```

`populate()`가 없다면? 제품의 배열이 로그 되어 있습니다. <br/>

![mongoose3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/53658d8d-57bb-4dd6-b749-52767f316058)

<br>
본인이 원하는 데이터를 가지고 오고 싶을 때 `find()`메소드 다음에 올 수 있는 `populate()`를 추가하면 됩니다.

위 코드에

```jsx
Product.find().populate("userId");
```

를 추가하면 userId의 ID만 존재하지 않고 전체 user객체가 등장합니다. <br>

![mongoose4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/bbfc13c1-6e86-4215-8e71-b7d15feb15f0)

<br>
이것은 데이터를 가져올 때 매우 유용하며 직접 중첩된 쿼리를 작성하는 대신 한 번에 모든 데이터를 얻을 수 있게 해줍니다. <br>
또한 두 번째 인수를 전달하여 동일하게 진행 가능합니다. 
```jsx
Product.find().populate('userId', 'name')
```
<br>

![mongoose5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/05baec14-15a6-485b-aa3f-73d4e3ecd497)

<br>
`userId`객체 안에 있는 `name` 까지만 로그되게 할 수 있습니다.

<br>

## ⭕️ Select()

`find()` 다음에는 populate뿐 아니라 `select()`도 호출 가능합니다. 이것은 선택하거나 제외할 필드 즉 데이터베이스에서 실제로 검색할 필드를 정의하도록 도와주는 메소드입니다.

```jsx
exports.getProducts = (req, res, next) => {
  Product.find()
    .select("title price -_id")
    .populate("userId", "name")
    .then((products) => {
      console.log(products);
      res.render("admin/products", {
        prods: products,
        pageTitle: "Admin Products",
        path: "/admin/products",
      });
    });
};
```

`title`, `price`만 추출 하고 싶고 `_id`를 추출하고 싶지 않으면 -를 붙여 제외 가능합니다. 참고로 id는 명시적으로 제외하지 않으면 항상 검색되게 됩니다.
<br>

![mongoose6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/60055b12-5be6-4a26-a550-15808f70a01c)

<br>
위와 같이 출력되는 것을 확인할 수 있습니다.

---

→ [`Mongoose Guide`](https://mongoosejs.com/docs/guides.html) 자세한 내용은 사이트를 찾아보시면 됩니다. <br>
이번에는 MongoDb에 이어 Mongoose에 대해 알아보았습니다. MongoDB보다 더욱 강력하고 편리한 메소드가 많으니 사용하면 편리하겠죠❓

글 읽어주셔서 감사합니두💥
