---
title: "[Algorithm] 달리기 경주"
categories: [algorithm]
tags: [algorithm, programmers, Lv1]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 쉰 번째 포스팅

안녕하세요! 쉰 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 와 벌써 50❗️ 추카방구티비털💜

오늘의 포스팅 내용은 **프로그래머스 - 달리기 경주**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 문제

[**[프로그래머스] 달리기 경주 (문제 링크)**](https://school.programmers.co.kr/learn/courses/30/lessons/178871)

## 💨 **문제 설명**

얀에서는 매년 달리기 경주가 열립니다. 해설진들은 선수들이 자기 바로 앞의 선수를 추월할 때 추월한 선수의 이름을 부릅니다. 예를 들어 1등부터 3등까지 "mumu", "soe", "poe" 선수들이 순서대로 달리고 있을 때, 해설진이 "soe"선수를 불렀다면 2등인 "soe" 선수가 1등인 "mumu" 선수를 추월했다는 것입니다. 즉 "soe" 선수가 1등, "mumu" 선수가 2등으로 바뀝니다.

선수들의 이름이 1등부터 현재 등수 순서대로 담긴 문자열 배열 `players`와 해설진이 부른 이름을 담은 문자열 배열 `callings`가 매개변수로 주어질 때, 경주가 끝났을 때 선수들의 이름을 1등부터 등수 순서대로 배열에 담아 return 하는 solution 함수를 완성해주세요.

## 💨 **제한 사항**

- 5 ≤ players의 길이 ≤ 50,000
  - `players[i]`는 i번째 선수의 이름을 의미합니다.
  - `players`의 원소들은 알파벳 소문자로만 이루어져 있습니다.
  - `players`에는 중복된 값이 들어가 있지 않습니다.
  - 3 ≤ `players[i]`의 길이 ≤ 10
- 2 ≤ `callings`의 길이 ≤ 1,000,000
  - `callings`는 `players`의 원소들로만 이루어져 있습니다.
  - 경주 진행중 1등인 선수의 이름은 불리지 않습니다.

## 💨 **입출력 예**

|                players                |            callings            |                result                 |
| :-----------------------------------: | :----------------------------: | :-----------------------------------: |
| ["mumu", "soe", "poe", "kai", "mine"] | ["kai", "kai", "mine", "mine"] | ["mumu", "kai", "mine", "soe", "poe"] |

## 💨 **입출력 예 설명**

**입출력 예 #1** <br>

4등인 "kai" 선수가 2번 추월하여 2등이 되고 앞서 3등, 2등인 "poe", "soe" 선수는 4등, 3등이 됩니다. 5등인 "mine" 선수가 2번 추월하여 4등, 3등인 "poe", "soe" 선수가 5등, 4등이 되고 경주가 끝납니다. 1등부터 배열에 담으면 ["mumu", "kai", "mine", "soe", "poe"]이 됩니다.

---

# 2️⃣ 문제 풀이

## 🔥 나의 문제 풀이

> 1. players 배열에 있는 선수들의 순위를 obj 객체에 저장함.
> 2. callings 배열에 있는 선수들의 순위를 바꿔주는데, obj 객체를 이용하여 순위를 바꿔줌.
> 3. 순위를 바꾸고 나서는 obj 객체를 업데이트 해줌.
> 4. 단순히 , obj 객체는 순위를 저장하는데 사용하고, players 배열은 최종 결과를 위해 사용함.
> 5. players 배열을 리턴함.

```js
function solution(players, callings) {
  const obj = {};

  players.forEach((player, rank) => {
    obj[player] = (obj[player] || 0) + rank;
  });
  // { mumu: 0, soe: 1, poe: 2, kai: 3, mine: 4 }

  for (let i = 0; i < callings.length; i++) {
    let playerRank = obj[callings[i]];

    // players 배열에서 순위를 바꿔줌.
    let temp = players[playerRank - 1];
    players[playerRank - 1] = players[playerRank];
    players[playerRank] = temp;

    // obj 객체를 업데이트 해줌.
    obj[players[playerRank]] = playerRank;
    obj[players[playerRank - 1]] = playerRank - 1;
  }

  return players;
}

console.log(
  solution(
    ["mumu", "soe", "poe", "kai", "mine"],
    ["kai", "kai", "mine", "mine"]
  )
);
```

문제는 생각보다 간단했다. `callings`에 따라서 순서를 변경해주기만 하면 된다.

## 🔥 시간 초과 풀이

```js
function solution(players, callings) {
  for (let i = 0; i < callings.length; i++) {
    const playerRank = players.indexOf(callings[i]);
    const player = players.splice(playerRank, 1);
    players.splice(playerRank - 1, 0, ...player);
  }
  return players;
}
```

처음에는 위와 같은 방법을 사용해서 정말 짧은 코드가 나왔다. `splice()`를 사용해서 간단하게 위치를 바꿔줄 수 있었다.

하지만, 위 코드는 시간초과가 발생한다. `splice()`를 사용하면 시간 복잡도가 O(n)이기 때문에 시간 초과가 발생하게 된다.

그래서 객체를 사용해서 객체에 선수마다 순위를 저장해둔 후 `players` 배열에서 순위를 바꿔준다. 하지만 객체에 순위가 저장되어 있으므로 객체도 업데이트 해줘야 한다. 그래야만 정답이 나온다.

---

# 3️⃣ 느낀점

객체를 업데이트 해줘야 할 때 이미 바뀌어버린 `players` 배열 때문에 조금 헷갈렸다. 약간 뇌정지가 왔달까..?

배열과 객체가 우르르르 나오고 배열 안의 배열처럼 계속 고뇌하다 보니 풀다가 뇌정지가 왔던 것 같다.

배열만 사용한 풀이에서 `splice()`를 사용하여 시간초과가 났다. `callings`의 배열의 최대 길이가 백만이었기 때문에 시간초과가 날 것 같았다.

보통 제한사항에서 크기가 백만 가까이 되면 일반적으로 풀면 시간초과가 발생한다. 그것을 어떻게 유연하게 잘 헤쳐 나가느냐가 매우 중요한 것 같다. 아좌좌좌좌좌좌조좌조자❗️💕
