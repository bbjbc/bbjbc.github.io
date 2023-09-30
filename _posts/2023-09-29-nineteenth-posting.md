---
title: "[Graphql] GraphQL의 이해"
categories: [graphql, node]
tags: [express, graphql, node]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열아홉 번째 포스팅

안녕하세요! 열아홉 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

올해도 추석이 찾아왔습니다. 가을 햇살처럼 풍요롭고 여유로운 마음으로 감사 넘치는 한가위 되셨으면 좋겠습니다🙏<br>
**보름달보다 넉넉한 한가위 되십쇼😆**

오늘의 포스팅 내용은 **GraphQL**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ GraphQL이란?

## ⭕️ REST API vs. GraphQL

REST API는 무상태로 클라이언트와 독립되어 데이터를 교환하는 API였습니다. 클라이언트를 고려하지 않으며 오직 요청을 받고 데이터를 분석 후 JSON 데이터와 함께 응답을 반환했습니다.

GraphQL API도 크게 다른 건 없습니다. 무상태이며 클라이언트와 독립되어 데이터를 교환하는 API입니다. **하지만 쿼리의 유연성이 높다는 것입니다.**<br> 하지만 복잡한 쿼리가 서버에 부하를 줄 수 있으며 고려해야 할 점도 있습니다.

> **REST API의 제한점?** <br>
>
> - 클라이언트 측에서 필요한 데이터가 각자 다르다면 더욱 많은 엔드포인트를 사용하면 됩니다. 같은 엔드포인트를 사용하면서 분석이나 필터링을 통해 프론트엔드에 필요한 데이터만 가질 수도 있지만 필요 이상으로 많은 데이터를 보내면 특히 모바일 기기를 다룰 때 문제가 생길 수 있습니다. <br> **엔드포인트가 너무 많으면 지속적으로 업데이트하는 데에 어려움이 있을 수 있어 유연성이 떨어집니다**
> - 쿼리 매개변수를 사용하는 것? 위 방법과 비슷하게 백엔드 개발자가 추가해야 프론트엔드가 재개할 수 있는 종속성이 있습니다. 또한 어떤 쿼리 매개변수에 어떤 값을 설정하는지 명확하지 않으면 API를 이해하기 어렵기 때문에 유연성이 떨어지며 이상적인 방법이라 하기 어렵습니다.

여러 페이지에 다양한 데이터 요구사항이 있는 애플리케이션에 이상적인 해결 방법이 **GraphQL** 입니다.<br>
위의 문제점이 없습니다. GraphQL에는 풍부한 쿼리 언어가 존재하여 FE에서 BE로 분석하며 필요한 데이터를 검색 가능합니다. SQL이나 MongoDB 같은 쿼리 언어가 FE에 있는 것과 다름 없습니다. 이렇게 BE로 보내는 요청에 쿼리 언어를 붙입니다.

GraphQL과 REST API는 각각의 장단점이 있으며, 프로젝트의 특성과 요구 사항에 따라 선택하는게 맞으며 팀에 무엇이 더 좋을지 고려하는 것이 가장 중요하다고 생각합니다.

<br>

## ⭕️ GraphQL 작동 원리

1. **스키마(Schema)**
   - GraphQL은 데이터의 구조와 타입을 정의하는 스키마를 가집니다.
   - 스키마는 어떤 타입의 데이터를 쿼리할 수 있는지 각 데이터 타입이 어떤 필드를 가지고 있는지 정의합니다.
2. **쿼리(Query)**
   - 클라이언트는 원하는 데이터를 정확하게 명시하는 GraphQL 쿼리를 작성합니다.
   - 쿼리는 클라이언트가 서버에게 요청하는 데이터의 구조를 나타내며, JSON 형식으로 반환됩니다.
3. **리졸버(Resolver)**
   - 서버 측에서는 각 필드의 데이터를 실제로 어떻게 가져올지를 정의하는 리졸버 함수를 작성합니다.
   - 리졸버 함수는 필드에 대한 실제 데이터 소스를 쿼리하며 그 결과를 반환합니다.
4. **실행(Execution)**
   - 클라이언트가 보낸 GraphQL 쿼리는 서버에 도착하면 실행됩니다.
   - 서버는 쿼리를 분석하고 각 필드의 리졸버 함수를 호출하여 데이터를 수집합니다.
5. **변수(Variables)**
   - 쿼리에서 동적인 값 또는 파라미터를 사용하려면 변수를 정의하여 쿼리에 전달합니다.
   - 변수를 사용하면 동일 쿼리를 재사용하고, 보안과 성능을 향상시킬 수 있습니다.
6. **서브스크립션(Subscriptions)**
   - 실시간 데이터 업데이트를 지원하기 위한 Subscriptions 메커니즘을 제공합니다.
   - 클라이언트는 특정 이벤트에 대한 서브스크립션을 등록하고 이벤트가 발생할 때마다 실시간으로 업데이트를 받을 수 있습니다.
7. **뮤테이션(mutation)**
   - 데이터를 CRUD 작업에 사용하며 변경 가능한 동작을 정의하고, 클라이언트가 서버에게 데이터 변경을 요청하는 방법을 제공합니다.

GraphQL의 핵심 아이디어는 클라이언트가 필요로 하는 데이터를 정확하게 요청하고, 서버는 해당 요청을 처리하기 위한 데이터 소스를 지정하는 것입니다. 이를 통해 클라이언트와 서버 간의 효율적인 데이터 통신이 가능합니다.

---

# 2️⃣ GraphQL 적용

## ✔️ 종속성 추가

```js
npm install --save graphql express-graphql
```

**graphql** : GraphQL 서비스의 스키마를 정의하는 데 필요한 패키지로 쿼리, 뮤테이션 등의 정의를 허용합니다.<br>
**express-graphql** : 서버가 들어오는 요청 등을 분석합니다.

<br>

## ✔️ schema, resolve 정의

[schema.js]

```js
const { buildSchema } = require("graphql");

module.exports = buildSchema(` 
    type TestData {
        text: String!
        views: Int!
    }

    type RootQuery {
        hello: TestData!
    } 
    
    schema {
        query: RootQuery
    }
`);
```

`buildSchema` 함수는 graphql 및 express-graphql에 의해 분석될 수 있는 스키마를 구축할 수 있게 합니다.<br>
여기서 query는 실질적으로 데이터를 받는 부분으로 허용하고자 하는 모든 쿼리를 의미합니다. <br>
쿼리 이름을 붙이고 그 다음에 유형을 적어줍니다. `!`는 필수적으로 반환해야 한다는 것을 나타냅니다.

[resolvers.js]

```js
module.exports = {
  hello() {
    return {
      text: "Hello World!",
      views: 1245,
    };
  },
};
```

resolver는 내보낸 객체로 hello 함수 및 메서드를 필요로 합니다. 즉 스키마에 정의된 각 쿼리나 뮤테이션에 메서드가 필요합니다. 정의한 이름과 일치하는 이름을 입력해야 합니다!

이제 유효한 리졸버와 스키마가 있으니 express-graphql 패키지를 통해 공개해야 합니다.

[app.js]

```js
const { graphqlHTTP } = require("express-graphql");

const graphqlSchema = require("./graphql/schema");
const graphqlResolver = require("./graphql/resolvers");

app.use(
  "/graphql",
  graphqlHTTP({
    schema: graphqlSchema,
    rootValue: graphqlResolver,
  })
);
```

graphqlHttp를 임포트한 후 미들웨어를 추가해줍니다. graphql은 엔드포인트를 /graphql로 설정합니다. 원하는 대로 바꿀 수 있지만 이것을 사용합니다.

이렇게 설정 후 postman을 통해 확인합시다. <br>

![graphql2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/004a41d9-c90c-49b1-a649-bbe1be02211e)<br>

POST요청을 통해 'localhost:8080/graphql'로 요청을 보냅니다. 요청의 본문에는 쿼리를 설명하는 JSON 데이터를 넣습니다. 중괄호 안에 쿼리를 작성해주며 큰 따옴표 안에 graphql 쿼리 표현을 넣어줍니다. <br>

![graphql3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/5c330cd7-1d25-4d18-b4d9-b57075745111)<br>

send를 보내면 위와 같은 데이터가 나타나게 됩니다.

하나의 엔드포인트지만 FE에 받고자 하는 데이터를 정의하며 FE에서 데이터를 필터링하는 것이 아니라 express-graphql에 의해 서버에서 필터링을 하는 것입니다.<br> 이렇게 서버가 작업을 수행하지만 스키마와 리졸버만 정의하면 됩니다.

<br>

## ✔️ mutation 정의

```js
const { buildSchema } = require("graphql");

module.exports = buildSchema(` 
    type Post {
        _id: ID!
        title: String!
        content: String!
        imageUrl: String!
        creator: User!
        createdAt: String!
        updatedAt: String!
    }

    type User {
        _id: ID!
        name: String!
        email: String!
        password: String
        status: String!
        posts: [Post!]!
    }

    input UserInputData {
        email: String!
        name: String!
        password: String!
    }

    type RootQuery {
        hello: String
    }

    type RootMutation {
        createUser(userInput: UserInputData): User!
    }

    schema {
        query: RootQuery
        mutation: RootMutation
    }
`);
```

원래 만들었던 model을 기반으로 뮤테이션 스키마를 작성합니다. `UserInputData`에는 입력값 즉 인수로 사용된 데이터를 위한 특별한 키워드인 input 키워드를 사용합니다. ID 유형은 GraphQL이 제공하는 특별한 유형으로 ID로 취급한다는 뜻입니다.

<br>

## ✔️ resolver 사용 및 mutation 테스트

```js
const bcrypt = require("bcryptjs");

const User = require("../models/user");

module.exports = {
  createUser: async function ({ userInput }, req) {
    //   const email = args.userInput.email;
    const existingUser = await User.findOne({ email: userInput.email });
    if (existingUser) {
      const error = new Error("User exists already!");
      throw error;
    }
    const hashedPw = await bcrypt.hash(userInput.password, 12);
    const user = new User({
      email: userInput.email,
      name: userInput.name,
      password: hashedPw,
    });
    const createdUser = await user.save();
    return { ...createdUser._doc, _id: createdUser._id.toString() };
  },
};
```

리졸버는 REST API에서 controller와 같다고 보면 됩니다. controller에 있던 createUser핸들러를 그대로 가져온 것이라고 보면 됩니다. <br>
`mutation`으로 사용했던 `RootMutation` 안에 있는 `creatUser`함수를 사용합니다. 이 안에 있던 인수인 `userInput` 를 구조 분해를 통해 함수 인수에 객체로 받을 수 있습니다. <br>
또한 리졸버에 프로미스를 반환하지 않으면 GraphQL은 결과를 기다리지 않습니다.

이 mutation을 테스트하기 위해서 <br>
[app.js]

```js
app.use(
  "/graphql",
  graphqlHTTP({
    schema: graphqlSchema,
    rootValue: graphqlResolver,
    graphiql: true,
  })
);
```

graphiql을 true로 설정하는 것을 추가해줍니다. 특별한 툴을 사용할 수 있게 합니다. 'localhost:8080/graphql'을 방문하면 새로운 브라우저가 보일 것입니다. <br>

![graphql4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/ae04a09b-3f1e-48d4-88b1-406d2d0295ed)<br>

위와 같은 화면이 보여지며 GraphQL 작업을 할 수 있습니다. 오른쪽 Documentation Explorer를 보면 어떤 뮤테이션이 있고 어떤 쿼리가 있으며 어떤 데이터를 보내야 하는지 나옵니다. 이에 그치지 않고 왼편에는 데이터를 보낼 수 있습니다.<br>

![graphql5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/92e6aa4a-609b-4d21-b368-9e4df104c0a9)<br>

email, name, password 입력 후 이 쿼리가 완료되면 반환할 데이터를 정의하면 됩니다. 그 후 재생하면 오른쪽과 같은 결과를 맞이할 수 있습니다.<br>

![graphql6](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/95de5361-db24-456d-ad08-ae21dd54d569)<br>

또한 MongoDB Compass 에서도 저장된 것을 확인할 수 있습니다.

<br>

## ✔️ 프론트엔드에 로직 추가

```js
const graphqlQuery = {
      query: `
        mutation {
          createUser(userInput: {
            email: "${email.value}",
            name: "${name.value}",
            password: "${password.value}"
          }) {
            _id
            email
          }
        }
      `,
    };
    fetch("http://localhost:8080/graphql", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(graphqlQuery),
    })
      .then((res) => {
        return res.json();
      })
      .catch(err)=>{
        console.log(err);
      }
```

fetch할 때 원래 라우트의 요청을 가져왔었지만 GraphQL에서는 모든 API 요청이 /graphql만 사용하기 때문에 위와 같은 url을 사용하며 POST 요청이어야 합니다. <br>
또한 쿼리를 사용하여 body를 채우는데 body에는 아까 graphiql에서 사용한 쿼리를 가져와 사용하면 됩니다. 실행하려는 뮤테이션은 createUser입니다.

POST 요청을 유효한 GraphQL 쿼리 표현식과 함께 전송해 프론트엔드와 백엔드를 연결하는 방법은 이렇습니다.

<br>

## ✔️ GraphQL 로직 최적화

```js
const graphqlQuery = {
  query: `
        mutation {
          createUser(userInput: {
            email: "${email.value}",
            name: "${name.value}",
            password: "${password.value}"
          }) {
            _id
            email
          }
        }
      `,
};
```

위 프론트엔드에 graphql 로직을 추가했었습니다.

GraphQL 쿼리에 동적 값을 입력할 때마다 $와 {}를 사용해 문자열 리터럴에 값을 채우고 있습니다. 하지만 변수를 쿼리에 추가하는 것은 좋은 방식이 아닙니다.

```js
const graphqlQuery = {
  query: `
        mutation CreateNewUser($email: String!, $name: String!, $password: String!) {
          createUser(userInput: {
            email: $email, 
            name: $name,
            password: $password
          }) {
            _id
            email
          }
        }
      `,
  variables: {
    email: email.value,
    name: name.value,
    password: password.value,
  },
};
```

mutation이나 query 둘 다 방법은 동일합니다. mutation으로 예를 들면 mutation 키워드 다음에 원하는 이름을 작성 후 소괄호에 $기호를 사용하며 value값에는 변수타입을 작성합니다. **변수타입은 백엔드 코드 스키마에서 작성했던 변수타입을 그대로 작성해야 오류가 생기지 않습니다.**

이것은 GraphQL 구문으로 서버에서 분석됩니다. GraphQL 서버에 내부 변수를 사용하는 쿼리가 있음을 알려줍니다.<br>
선언한 타입변수의 키값의 동적 변수를 ${}자리에 넣어줍니다. <br>
그리고 두번째 인자로 `variables`를 받고 이 곳에 ${}자리에 사용했던 value를 넣어줍니다.

이해가 되셨나요❓ 이것이 동적 값을 쿼리에 입력하는 데에 추천되는 방법입니다.

---

|                                            GraphQL 핵심 개념                                             |
| :------------------------------------------------------------------------------------------------------: |
|                                      무상태, 클라이언트 독립적 API                                       |
|              클라이언트에 노출되는 사용자 정의 쿼리 언어로 인해 REST API보다 더 높은 유연성              |
|   쿼리(GET), 뮤테이션(POST, PUT, PATCH, DELETE) 및 subscription을 사용하여 데이터를 교환하고 관리 가능   |
|                       모든 GraphQL 요청은 하나의 엔드포인트(POST /graphql)로 전달                        |
| 서버는 들어오는 쿼리 표현식(일반적으로 서드파티 패키지에 의해 수행)을 구문 분석하고 적절한 확인자를 호출 |
|                            GraphQL은 React.js 애플리케이션에만 국한되지 않음                             |

데이터와 관련해 좀 더 유연할 수 있는 GraphQL의 성능이었습니다.

오늘도 포스팅 읽어 주셔서 감사합니다💖
