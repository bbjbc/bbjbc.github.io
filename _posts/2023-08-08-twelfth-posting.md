---
title: "[Node] Express + MongoDB"
categories: [mongodb, node]
tags: [express, node, database, nosql, mongodb]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열두 번째 포스팅

안녕하세요! 열두 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

태풍이 무섭게 북상하고 있다고 합니다. 풍속이 약해지길 바라고 다들 피해 없이 무사히 지나가는 태풍이면 좋겠습니다🙏

오늘의 포스팅 내용은 **MongoDB**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ MongoDB란?

지금까지는 MySQL과 같은 SQL 기반 데이터베이스를 사용하여 애플리케이션 데이터를 저장하는 방법에 대하여 알아보았습니다. 또한 Node.js가 MySQL 데이터베이스와 어떤식으로 상호작용 하는지도 알아보았습니다. <br>

SQL 기반 데이터베이스도 엄청 자주 사용되지만 그만큼 NoSQL도 자주 사용됩니다. NoSQL에 대표적인 기반 데이터베이스인 **MongoDB**에 대하여 알아볼 겁니다❗️

**MongoDB**는 회사명이기도 하지만 데이터베이스 솔루션 혹은 데이터베이스 엔진의 이름이기도 합니다. 효율적인 NoSQL 데이터베이스를 실행 가능한 툴입니다. `Humongous`라는 단어에서 비롯되었고 MongoDB의 주요 목적은 **저장 및 작업**입니다. 아주 방대한 데이터를 저장할 수 있습니다. <br/>즉, 규모가 큰 애플리케이션을 위해 구축된 거라고 말할 수 있습니다. 데이터 쿼리, 저장, 상호 작용 등 아주 빠르게 처리할 수 있으며 훌륭한 데이터베이스라고 할 수 있습니다.

SQL에서 했던 것처럼 MongoDB 서버를 가동하면 다수의 데이터베이스가 생성됩니다. 또한 MongoDB는 스키마 구조가 없기 때문에 구조에 국한되지 않아도 된다는 이점이 있습니다. 이렇게 유연성이 있는 만큼 시간이 지남에 따라 데이터베이스에 표현하기 어렵지 않으면서 애플리케이션의 데이터 요구사항을 변경할 수 있습니다.

MongoDB는 JS 객체 형식과 비슷한 JSON을 통해 컬렉션에 데이터를 저장합니다. 엄밀히 말하자면 Binary JSON인 BSON을 사용하는데 파일에 저장하기 전에 이면에서 데이터를 변형한다는 뜻으로 신경 쓰지 않아도 됩니다.

```json
{
  "name": "Boongranii",
  "age": 23,
  "address": {
    "city": "Hwaseong"
  },
  "hobbies": [{ "name": "Studying" }, { "name": "Weight Traning" }]
}
```

위와 같은 형식으로 데이터를 저장하게 됩니다.

## 🎐 NoSQL 특징

- No Data Schema
  - 특정 구조가 필요하지 않으면서 유연성 향상
- Fewer Data Relations
  - 내장을 통해 관계를 지을 수 있어 데이터 관계 줄어듦

<br>

## 🎐 MongoDB에서의 관계

`Orders`

```json
{
  "id": "gogo",
  "user": { "id": 1, "email": "chan@test.com" },
  "product": { "id": 2, "price": 10.11 }
}
{
  "id": "backback",
  "user": { "id": 2, "email": "back@test.com" },
  "product": { "id": 1, "price": 112.11 }
}
{
  "id": "happycat",
  "product": { "id": 2, "price": 52.21 }
}
```

`Users`

```json
{ "id": 1, "name": "Chan", "email": "chan@test.com" }
{ "id": 2, "name": "Cho", "email": "cho@test.com" }
```

Users 컬렉션에 사용자에 관한 세부 사항이 있지만 해당 데이터의 일부는 다른 컬렉션 문서에 내장되어 있을 수 있습니다. 따라서 SQL에서처럼 ID에 따라 맞추는 것이 아니라 다른 문서에 데이터를 내장함으로써 관계를 표현할 수 있습니다. 다른 문서를 가리키는 ID를 내장해서 두 문서를 직접 병합하는 것입니다.

데이터가 필요한 형식으로 쿼리를 만들어 필요한 형식으로 데이터를 저장해서 추후에 병합 과정을 거치지 않고도 필요한 형식의 데이터를 가져올 수 있도록 만들어 줍니다.

관계를 표현하기 위해 사용할 수 있는 내장 문서 외에도 `References`라는 참조가 있습니다. 내장 문서를 활용하면 중복되는 데이터가 너무 많아지기 때문에 참조만 저장하는 것입니다.

```json
{
    "userName": "cat",
    "Books": [{...},{...}]
}
```

위와 같은 형식을 아래로 표현 가능합니다.

```json
{
  "username": "cat",
  "Books": ["id1", "id2"]
}
```

```json
{
  "_id": "id1",
  "name": "Lord of the Rings1"
}
```

첫번째에서는 책에 대한 참조만 저장한 후 다른 컬렉션으로 관리하는 두번째 예시로 직접 병합하는 것이 좋습니다.

목적에 따라 Embedded Documents 방법이나 References 방법을 사용하면 됩니다.

<br>

---

# 2️⃣ MongoDB 연결

## 🔧 드라이버 설치

→ [**`mongodb.com`**](https://mongodb.com) 여기에서 MongoDB Atlas를 사용하기 위해 회원가입을 하면 됩니다.

> 제품 > Community Server > MongoDB Atlas

![mongo1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/9fa63591-d750-46b6-af34-c0245c601b24)
<br>

회원가입을 하고 클러스터를 생성하면 위와 같은 화면이 뜨게 됩니다. `connect`를 누르면 다음과 같은 화면입니다.<br/>
<br/>
![mongo2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/93fe99e9-1f6a-4bfa-8045-234753407682)<br>

내 애플리케이션에서 사용할 것이니
**Connect to your application**에서 Drivers를 클릭해줍니다.<br/><br/>
![mongo3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/c8d1b41b-54c9-4c54-bc8f-7e43d390b20b)
<br>

다음과 같은 화면이 뜰텐데 맨 아래 코드만 복사한 후 이제 **vscode**에서 연결 해보도록 하겠습니다.

<br>

## ⌛️ 연결

```jsx
npm install --save mongodb
```

콘솔창에 npm을 통해 mongodb를 설치해줍니다. 공식 MongoDB 드라이버가 설치되고 MongoDB에 연결할 수 있게 됩니다.

`/util/database.js`

```jsx
const mongodb = require("mongodb");
const MongoClient = mongodb.MongoClient;

const mongoConnect = (callback) => {
  MongoClient.connect(
    "mongodb+srv://<Id>:<Password>@cluster0.u8voidr.mongodb.net/?retryWrites=true&w=majority"
  )
    .then((client) => {
      console.log("Connected!");
      callback(client);
    })
    .catch((err) => {
      console.log(err);
    }); // MongoDB에 연결 생성
};

module.exports = mongoConnect;
```

mongoDB에 접근을 하여 MongoClient 생성자를 추출할 수 있습니다. `MongoClient.connect`를 해주면 MongoDB에 연결을 생성할 수 있습니다. 그 후 안에는 위에서 복사 해두었던 url을 가져와 넣어줍니다. 그리고 자신이 지정한 Id와 Password를 알맞게 넣어줍니다. 또한 위와 같은 연결 메소드는 promise를 반환하기 때문에 then과 catch 구문도 적절히 넣어줍니다. mongoConnect 함수를 app.js에서 사용하기 위해 export도 해줍니다.

`app.js`

```jsx
const mongoConnect = require("./util/database");
mongoConnect((client) => {
  console.log(client);
  app.listen(3000);
});
```

위처럼 노드 서버에 연결하기 위해 listen을 해주며 mongoConnect를 사용할 수 있습니다.<br>

![mongo4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/5294a749-b97e-43f2-a144-41b32e7c8f46)
<br>
<br/>
Connected!와 함께 MongoClient 객체와 연결에 관한 상세 내역이 나오게 됩니다. 이 객체는 우리가 상호작용하고 작업하여 데이터베이스에 데이터를 생성하는 등 활용 가능합니다.

<br>

---

# 3️⃣ MongoDB 사용

## 🔩 연결 사용

`database.js`에서 MongoDB 내부에 연결하는 위치로 콜백을 전달하게 됩니다. 그리고 콜백을 실행한 뒤 연결된 클라이언트를 반환하여 상호작용을 할 수 있도록 해줍니다. 그러나 이렇게 진행하려면 매 작업마다 MongoDB에 연결해야 할 것이고 작업 후 연결을 해제하지도 못할 것입니다. <br>
따라서 MongoDB에 연결하기에 좋은 방법은 아닙니다. 앱의 다양한 위치에서 연결과 상호작용을 진행하고자 하기 때문입니다. 그러니 데이터베이스의 한 연결을 처리하고 클라이언트로 접근을 반환한 뒤 거기서 설정하거나 앱의 접근이 필요한 다양한 위치로 반환시키는 것이 더 나을 것입니다.

```js
let _db;

const mongoConnect = (callback) => {
  MongoClient.connect(...)
    .then((client) => {
      _db = client.db();
      callback();
    })
    .catch((err) => {
      console.log(err);
      throw err
    });
};
```

\_db라는 변수를 추가하는데 밑줄의 용도는 파일 내부에서만 사용됨을 알리는 용도입니다. 데이터베이스로의 접근을 client.db()에 저장합니다.

```js
const getDb = () => {
  if (_db) {
    return _db;
  }
  throw "No database found!";
};
```

\_db가 설정되어 있는지를 확인하기 위함입니다.

```js
exports.mongoConnect = mongoConnect;
exports.getDb = getDb;
```

두 메소드를 내보내는데 연결을 위한 것과 연결을 저장하는 용도입니다.

```js
const getDb = require("../util/database").getDb;
```

이를 통해 서버를 시작할 때 처음 설정하는 데이터베이스 연결로 접근하게 하는 요소를 임포트하고 있으며 우리가 재사용할 수 있는 개념입니다.

`/models/products.js`

```js
class Product {
  constructor(title, price, description, imageUrl) {
    this.title = title;
    this.price = price;
    this.description = description;
    this.imageUrl = imageUrl;
  }

  save() {
    const db = getDb();
  }
}
```

`getDb()`는 우리가 연결되어 있는 데이터베이스 인스턴스를 반환하기 때문에 데이터베이스와 상호작용이 가능한 연결을 확보하게 되었습니다.

```js
save() {
    const db = getDb();
    db.collection("products")
      .insertOne(this)
      .then((result) => {
        console.log(result);
      })
      .catch((err) => console.log(err));
  }
```

모든 컬렉션에 연결이 가능하며 데이터베이스에서처럼 아직 존재하지 않는 경우 데이터를 처음 입력할 때 생성됩니다. products라는 컬렉션을 생성할 것이며 이 컬렉션에서 MongoDB 명령어나 작업을 실행 가능합니다. <br>
→ [**`MongoDB CRUD Manual`**](https://www.mongodb.com/docs/manual/crud/)에서 여러 명령어를 찾아보실 수 있습니다. 명령어는 promise를 반환하여 then과 catch를 작성해줍니다.

<br>

## 🔨 MongoDB Compass

MongoDB Compass는 MongoDB에서 제공하는 GUI 환경의 MongoDB 클라이언트입니다. 이것을 이용하여 외부에서 MongoDB에 접근 가능합니다. <br/>
→ [**`MongoDB Compass Download`**](https://www.mongodb.com/try/download/compass)에서 다운로드를 합니다.<br>
<br>
설치 후 접속 하면 다음과 같은 화면이 뜹니다. <br><br>
![compass1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/82d0583b-5a7b-4a06-a344-ce69f0074298)<br><br>
URI에는 다음과 같은 과정에 따라 넣으면 됩니다. <br><br>
![compass2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/5753d089-22e1-4b6b-b0fc-e178566a5b14)<br><br>
MongoDB Atlas에서 클러스터 생성에 있는 Compass를 클릭해줍니다.<br><br>
![compass3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/0d5561e9-6b8a-42a2-98b7-6d556061e461)<br><br>
아래 있는 문자열을 아까의 URI에 붙여 넣어준 후 추가적인 설정을 확인하며 연결 합니다.

접속 하고 데이터베이스를 보면 아까 만들었던 shop이 있습니다. <br><br>
![compass4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/627c774b-4afc-4752-a094-235ec726de98)<br><br>
들어가면 생성했던 products 컬렉션에 내가 입력한 데이터가 나타남을 확인할 수 있습니다. <br><br>
![compass5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/181d03bc-4b94-42d5-9dc9-6da69a9d1944)<br><br>

<br>

## 📄 Model 적용

### 🎯 모든 제품 가져오기

`/models/product.js`

```jsx
static fetchAll() {
    const db = getDb();
    return db
      .collection("products")
      .find()
      .toArray()
      .then((products) => {
        console.log(products);
        return products;
      })
      .catch((err) => {
        console.log(err);
      });
  }
```

모든 제품을 가져와 렌더링 하기 위한 메소드입니다. `find`라는 데이터 탐색을 위한 메소드를 사용합니다.

`find`는 promise를 즉시 반환하는 대신 **커서**라는 것을 반환합니다.

> **커서**는 MongoDB가 제공하는 객체로 단계별로 요소와 문서를 탐색합니다. 왜냐하면 이론적으로 컬렉션에서 find는 수많은 분량을 반환 가능하지만 반환하고 싶지 않을 수 있죠. 대신 find는 MongoDB에게 다음 문서를 순차적으로 요청할 수 있는 지팡이 같은 셈입니다. <br>
> 또한 `toArray`메소드를 실행하면 MongoDB에게 모든 문서를 받아서 JS 배열로 바꾸게 할 수 있습니다. <br> `getDb`를 통해 데이터베이스로의 접근을 얻어야 하기 때문에 선언을 해줍니다.

<br>

### 🎯 한 제품 가져오기

하나의 제품을 가져오기 위해서는 id를 통해 가져옵니다.

```jsx
static findById(id) {
    const db = getDb();
    return db
      .collection("products")
      .find({ _id: new mongodb.ObjectId(id) })
      .next()
      .then((product) => {
        console.log(product);
        return product;
      })
      .catch((err) => {
        console.log(err);
      });
  }
```

보유한 데이터베이스에 접근하기 위해 `getDb`를 호출합니다.
하나의 제품만 찾도록 하기 위해서 find를 통해 필터 구성을 해줍니다.

> find는 여전히 커서를 제공할텐데 MongoDB는 한 개의 값만 받는다는 것을 모릅니다. 여기에서는 `next`함수를 사용하여 find를 통해 반환된 마지막 문서를 확보합니다.
> MongoDB의 ID는 조금 다른 방식으로 저장되는데 Compass에서 보듯 ID는 사실 **ObjectId**입니다. <br><br> ![compass6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/0326508b-050a-4ee8-9c69-e255717d1606)<br><br>
> MongoDB가 데이터를 BSON으로 저장하는데 이 JSON의 이진 형식은 단지 작업 속도가 빨라서 사용하는 것은 아니고 MongoDB가 내부에 특별한 유형의 데이터를 저장할 수 있기 때문인데 ObjectId가 이런 유형에 속합니다. <br> **즉, ObjectId는 MongoDB가 제공하는 객체입니다.**

```jsx
const mongodb = require("mongodb");
```

위를 임포트한 후 이 상수를 사용하여 ObjectId에 접근 가능합니다. 새로운 생성자인 새 ObjectId를 생성하여 안에 포함될 문자열을 전달 가능합니다.

<br>

### 🎯 제품 UPDATE

제품을 편집하여 새로 업데이트하기 위해서 `save()`메소드를 수정해야 합니다.

```js
save() {
    const db = getDb();
    let dbOperation;
    if (this._id) {
      // Update the product
      dbOperation = db
        .collection("products")
        .updateOne({ _id: this._id }, { $set: this });
    } else {
      dbOperation = db.collection("products").insertOne(this);
    }
    return dbOperation
      .then((result) => {
        console.log(result);
      })
      .catch((err) => {
        console.log(err);
      });
  }
```

모델 생성자에 \_id 인수를 생성하고 `this._id = new mongodb.ObjectId(_id)` 를 넣어준 후 이것이 변경 되었다면 `updateOne()`메소드를 통해 하나의 요소를 업데이트 해줍니다. <br>

> 이 메소드는 적어도 2개의 인수를 넣어줘야 합니다.<br>**첫 번째**는 어떤 요소 또는 어떤 문서를 업데이트하는지 정의하는 필터입니다. 찾는 문서 \_id가 `this._id`와 같은 문서로 `this._id`를 전달하므로 id가 객체의 일부가 됩니다. <br> **두 번째**는 문서를 업데이트하는 방식을 지정합니다. this만 입력하면 MongoDB가 기존의 문서를 대체하게 되므로 안되고 연산을 설명하기 위해 MongoDB만의 특별 속성인 **`$set`**을 사용합니다. 필터를 적용해 찾은 기존 문서에 만들고 싶은 수정 사항을 설명 해야 합니다. MongoDB가 키-값 필드를 데이터베이스에서 찾는 문서에 설정할 것입니다. <br> → [**`$set manual`**](https://www.mongodb.com/docs/manual/reference/operator/update/set/) 상세한 내용은 참고 바랍니다❗️

<br>

### 🎯 제품 DELETE

```js
static deleteById(prodId) {
    const db = getDb();
    return db
      .collection("products")
      .deleteOne({ _id: new mongodb.ObjectId(prodId) })
      .then((result) => {
        console.log("Deleted");
      })
      .catch((err) => {
        console.log(err);
      });
  }
```

→ [**`deleteOne()`**](https://www.mongodb.com/docs/manual/reference/method/db.collection.deleteOne/) 상세한 내용은 참고 바랍니다❗️<br>
`findById`메소드와 비슷한 형식으로 `deleteOne()`메소드를 사용하여 작성해줍니다.

---

이렇게 해서 MongoDB가 무엇인지 express와 연결 방법 및 MongoDB에 대한 기본 CRUD 연산에 대해 알아보았습니다. MongoDB로 요소 삽입, 탐색, 갱신, 삭제하는 방법과 MongoDB에 접속하는 방법을 살펴보았죠. 간단하게 MongoDB에 대해서 경험할 수 있는 시간이었습니다❗️

긴 글 읽어 주셔서 감사합니다🙆
