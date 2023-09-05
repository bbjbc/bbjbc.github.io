---
title: "[JavaScript] 비동기 요청의 이해"
categories: [javascript]
tags: [express, javascript, asynchronous]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 열여섯 번째 포스팅

안녕하세요! 열여섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

개강해서 그런지 포스팅을 못했네요,, 는 핑계겠죠 :(💩 <br>
아직도 수강신청을 제대로 못했다는게 어이가 없네요~ 최종정정으로 18학점 만듭니다 꼭👍 <br>

오늘의 포스팅 내용은 **비동기 요청**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

Udemy의 NodeJS-The Complete Guide (incl MVC, REST APIs, GraphQL, Deno) 강의를 바탕으로 작성된 글입니다.

---

# 1️⃣ 비동기 요청이란?

지금까지 다루었던 것은 특정한 유형의 요청과 응답만을 다뤘습니다. **요청**은 어떤 양식을 채우거나 URL을 입력하면 브라우저가 보내는 요청이었구요~ **응답**은 리다이렉트나 새 HTML 페이지를 반환했어요!<br>
이렇게 해도 많은 작업이 가능하지만 어떤 요청은 반드시 배후에서 일어날 수도 있습니다.

> 클라이언트와 서버 즉 브라우저와 노드 애플리케이션이 있는 설정을 사용하고 있습니다. 바로 BE와 FE입니다. 보통 클라이언트가 서버로 요청을 보내고 서버는 클라이언트로 응답을 해줍니다. 위에서 말했듯이 우리는 응답을 HTML 페이지나 반환을 위한 리다이렉트하는 라우트를 사용했어요.<br>
> 여기서 문제가 발생하진 않지만 항목을 제거하는 경우 페이지를 새로 고침하지 않습니다. 새 페이지를 로딩하지 않고 현재 페이지를 빠르게 바꾸는 것입니다.

> **비동기식 요청**은 JSON이라는 특별한 형식의 데이터를 포함하는 요청을 의미합니다. 데이터는 특정 URL이나 라우트로 서버에 보내진 후 허용됩니다. 여기서 서버는 데이터를 가지고 작업 후 응답을 해주는데 응답 또한 배후에서 반환됩니다. <br>
> 즉, 응답으로 새로운 HTML 페이지를 렌더링하지 않는 것입니다. 보통 JSON 형식의 데이터가 돌아옵니다. **이처럼 새로고침 및 페이지 구축으로 새 HTML 페이지를 반환하지 않고도 클라이언트 측 서버가 JS와 서버 측 논리와 통신 가능합니다.**

이렇게 하면 배후에서 작업이 진행되어 유저플로우를 방해하거나 페이지를 새로 고침할 필요가 없어집니다❗️

---

# 2️⃣ 클라이언트 측 논리 추가

![비동기](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/bf99bb5d-f099-4c7c-9272-a390af615648)<br>

이렇게 현재 저 Delete 버튼을 클릭하면 서버에 요청이 보내져서 해당 제품이 사라진 새로운 버전의 페이지가 나타납니다.

```js
exports.postDeleteProduct = (req, res, next) => {
  const prodId = req.body.productId;
  Product.findById(prodId)
    .then((product) => {
      if (!product) {
        return next(new Error("Product not found."));
      }
      fileHelper.deleteFile(product.imageUrl);
      return Product.deleteOne({ _id: prodId, userId: req.user._id });
    })
    .then(() => {
      console.log("DESTROYED PRODUCT");
      res.redirect("/admin/products");
    })
    .catch((err) => {
      const error = new Error(err);
      error.httpStatusCode = 500;
      return next(error);
    });
};
```

현재는 위와 같이 제품에 해당하는 이미지와 제품을 삭제 후 리다이렉트를해서 새로운 HTML 페이지를 반환합니다.

이것을 Delete 버튼을 눌러 배후 서버에 정보를 보내면 서버의 작업을 거쳐 응답으로 JSON 데이터를 보낼 것입니다. 브라우저가 메시지를 받으면 해당 DOM 요소를 삭제하며 새로운 HTML으로 리다이렉트 되지 않고 DOM 요소만 없어지도록 할 것입니다. 이것이 **비동기식 JS 요청**입니다.<br>
로직은 거의 변화가 없고 라우트 방식이 바뀝니다.

public 폴더에 새로운 js파일을 만들 것입니다. 이것은 브라우저에 실행되는 JS 코드입니다. 제품을 불러오는 html코드 맨 아래에 script를 임포트 해줍니다.

```html
<script src="/js/admin.js"></script>
```

이제 이 파일이 제품 ejs 파일에 실행되며 전체 DOM이 분석될 것입니다.

```html
<form action="/admin/delete-product" method="POST">
  <input type="hidden" value="<%= product._id %>" name="productId" />
  <input type="hidden" name="_csrf" value="<%= csrfToken %>" />
  <button class="btn" type="submit">Delete</button>
</form>
```

위와 같이 서버에 요청하는 폼을 아래와 같이 바꿔줍니다.

```html
<input type="hidden" value="<%= product._id %>" name="productId" />
<input type="hidden" name="_csrf" value="<%= csrfToken %>" />
<button class="btn" type="button" onClick="deleteProduct(this)">Delete</button>
```

type이 submit이 아니라 button이어야 합니다.
`deleteProduct()`는 `admin.js`에서 사용되는 함수명입니다. this로 받아 클릭한 요소인 버튼을 나타냅니다. (아래 참고)

이 Delete 버튼을 통해 `productId`와 `csrfToken`에 접근하기 위해 `admin.js`에 로직을 작성합니다.

```js
const deleteProduct = (btn) => {
  const prodId = btn.parentNode.querySelector("[name=productId]").value;
  const csrf = btn.parentNode.querySelector("[name=_csrf]").value;
};
```

버튼에 접근 가능하니 `productId`와 `csrfToken`에 접근을 위한 코드입니다. <br>

![비동기2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/1969b27a-a14d-4b92-b558-0baa20fec1f2)<br>

btn.parentNode는 버튼 주변의 요소이며 여기서는 div를 가리킵니다. 위 사진에 값이 나와 있는데 이것은 querySelector를 사용해 추출할 수 있습니다.

이 두 가지 값을 통해 서버에 비동기식 요청을 보낼 수 있습니다.

---

# 3️⃣ 백그라운드 요청 전송

이제 백엔드에 JS 요청을 보내도록 라우트를 추가해야 합니다.

```js
router.post("/delete-product", isAuth, adminController.postDeleteProduct);
```

위를 JavaScript를 통해 직접 요청을 보내기 때문에 HTTP 동사를 사용 가능합니다. 여기에서는 삭제를 해야하기 때문에 `delete`를 써줍니다. HTTP 메서드의 일종으로 삭제를 위해 사용합니다.

```js
router.delete("/product/:productId", isAuth, adminController.deleteProduct);
```

위와 같이 동적 매개변수를 가지면 URL로부터 해당 정보를 추출 가능합니다.

라우트가 변경됨에 따라 핸들러 함수도 변경되어야 합니다.
위에 `postDeleteProduct()`를 변경합니다. 라우트에서 동적 매개변수를 사용하여 `productId`에 접근하였기에 `req.params.productId`로 바꿔줍니다.

반환 응답을 변경해야 하는데 리다이렉트는 새로운 페이지가 반환될 것이므로 json 데이터를 보내도록 해야합니다. 지금 하는 것은 배후에 요청이 보내지는 것이기 때문이죠^^

```js
res.status(200).json({ message: "Success!" });
```

JSON 데이터도 디폴트로 상태 코드 200을 받을텐데 이 경우 리다이렉트 등을 통해 자동으로 상태 코드를 받지 않으니 상태 코드를 명확히 입력하고 메시지를 보내줍니다.

```js
res.status(500).json({ message: "Deleting product failed." });
```

오류 핸들러 또한 위와 같이 반환합니다.

```js
exports.deleteProduct = (req, res, next) => {
  const prodId = req.params.productId;
  Product.findById(prodId)
    .then((product) => {
      if (!product) {
        return next(new Error("Product not found."));
      }
      fileHelper.deleteFile(product.imageUrl);
      return Product.deleteOne({ _id: prodId, userId: req.user._id });
    })
    .then(() => {
      console.log("DESTROYED PRODUCT");
      res.status(200).json({ message: "Success!" });
    })
    .catch((err) => {
      res.status(500).json({ message: "Deleting product failed." });
    });
};
```

수정된 핸들러 함수 코드입니다.

이제 `productId`를 추출하고 새 페이지를 렌더링하지 않고 데이터만 반환하기 위한 JSON 응답을 반환하도록 만들었습니다. 이제 응답 데이터를 다루겠습니다.

public 폴더에 있는 클라이언트 측 `admin.js` 파일로부터 요청을 보내야 합니다. 이 때 HTTP 요청을 보낼 때 브라우저가 제공하는 `fetch()` 메소드를 사용할 수 있는데 데이터를 가져올 뿐만 아니라 데이터를 보내는 데에도 사용됩니다.

```js
fetch("/admin/product/" + prodId, {
  method: "DELETE",
  headers: {
    "csrf-token": csrf,
  },
})
  .then((result) => {
    console.log(result);
  })
  .catch((err) => {
    console.log(err);
  });
```

첫 번째 인수에는 전달할 url을 써줍니다. 두 번째 인수에는 fetch 요청을 구성하는 객체를 지정합니다. <br>
`csurf` 패키지가 req.body 뿐 아니라 쿼리 매개변수도 검토하기 때문에 추가가 가능합니다.

위와 같이 진행 후 Delete 버튼을 누르면<br>

![비동기3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/ef2e8de9-9d4a-4515-b303-8a9b1fdfa2c4)<br>

요청 본문과 상태 코드 200을 응답 받았습니다. 페이지에서 제품은 사라지지 않았지만 새로 고침하면 없어집니다. 이 문제는 DOM 조작을 통해 알아보겠습니다.

---

# 4️⃣ DOM 조작

```js
const productElement = btn.closest("article");

fetch("/admin/product/" + prodId, {
  method: "DELETE",
  headers: {
    "csrf-token": csrf,
  },
})
  .then((result) => {
    return result.json();
  })
  .then((data) => {
    console.log(data);
    productElement.parentNode.removeChild(productElement);
  })
  .catch((err) => {
    console.log(err);
  });
```

위에 then 블록 모두 응답으로 서버에서 항목이 삭제될 것이기 때문에 DOM에서도 삭제하도록 해야 합니다. Delete 버튼은 결국 삭제하고자 하는 DOM 요소 안에 있으며 삭제하고자 하는 것은 바로 article 부분입니다. <br>

![비동기4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/5cb9bd44-c92a-45f5-9fdd-bfeaf98de4e1)<br>

`btn.closest()`를 사용합니다.

> [`element.closest()`](https://developer.mozilla.org/ko/docs/Web/API/Element/closest) <br>
> 주어진 css 선택자와 일치하는 요소를 찾을 때까지 자기 자신을 포함해 위쪽(부모 방향, 문서 루트까지)으로 문서 트리를 순회합니다.

`closest()`메소드를 통해 삭제하고자 하는 요소를 가리킵니다. parentNode에 접근해 자식 요소를 삭제하도록 합니다. 이 때 자식 요소가 `productElement`가 되는 것입니다. <br>
이제 Delete 버튼을 누르면 새로운 페이지의 렌더링 없이 삭제 되는 것을 볼 수 있습니다. <br>

![비동기5](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/37b9591d-7913-4c8e-af94-f40bd3c0159a) <br>

또한 아까 설정한 메시지가 콘솔창에 뜨는 것을 확인할 수 있습니다❗️

---

이렇게 해서 비동기식 요청을 사용하는 방법에 대해서 간단하게 알아보았습니다.<br>
클라이언트 측에서 HTTP 요청 없이 바로 렌더링되는 프론트엔드 기술이 있지만 이것은 비동기식 요청을 통해 데이터를 보내고 백엔드에서 처리하는 방법을 배워본 것입니다❗️

오랜만에 올린 포스팅 읽어 주셔서 감사합니다🎶
