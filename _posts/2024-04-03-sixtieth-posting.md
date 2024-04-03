---
title: "[Cryptography] RSA 암호문 복호화하기"
categories: [cryptography]
tags: [cryptography, encrypt, rsa, algorithm, bigint]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 예순 번째 포스팅

안녕하세요! 예순 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 60❗️💜

오늘의 포스팅 내용은 **Cryptography - RSA 암호문 복호화하기**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제 설명

## 💨 **문제 설명**

Alice는 Bob에게 RSA로 암호화된 메시지를 보내려고 합니다.

> **Bob의 Public Key**
>
> - N = 3174654383
> - e = 65537

> **암호화된 Message**
>
> - C = 2487688703

> **공격해서 얻어내야 하는 정보**
>
> - plaintext & private key d

---

# 2️⃣ 문제 풀이

## 🔥 문제 해결 풀이

> 1. RSA 암호화
> 2. N의 약수를 찾아서 p, q를 구하고, phi를 구한다.
> 3. phi 값을 구했으면 ed = 1 mod phi → ed = 1 mod (p-1)(q-1)을 만족하는 d를 구한다.
> 4. d 값을 구했으니 M = C^d mod N을 계산한다.
> 5. M을 16진수로 변환하고, ASCII로 변환한다.

```js
function RSA() {
  const N = BigInt(3174654383);
  const e = BigInt(65537);
  const C = BigInt(2487688703);
  let p = null;
  let q = null;
  let phi = null; // (p-1)(q-1)
  let d = null;
  let M = null;

  // N의 약수를 찾아서 p, q를 구하고, phi를 구한다.
  for (let i = BigInt(2); i * i <= N; i++) {
    if (N % i === BigInt(0)) {
      p = i;
      q = N / p;
      phi = (p - BigInt(1)) * (q - BigInt(1));
      console.log(`p=${p}, q=${q}, phi=${phi}`);
      break;
    }
  }

  // ed = 1 mod phi → ed = 1 mod (p-1)(q-1)
  // d = e^-1 mod phi
  d = modInverse(e, phi);
  console.log(`d=${d}`);

  // M = C^d mod N
  M = modPow(C, d, N);
  console.log(`M=${M}`);

  M = toHex(M); // M을 16진수로 변환
  console.log(fromHexToASCII(M)); // ASCII로 변환
}

// 10진수를 16진수로 변환
function toHex(str) {
  return str.toString(16);
}

// 16진수를 ASCII로 변환
function fromHexToASCII(str) {
  let result = "";
  for (let i = 0; i < str.length; i += 2) {
    result += String.fromCharCode(parseInt(str.substr(i, 2), 16));
  }
  return result;
}

// BigInt를 이용한 모듈러 연산
function modPow(base, exp, mod) {
  if (mod === BigInt(1)) return BigInt(0);
  let result = BigInt(1);
  base = base % mod;
  while (exp > 0) {
    if (exp % BigInt(2) === BigInt(1)) {
      result = (result * base) % mod;
    }
    exp = exp / BigInt(2);
    base = (base * base) % mod;
  }
  return result;
}

// BigInt를 이용한 모듈러 역원
function modInverse(a, m) {
  let [m0, x0, x1] = [m, BigInt(0), BigInt(1)];
  while (a > BigInt(1)) {
    let q = a / m;
    [a, m] = [m, a % m];
    [x0, x1] = [x1 - q * x0, x0];
  }
  return x1 < BigInt(0) ? x1 + m0 : x1;
}

RSA();
```

먼저 복호화하기 위해서 M=C<sup>d</sup> mod N 과 같은 공식을 사용해야 한다.

현재 공식에서 알고 있는 값은 `N`과 `C`이다. `d`를 구해야 `M`을 풀 수 있다.

`d`를 구하기 위해서는 `ed=1 mod phi` 공식을 사용하면 된다. `phi`를 구하기 위해서는 `N`을 통해 구해야 한다.

`N=pq`이므로 소인수 분해를 통해 나머지가 0일 때 `p`와 `q`값이 정해진다. `p`값과 `q`값이 정해지면 `phi`값도 구할 수 있다.

`phi=(p-1)(q-1)`이므로 구해진다.

아까 위에서 봤던 공식인 `ed=1 mod phi`를 활용하자.

이제 `e`, `phi를` 알고 있으니 `d`값을 구할 수 있다.

d=e<sup>-1</sup> mod phi 를 통해서 비로소 구할 수 있게 된다.

문제를 해결하면서 JavaScript의 [**[BigInt]**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/BigInt)를 사용하였다. BigInt는 JavaScript에서 정수를 표현하는 데 사용되는 데이터 유형이며 일반적인 숫자 데이터 유형의 범위보다 훨씬 더 큰 정수를 나타낼 수 있다.

암호화, 보안과 같은 작업에서는 매우 큰 정수를 다루어야 하기 때문에 BigInt를 사용한다고 한다.

그리고 RSA 암호화는 보통 큰 정수를 처리하는 데 사용된다고 한다. 암호화된 메시지는 일반적으로 큰 정수 형태로 표현되기 때문에 일반적으로 16진수로 변환하여 사용한다고 한다.

구한 메시지 값은 큰 정수 형태로 표현되어 있기 때문에 16진수로 변환한 뒤 ascii 문자열로 해석하여 해독한 것이다.

나머지 코드는 읽기 쉽게 메소드로 분리해놨으니 차근차근 읽어가기 바란다.

---

# 3️⃣ 느낀점

Java에는 `BigInteger class`가 있다고 한다. JavaScript에는 비슷한 무엇인가가 없나 하고 검색을 해봤더니 `BigInt`를 사용할 수 있었다. 그래서 JavaScript로 RSA 암호문을 복호화 하였다.

공식이 너무 정확하고 확고하게 나와 있기 때문에 그 공식에 맞게 대입만 잘 하고 메서드만 잘 짜면 큰 문제 없이 해독할 수 있을 것이다.

BigInt라는 것을 처음 써봤는데 신기하다. 인자로는 문자열도 사용이 가능하더라.

주의할 점은 내장 `Math` 객체의 메서드와 함께 사용이 불가능하며 `Number`와 함께 연산이 불가능하다. 당연한건가 ㅋ? 쨌든. 같은 자료형으로 형변환을 해줘야 충돌이 없다고 한다. 이상 전달 끝🔥
