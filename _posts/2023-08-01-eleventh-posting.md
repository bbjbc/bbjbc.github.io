---
title: "[Node] Express + Sequelize"
categories: [node]
tags: [express, node, database, sql, sequelize]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

<br/>

# 열한 번째 포스팅

안녕하세요! 열한 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

8월의 시작입니다. 매미가 엄청 울고 엄청 더운 날씨가 이어지고 있어요💦<br>
다들 8월에도 좋은 기운을 받아 열심히 행복하게 생활할 수 있도록 기원합니다💪

오늘의 포스팅 내용은 **Sequelize**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ Sequelize란?

저번 시간에는 express에서 MySQL을 통해 데이터베이스를 가져오고 사용해 보았습니다. 지난 시간에서는 코드 내부에 SQL문법을 사용했지만 외부 패키지를 통해 JS 객체를 다루면 데이터베이스에 간편하게 새 요소를 생성하거나 편집하고 삭제하거나 찾을 뿐 아니라 연결하는 데 편리할 겁니다. <br/>

**Sequelize**는 서드파티 패키지로 ORM입니다. <br>

> **ORM**이란? **O**bject-**R**elational **M**apping Library를 말합니다.<br/> > &nbsp;이것은 백그라운드에서 실제로 SQL 코드를 처리하며 JS 객체로 매핑하여 SQL 코드를 실행하는 편리한 메소드를 제공하기 때문에 SQL 코드를 직접 작성하지 않아도 됩니다.

User에 name, age, email, pw가 있다고 가정합니다. 객체가 Sequelize에 의해 db에 매핑되면 테이블을 자동으로 생성합니다. 테이블뿐 아니라 관계까지 자동으로 설정합니다. 새로운 사용자를 만들기 위해 User라는 객체에 메소드를 호출하면 Sequelize가 필요한 SQL 쿼리 및 명령을 실행하게 됩니다.

```sql
INSERT INTO users VALUES (1, 'Chan', 23, 'chan23');
```

위와 같은 쿼리를 작성하지 않고

```js
const user = User.create({ name: "Chan", age: 23, pw: "chan23" });
```

이렇게 메소드를 호출하면 쿼리를 작성하지 않아도 SQL 코드를 실행하게 됩니다.

Sequelize는 데이터베이스를 다루는 모델을 제공하며 정의도 가능합니다. 모델을 구성하는 데이터베이스에 저장될 데이터를 정의할 수 있습니다. 모델과 같은 클래스를 실체화하려면 생성자 함수를 실행하거나 utility 메소드를 이용하여 새로운 객체를 생성합니다. 또한 쿼리를 실행할 수 있으며 연관 지을 수 있습니다.

- Models
- Instances
- Queries
- Assciations

---

# 2️⃣ Sequelize 연결

## ⭕️<u>설치</u>

```jsx
npm install --save sequelize
```

sequelize를 설치하기 위해서는 mysql2가 설치되어 있다는 전제 조건 하에 가능합니다.
<br/>

## ⭕️<u>Sequelize & Express 초기 설정</u>

`sequelize`를 적용하기 위해서 기존에 있던 테이블을 삭제하고 `database.js`로 가서 연결 시켜야 합니다.

```jsx
const Sequelize = require("sequelize");

const sequelize = new Sequelize("SCHEMA_NAME", "ROOT_NAME", "ROOT_PW", {
  dialect: "mysql",
  host: "localhost",
});

module.exports = sequelize;
```

이렇게 하면 sequelize 객체를 사용할 수 있게 됩니다.
<br/>

## ⭕️<u>모델 작성</u>

```jsx
const Sequelize = require("sequelize");

const sequelize = require("../util/database");

const Product = sequelize.define("product", {
  id: {
    type: Sequelize.INTEGER,
    autoIncrement: true, // 자동으로 커짐
    allowNull: false, // 공백값 x
    primaryKey: true,
  },
  title: Sequelize.STRING,
  price: {
    type: Sequelize.DOUBLE,
    allowNull: false,
  },
  imageUrl: {
    type: Sequelize.STRING,
    allowNull: false,
  },
  description: {
    type: Sequelize.STRING,
    allowNull: false,
  },
});

module.exports = Product;

// docs.sequelizejs.com
```

위는 상품 모델인데 필요한 요소에 대해 기본값을 정의해 주는 것입니다. 자세한 내용은 [`sequelizejs`](docs.sequelizejs.com) 홈페이지에서 찾아보면 됩니다.

이제 테이블을 sequelize를 통해서 생성하라고 전달하면 됩니다. <br>

## ⭕️<u>Sequelize 연결</u>

`app.js`에서 앱을 시작할 때 모든 모델이 테이블로 이동했는지 또는 파일에 속한 테이블을 다 불러왔는지 확인합니다.

```jsx
const sequelize = require("./util/database");

sequelize
  .sync()
  .then((result) => {
    console.log(result);
  })
  .catch((err) => console.log(err));
```

sequelize를 호출하고 `sync()` 메소드를 호출합니다. sync 메소드는 정의한 모든 모델을 둘러봅니다. 이것의 역할은 모델을 데이터베이스로 동기화하여 해당하는 테이블을 생성하고 관계가 있다면 관계도 생성합니다.
<br>

![데이터베이스4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/7281a81b-4964-40f3-88f2-763d2d708ddb)
<br/>

위와 같이 아까 `product` 테이블이 생성 되었음을 나타내주는 콘솔창입니다.

<br/>
![데이터베이스5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/0c42ba61-cc56-4b8c-aaf3-02d4586d9fb6)
<br/>

이제 MySQL 워크벤치에 들어가서 새로고침 후 확인하면 sequelize를 통해 테이블이 생성된 것을 확인할 수 있습니다❕<br>
추가적으로 `createdAt`, `updatedAt` 두 필드가 sequelize에 의해 추가 되었습니다. 우리 대신 자동으로 타임스탬프를 관리해 주는 것입니다.

---

# 3️⃣ Sequelize 사용

## ⭕️<u>데이터 삽입</u>

```jsx
const product = new Product(null, title, imageUrl, price, description);
product
  .save()
  .then(res.redirect("/"))
  .catch((err) => console.log(err));
```

sequelize가 아닌 db를 활용할 때는 위처럼 작성했었습니다.

```jsx
Product.create({
  title: title,
  price: price,
  imageUrl: imageUrl,
  description: description,
})
  .then((result) => console.log(result))
  .catch((err) => console.log(err));
```

create메소드는 한 번에 과정을 완성하지만 build는 JS 객체를 받은 후 직접 저장해야 합니다. 따라서 `create`를 사용하였습니다. 위처럼 작성하면 자동으로 데이터베이스에 저장할 수 있는 로직이 작성됩니다.

```jsx
exports.postAddProduct = (req, res, next) => {
  const title = req.body.title;
  const imageUrl = req.body.imageUrl;
  const price = req.body.price;
  const description = req.body.description;
  Product.create({
    title: title,
    price: price,
    imageUrl: imageUrl,
    description: description,
  })
    .then((result) => console.log(result))
    .catch((err) => console.log(err));
};
```

위처럼 `postAddProduct`를 실행하기 위해 작성하면 sequelize가 알아서 추가를 해줍니다!<br>

![데이터베이스6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/4e14fa9c-9cb3-4f1f-b2d7-eae989bf80f1)<br>

콘솔창에 `INSERT`를 통해 삽입이 된 것을 알 수 있고 <br>

![데이터베이스7](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/394e68ae-05b7-4545-8329-d1e183f4e408)
<br>

다음처럼 MySQL 워크벤치에도 업데이트가 된 것을 볼 수 있습니다 !
<br/>

## ⭕️<u>데이터 가져오기</u>

기존에는 `fetchAll()`메소드를 통해서 데이터를 가져왔는데 sequelize는 fechAll메소드를 지원하지 않습니다. 하지만 다른 메소드를 사용할 수 있는데 그것이 `findAll()`메소드입니다. 이것도 마찬가지로 promise를 반환합니다. <br>
findAll은 몇 가지 옵션으로 구성됩니다. 그 중에서도 `where`을 통해 검색하는 데이터의 유형을 제한할 수 있습니다.

```jsx
exports.getIndex = (req, res, next) => {
  Product.findAll()
    .then((product) => {
      res.render("shop/index", {
        prods: product,
        pageTitle: "Shop",
        path: "/",
      });
    })
    .catch((err) => console.log(err));
};
```

이렇게 사용하여 아까 삽입했던 품목들을 `findAll()`할 수 있습니다.
<br/>

## ⭕️<u>단일 제품 가져오기</u>

```jsx
exports.getProduct = (req, res, next) => {
  const prodId = req.params.productId;
  Product.findByPk(prodId)
    .then((product) => {
      res.render("shop/product-detail", {
        product: product,
        pageTitle: product.title,
        path: "/products",
      });
    })
    .catch((err) => console.log(err));
};
```

`findById()` 메소드가 sequelize에선 적용이 되지 않아 그에 대응 메소드인 `findByPk()` 메소드를 통해 가져올 수 있습니다. <br/>
하지만 위에서 `findAll()`메소드는 `where`옵션을 가져 검색하는 데이터의 유형을 제한할 수 있다고 했습니다. 쿼리를 통해 사용할 수 있는 방법입니다.

```jsx
exports.getProduct = (req, res, next) => {
  const prodId = req.params.productId;
  Product.findAll({ where: { id: prodId } })
    .then((product) => {
      res.render("shop/product-detail", {
        product: product[0],
        pageTitle: product[0].title,
        path: "/products",
      });
    })
    .catch((err) => console.log(err));
};
```

`findAll()`메소드는 디폴트로 배열을 반환합니다. <br>
이렇게 해서 2가지의 메소드를 통해 단일 제품에 가져오는 방법에 대해서 알아 보았습니다.
<br/>

## ⭕️<u>상품 편집하기</u>

```jsx
exports.getEditProduct = (req, res, next) => {
  const editMode = req.query.edit;
  if (!editMode) {
    return res.redirect("/");
  }
  const prodId = req.params.productId;
  Product.findByPk(prodId)
    .then((product) => {
      if (!product) {
        return res.redirect("/");
      }
      res.render("admin/edit-product", {
        pageTitle: "Edit Product",
        path: "/admin/edit-product",
        editing: editMode,
        product: product,
      });
    })
    .catch((err) => console.log(err));
};
```

위와 같이 sequelize를 사용하여 변경해줄 수 있습니다.

```jsx
exports.postEditProduct = (req, res, next) => {
  const prodId = req.body.productId;
  const updatedTitle = req.body.title;
  const updatedPrice = req.body.price;
  const updatedImageUrl = req.body.imageUrl;
  const updatedDescription = req.body.description;
  Product.findByPk(prodId)
    .then((product) => {
      product.title = updatedTitle;
      product.price = updatedPrice;
      product.imageUrl = updatedImageUrl;
      product.description = updatedDescription;
      return product.save();
    })
    .then((result) => {
      console.log("UPDATED PRODUCT!");
      res.redirect("/admin/products");
    })
    .catch((err) => console.log(err));
};
```

이제 편집한 사항을 post해줘야 하는데 기존에 사용하던 클래스형을 지운 후 sequelize를 사용하여 바꿔줍니다.
`save()`메소드는 sequelize에서 업데이트된 사항을 덮어 쓰고 업데이트 해주는 기능을 합니다. 따라서 편집을 위해서는 있어야 하는 메소드입니다. 위에 .then 구문이 2개나 있는데 두 구문 모두 에러를 잡아줄 수 있습니다. 새롭게 편집을 성공한다면 그 다음으로 넘어가 로깅을 할 수 있습니다.

`redirect` 문장이 원래 맨 아래에 있었는데 올린 이유는 js는 위에서 아래로 읽다보니 then구문을 처리하면 바로 빠져나가 밖에 있는 구문을 실행합니다. 따라서 편집된 내용을 바로 리다이렉트하여 업데이트하여 보기 위해서는 편집이 성공 시에만 리다이렉트 되는 식으로 하기 위해서 두 번째 then구문 안에 넣어 주었습니다.
<br/>

## ⭕️<u>상품 삭제하기</u>

```jsx
exports.postDeleteProduct = (req, res, next) => {
  const prodId = req.body.productId;
  Product.deleteById(prodId);
  res.redirect("/admin/products");
};
```

sequelize에는 `deleteById`메소드가 존재하지 않습니다.

```jsx
exports.postDeleteProduct = (req, res, next) => {
  const prodId = req.body.productId;
  Product.findByPk(prodId)
    .then((product) => {
      return product.destroy();
    })
    .then((result) => {
      console.log("DESTROYED PRODUCT");
      res.redirect("/admin/products");
    })
    .catch((err) => console.log(err));
};
```

sequelize에서는 `destroy()`메소드를 통해 삭제할 수 있습니다. 일치하는 제품 id를 찾아 그 상품을 삭제하게끔 합니다.

---

# 3️⃣ Sequelize 관계

```jsx
const Product = sequelize.define("users", {});
const User = sequelize.define("products", {});
const Cart = sequelize.define("carts", {});
const CartItem = sequelize.define("cartitems", {});
```

위와 같이 4개의 모델이 있습니다.
<br/>

## ⭐️ One-to-One

일대일 관계를 정의하는 방법에 대해 알아보도록 하겠습니다. 위 4개의 모델 중 1:1 관계를 성립하는 모델은 `User`와 `Cart` 모델입니다.

```jsx
User.hasOne(Cart);
Cart.belongsTo(User);
```

User는 Cart를 하나만 가질 수 있고 Cart는 User에 대해 속한다를 정의하고 있는 것입니다.

> 원본(Source-부모 테이블)으로 사용할 것이 User이기 때문에 hasMany를 붙인 것이고, <br> 타겟(Target-자식 테이블)으로 사용할 것이 Product이기 때문에 belongsTo를 붙인 것입니다.

<br/>

## ⭐️ One-to-Many

일대다 관계를 정의하는 방법도 일대일 방법과 비슷합니다.

```jsx
User.hasMany(Product);
Product.belongsTo(User);
```

User는 여러 개의 Product를 구매 가능하며 Product는 User에 속해 있습니다. `hasMany`메소드를 통해 정의 가능합니다. 위와 같은 경우에 Product모델에 User 모델을 참조하기 위한 `userId`라는 외래키 속성이 생깁니다. 관계가 어떻게 관리될지를 정의할 수 있는 것입니다.
<br/>

## ⭐️ Many-to-Many

다대다 관계는 조금 다릅니다.

```jsx
Cart.belongsToMany(Product, { through: CartItem });
Product.belongsToMany(Cart, { through: CartItem });
```

`Cart`와 `Product`는 다대다 관계를 가집니다.
`belongsToMany`메소드를 사용하여 연결을 할 수 있습니다. `through`옵션을 추가하여 sequelize에게 이 연결들의 저장 위치를 알려주어 `CartItem` 모델에 저장되는 것입니다. 중간 위치에 있다는 것입니다.
<br/>

## 💧 Option

### ✂️ Cascade

`belongsTo`메소드 안에는 선택적으로 여러 옵션을 줄 수 있기도 합니다.

```jsx
User.hasOne(Cart, {
  onDelete: "CASCADE",
  onUpdate: "CASCADE",
  constraints: true,
});
Cart.belongsTo(User);
```

이런 식으로 `CASCADE` 옵션이나 제약조건을 설정할 수 있습니다.

> **CASCADE ?** <br>
> 참조 관계가 있을 경우 참조되는 데이터도 자동으로 삭제 가능한 조건입니다.

---

### 👤 Foreign Key

외래키의 이름을 다르게 설정하고 싶을 때도 옵션을 통해서 정의 가능합니다.

```jsx
User.hasOne(Cart, {
  foreignKey: "user__id",
});
Cart.belongsTo(User);
```

---

지금까지 **sequelize**를 **express**에 적용하는 방법에 대해서 알아보았습니다.<br/>
코드 안에 쿼리문을 작성하지 않고 sequelize의 객체를 사용하여 간편하게 사용할 수 있었습니다. <br>

![데이터베이스8](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/cf14c1c5-066a-4210-9607-c6c8e07c2fea)
<br>

위와 같이 쿼리를 계속 삽입해야 한다면 정말 귀찮고 불편한 일이겠죠? 잘 숙지하면 엄청 편리한 기능인 것 같습니다.

긴 글 읽어주셔서 감사합니다🌝 8월 화이팅👋
