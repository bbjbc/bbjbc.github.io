---
title: "[Algorithm] 삼각 달팽이"
categories: [algorithm]
tags: [algorithm, programmers, Lv2, 월간 코드 챌린지 시즌1]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 서른 네 번째 포스팅

안녕하세요! 서른 네 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **프로그래머스 - 삼각 달팽이**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 삼각 달팽이 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/68645)

## 💨 **문제 설명**

정수 n이 매개변수로 주어집니다. 다음 그림과 같이 밑변의 길이와 높이가 n인 삼각형에서 맨 위 꼭짓점부터 반시계 방향으로 달팽이 채우기를 진행한 후, 첫 행부터 마지막 행까지 모두 순서대로 합친 새로운 배열을 return 하도록 solution 함수를 완성해주세요. <br>

![snail](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/9619d7a1-305c-4287-8ac7-fa7ed56d1e74) <br>

## 💨 **제한사항**

- n은 1 이상 1,000 이하입니다.

## 💨 **입출력 예**

|  n  |                         result                          |
| :-: | :-----------------------------------------------------: |
|  4  |                 [1,2,9,3,10,8,4,5,6,7]                  |
|  5  |          [1,2,12,3,13,11,4,14,15,10,5,6,7,8,9]          |
|  6  | [1,2,15,3,16,14,4,17,21,13,5,18,19,20,12,6,7,8,9,10,11] |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

- 문제 예시와 같습니다.

**입출력 예 #2** <br>

- 문제 예시와 같습니다.

**입출력 예 #3** <br>

- 문제 예시와 같습니다.

---

# 2️⃣ 문제 풀이

## 🔥 문제 풀이

```md
0
00
000
0000

[0][0]
[1][0] [1][1]
[2][0] [2][1] [2][2]
[3][0] [3][1] [3][2] [3][3]
```

> 1. 반복문을 maxNumber까지 순회함.
> 2. maxNumber는 규칙을 찾아야 하는데 테스트 케이스를 계속 보다보면 `(n*(n+1))/2` 의 규칙성이 보임.
> 3. 달팽이 모양인 반시계 방향으로 숫자가 증가해야 해서 `아래로 이동`, `오른쪽으로 이동`, `대각 왼쪽 위로 이동`의 3가지 경우로 나눔.
> 4. 위와 같이 배열 그림을 그려서 인덱스를 생각해줌.
> 5. `아래로 이동` - row가 늘어나면서 a++ <br> `오른쪽으로 이동` - col이 늘어나면서 a++ <br> `대각 왼쪽 위로 이동` - row와 col이 감소하면서 a++ <br>

```js
function solution(n) {
  const result = new Array(n).fill().map((_, i) => new Array(i + 1).fill(0));
  let a = 1,
    row = 0,
    col = 0;
  const maxNumber = (n * (n + 1)) / 2;

  while (a <= maxNumber) {
    // 아래로 이동
    while (row < n && result[row][col] === 0) {
      result[row][col] = a++;
      row++;
    }
    row--;
    col++;

    // 오른쪽으로 이동
    while (col < result[row].length && result[row][col] === 0) {
      result[row][col] = a++;
      col++;
    }
    col -= 2;
    row--;

    // 대각 왼쪽 위로 이동
    while (row >= 0 && col >= 0 && result[row][col] === 0) {
      result[row][col] = a++;
      row--;
      col--;
    }
    row += 2;
    col++;
  }
  return [].concat(...result);
}
```

처음에 어떻게 풀 지 고민하다가 먼저 문제에 나온 모양과 비슷하게 배열을 생성한다. 그 배열에 있는 index에 따라 `a`를 증가 시켜주면 되는데 `a`가 `1`씩 증가하므로 달팽이 모양 (반시계 방향)으로 문제를 해결해야 한다고 생각했다.

그러려면 초기 상태에서 n보다 작을 때까지 `아래로 이동`을 해주고 `오른쪽으로 이동`을 해준다. 코드에서 볼 수 있듯이 `result[row].length`까지 반복을 해야 이상한 오류를 방지할 수 있다. 그 후 `대각 왼쪽 위로 이동`을 차근차근 해주면 된다.

각 반복문이 끝날 때마다 `row`와 `col` 인덱스가 조정이 되는데 내가 짠 코드 상 반복문 안에서 인덱스가 후위 증가 되어서 어쩔 수 없이 인덱스를 조정해야 했다.

> **그러나❗️**

![snail1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/fb16d068-8a66-4f6d-88f8-558b09699ccc) <br>

문제를 보고 알바에 가야 해서 어떻게 풀 지 생각하다가 코드를 짜본 것이다. 막상 코드는 작동하지 않았고 오류만 떴다.

![snail2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/e5a0c2b5-b80c-4942-9937-84a912e7070b) <br>

이런 내용의 오류인데 뭐가 문제인지 계속 파악이 되지 않았다.

> **결국❗️❗️ 알아냈다❗️❗️**

알아내긴 했겠지. 풀었으니까. <br>
뭐냐면 while문 안의 조건 순서가 잘못 됐었다. 나는 `&&` 이러한 논리 연산자가 있으면 솔직히 순서는 상관이 없는 줄 알았다.

하지만 생각해보니 while문에 들어가기 위한 조건을 작성하는 건데 조건에 맞지 않거나 오류를 발생시키는 원인이 있다면 에러가 발생할 것이다.

인덱스를 증가 시키고 다시 while문의 조건으로 들어가는데 `result[row][col] === 0`가 `row < n`를 선행할 수가 없었다. 인덱스는 이미 커져 있고 존재하지도 않는 인덱스를 배열에 있는 지 확인을 하려 했으니 에러가 발생하는 것이 당연했다.

이렇게 해결하다 보면 배열에는 `[ [ 1 ], [ 2, 9 ], [ 3, 10, 8 ], [ 4, 5, 6, 7 ] ]` 이런식으로 중첩 배열이 존재하게 된다.

중첩 배열을 없애기 위해서는 여러 방법이 존재한다. 예전에도 이런 문제가 있었는데 블로그에는 처음 쓰는 것이니 소개를 해보겠다.

---

# 3️⃣ 중첩 배열 → 1차원 배열

[**[apost.dev님의 블로그]**](https://apost.dev/1192/) 를 참조해서 작성했다.

문제를 풀다 보면 요구하는 result 값이 중첩배열이 아닌 1차원 배열로 변환해야 하는 문제가 다수 존재하며 아마 대부분이 그것을 요구할 것이다.

이 때, 우리는 JavaScript의 배열 내장 함수를 통해 해결할 수 있다.

[**[Array.prototype.reduce()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 과 [**[Array.prototype.concat()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)이다.

자세한 내용은 위 링크를 따라서 참조하고 공부하길 바란다.

## ✔️ **reduce() + concat()**

누적 결과를 반환하는 `reduce()` 메서드로 이전 결과에 현재 요소를 `concat()` 메서드를 통해 하나의 배열로 합칠 수 있다.

```js
let arr = [
  ["cho", "byeong", "chan"],
  ["boongranii", "quokka"],
];

let newArr = arr.reduce((prev, next) => {
  return prev.concat(next);
});

console.log(newArr); // ['cho', 'byeong', 'chan', 'boongranii', 'quokka']
```

`reduce()`를 함께 사용하는 것은 후처리를 추가로 할 수 있기 때문에 단순히 배열을 합치는 용도만으로 사용되지 않고 합칠 용도로 사용할 것이면 위 방법은 추천하지 않는다.

## ✔️ **스프레드 연산자 + concat()**

내가 이 문제를 풀 때 사용한 방법이다.

중첩 배열을 스프레드 연산자로 펼친 후 빈 배열에 `concat()` 메서드를 사용해서 하나로 합치는 쉬운 방법이다. 중첩 배열에서만 사용할 수 있는 단점이 있지만, 속도가 빠르기 때문에 가장 추천하는 방법이다.

```js
let arr = [
  ["cho", "byeong", "chan"],
  ["boongranii", "quokka"],
];

let newArr = [].concat(...arr);

console.log(newArr); // ['cho', 'byeong', 'chan', 'boongranii', 'quokka']
```

## ✔️ **flat()**

[**[Array.prototype.flat()]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) 참고 바람.

2중첩 이상의 배열을 1차원 배열로 변환해 주는 내장 메서드이다. 메서드 안 인자를 n으로 설정하면 n차원 배열까지 1차원 배열로 변환해 준다.

앞의 방법들과는 달리 중첩 깊이에 대한 제한도 없고, 단일 메서드로 단순하게 배열을 변환할 수 있어서 엄청 편리하다. 하지만, **치명적인 단점인 속도가 굉장히 느리다**는 것이다.

스프레드 연산자와 `concat()`을 사용하는 방법보다 평균적으로 1.5~2배 정도 느린 것으로 알려진다.

```js
let arr = [
  ["cho", "byeong", "chan"],
  ["boongranii", "quokka"],
];

let newArr = arr.flat();

console.log(newArr); // ['cho', 'byeong', 'chan', 'boongranii', 'quokka']
```

웹 브라우저에 따라 결과가 다소 다르긴 하지만, 배열의 크기가 아주 크고 다중첩 배열이 아닌 경우에는 이 메서드를 사용하기 보다는 `스프레드 연산자 + concat()`을 사용하길 바란다.

---

# 4️⃣ 느낀점

문제가 짧고 단순했지만 막상 어떻게 해결해야 할 지 생각을 오래 했던 것 같다. 달팽이 모양대로 카운트 증가 시켜주면 되지만 이게 말이 쉽지 막상 짜보려니 어려웠다고 생각한다.

이런 사소하고 멍청한 실수를 했지만 조건을 만족하려면 순서에도 제대로 신경을 써야 한다는 것을 느꼈다.

이것만이 끝이 아닌 중첩 배열을 어떻게 1차원 배열로 풀어 나갈지도 중요한 요소가 되었다. 인덱스에 대한 이해와 정확한 반복문, 스프레드 연산자에 대한 지식이 있어야 해결할 수 있던 문제라고 생각한다. **그럼 안녕핑🐌**

~~요즘 MZ들은 어쩔티비, ~~티비 말고 ~~핑을 사용한다고 한다. 그래서 핑 쓰는 거니 이해 바란다.~~
