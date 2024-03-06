---
title: "[Algorithm] 할인 행사"
categories: [algorithm]
tags: [algorithm, programmers, Lv2]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 서른 여섯 번째 포스팅

안녕하세요! 서른 여섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 할인 행사**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 할인 행사 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/131127)

## 💨 **문제 설명**

XYZ 마트는 일정한 금액을 지불하면 10일 동안 회원 자격을 부여합니다. XYZ 마트에서는 회원을 대상으로 매일 한 가지 제품을 할인하는 행사를 합니다. 할인하는 제품은 하루에 하나씩만 구매할 수 있습니다. 알뜰한 정현이는 자신이 원하는 제품과 수량이 할인하는 날짜와 10일 연속으로 일치할 경우에 맞춰서 회원가입을 하려 합니다.

예를 들어, 정현이가 원하는 제품이 바나나 3개, 사과 2개, 쌀 2개, 돼지고기 2개, 냄비 1개이며, XYZ 마트에서 14일간 회원을 대상으로 할인하는 제품이 날짜 순서대로 치킨, 사과, 사과, 바나나, 쌀, 사과, 돼지고기, 바나나, 돼지고기, 쌀, 냄비, 바나나, 사과, 바나나인 경우에 대해 알아봅시다. 첫째 날부터 열흘 간에는 냄비가 할인하지 않기 때문에 첫째 날에는 회원가입을 하지 않습니다. 둘째 날부터 열흘 간에는 바나나를 원하는 만큼 할인구매할 수 없기 때문에 둘째 날에도 회원가입을 하지 않습니다. 셋째 날, 넷째 날, 다섯째 날부터 각각 열흘은 원하는 제품과 수량이 일치하기 때문에 셋 중 하루에 회원가입을 하려 합니다.

정현이가 원하는 제품을 나타내는 문자열 배열 `want`와 정현이가 원하는 제품의 수량을 나타내는 정수 배열 `number`, XYZ 마트에서 할인하는 제품을 나타내는 문자열 배열 `discount`가 주어졌을 때, 회원등록시 정현이가 원하는 제품을 모두 할인 받을 수 있는 회원등록 날짜의 총 일수를 return 하는 solution 함수를 완성하시오. 가능한 날이 없으면 0을 return 합니다.

## 💨 **제한사항**

- 1 ≤ `want`의 길이 = `number`의 길이 ≤ 10
  - 1 ≤ `number`의 원소 ≤ 10
  - `number[i]`는 `want[i]`의 수량을 의미하며, `number`의 원소의 합은 10입니다.
- 10 ≤ `discount`의 길이 ≤ 100,000
- `want`와 `discount`의 원소들은 알파벳 소문자로 이루어진 문자열입니다.
  - 1 ≤ `want`의 원소의 길이, `discount`의 원소의 길이 ≤ 12

## 💨 **입출력 예**

|                    want                    |     number      |                                                            discount                                                            | result |
| :----------------------------------------: | :-------------: | :----------------------------------------------------------------------------------------------------------------------------: | :----: |
| ["banana", "apple", "rice", "pork", "pot"] | [3, 2, 2, 2, 1] | ["chicken", "apple", "apple", "banana", "rice", "apple", "pork", "banana", "pork", "rice", "pot", "banana", "apple", "banana"] |   3    |
|                 ["apple"]                  |      [10]       |              ["banana", "banana", "banana", "banana", "banana", "banana", "banana", "banana", "banana", "banana"]              |   0    |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

- 문제 예시와 같습니다.

**입출력 예 #2** <br>

- 사과가 할인하는 날이 없으므로 0을 return 합니다.

※ 공지 - 2024년 1월 26일 문제 지문의 오탈자가 수정되었습니다.

---

# 2️⃣ 문제 풀이

## 🔥 문제 풀이

> 1. want 배열과 number 배열을 하나의 객체로 합침. → `wantList`
> 2. 10일동안 진행하니 10개씩 잘라 순회함.
> 3. 본 객체가 아닌 복사 객체를 쓰는 것은 10개씩 자르는 것에 따라 계속 달라지기 때문.
> 4. 복사 객체에 할인 품목이 존재하면 그 품목의 카운트를 하나 낮춰.
> 5. 감소 시키고 존재하지 않는 품목이면 객체에서 속성 자체를 지워 버려.
> 6. 객체에서 키 값이 없다면 10일동안 만족하는 거니까 `totalDay` 올려.

```js
function solution(want, number, discount) {
  let totalDay = 0;
  let wantList = {};

  //  { banana: 3, apple: 2, rice: 2, pork: 2, pot: 1 }
  for (let i = 0; i < want.length; i++) {
    wantList[want[i]] = (wantList[want[i]] || 0) + number[i];
  }

  for (let i = 0; i < discount.length; i++) {
    const discountProduct = discount.slice(i, i + 10);
    const copy = { ...wantList };

    discountProduct.forEach((product) => {
      if (copy[product]) {
        copy[product]--;
      }
      // 감소 시켰다가 없으면 삭제시킴.
      if (!copy[product]) {
        delete copy[product];
      }
    });

    if (Object.keys(copy).length === 0) {
      totalDay++;
    }
  }

  return totalDay;
}
```

**이 문제는 예전에 손 대고 나서 한참을 못 풀었던 문제이다.** 일단 10일로 잘라내고 그 안에 원하는 품목이 10일 동안의 할인 기간에 할인 상품에 존재 하는 지 확인을 해야 했다.

```js
function solution(want, number, discount) {
  let totalDay = 0;
  const wantList = {};
  const discountList = {};

  // wantList: { banana: 3, apple: 2, rice: 2, pork: 2, pot: 1 }
  for (let i = 0; i < want.length; i++) {
    wantList[want[i]] = (wantList[want[i]] || 0) + number[i];
  }

  // discountList: { chicken: 1, apple: 4, banana: 4, rice: 2, pork: 2, pot: 1 }
  for (let i = 0; i < discount.length; i++) {
    discountList[discount[i]] = (discountList[discount[i]] || 0) + 1;
  }

  for (let i = 0; i < discount.length; i++) {
    const product = discount[i];

    // 원하는 제품이 할인되고 수량이 충분한 경우
    if (wantList[product] && wantList[product] <= discountList[product]) {
      // 할인되는 날짜 수 증가
      totalDay++;

      // 원하는 제품의 수량 감소
      wantList[product]--;

      // 모든 원하는 제품의 수량이 0이 되었다면
      if (Object.values(wantList).every((value) => value === 0)) {
        break;
      }
    }
  }

  return totalDay;
}
```

그 동안 통과가 되지 않고 썩혔던 코드이다. 오직 개수에만 집착해서 수량이 있는 경우에 날짜 수를 증가 시켰다. 할인 품목은 개수를 파악할 필요 없이 10개씩 잘라내서 순회하는 게 맞다고 생각했다.

저 코드는 각 할인 날짜에 대해 해당 날에 할인을 받을 수 있는 상품인지 단 한 번씩만 확인하기 때문에 통과가 될 리가 없었다. 너무 복잡하게 생각했던 것 같기도 하다. 나는 바보다.

아 그리고 문제를 풀다가 안풀리는 부분이 있어서 질문하기도 찾아봤는데 `delete`라는 연산자가 존재하더라. 나는 `delete()` 메서드를 생각하고 [**[Set.prototype.delete()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set/delete) 이것으로 써 보려고 애를 썼지만 절대 되지 않았다.

[**[delete operator]**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete) 이러한 연산자가 존재했다. 자세한 내용은 참고 바란다. 무슨 명령어마냥 사용이 가능하다.

> **delete operator**

`delete` 연산자는 객체의 속성을 삭제할 때 사용한다. 객체의 특정 속성을 삭제할 수 있다. <br> `delete obj['propertyName']` 이와 같이 사용할 수 있다.

> **delete()**

`delete()` 메서드는 `Set` 객체에서 특정 요소를 삭제할 때 사용된다. `Set` 객체에서는 특정 요소를 직접 지정해서 삭제할 수 있다. `set.delete(element)` 이와 같이 사용할 수 있다.

난 아래로 계속 이것 저것 해보았는데 안되길래 공식 문서를 보았고 이건 아닌 것 같아서 봤는데 연산자를 사용하면 됐었다. 정말 신기한 발견이었다❗️

---

# 3️⃣ 느낀점

문제를 풀수록 몰랐던 것들을 알게 된다. 원래는 찾아보려 하지도 않던 것들인데 문제에 적용 시키기 위해서는 찾아보게 되며 공부도 되는 것 같다.

물론 이것을 계속 찾지만 말고 내 머리에 적응이 되도록 계속 써먹어야 한다는 것이다. 꾸준히 풀다 보면 뭐 적응되고 잘 적용도 시킬 수 있겠지. **그럼 안녕핑🐌**
