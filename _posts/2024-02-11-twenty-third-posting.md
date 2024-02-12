---
title: "[React] 상태 관리 라이브러리 Jōtai"
categories: [jotai, react]
tags: [jotai, state, react, next]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 스물 두 번째 포스팅

안녕하세요! 스물 두 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

정말 오랜만에 돌아온 포스팅이네요! <br>

![갑진년](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/ce773f36-8836-4404-bca3-cf5129724b1c)<br>

청룡의 해입니다❗️ 갑진년에는 모두 건강하시고 무탈한 행복한 해가 되시길 바랍니다💪<br>
새해 복 많이 받으세요🙇

오늘의 포스팅 내용은 **상태 관리 라이브러리 - 조타이(Jōtai)**에 관한 이야기입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

[**[조타이 공식 홈페이지]**](https://tutorial.jotai.org/)를 기반으로 한 포스팅입니다.

---

# 1️⃣ 조타이(Jōtai)란?

jotai는 일본어로 '상태'라는 뜻이다.<br>
리액트의 애니메이션 라이브러리로 유명한 react-spring을 개발한 [**[Pmndrs]**](https://pmnd.rs/) 팀에서 만들었다고 한다.

**[공식 문서 소개]**

> <i>Jotai takes an atomic approach to global React state management. </i>
>
> <i> Build state by combining atoms and renders are automatically optimized based on atom dependency. This solves the extra re-render issue of React context, eliminates the need for memoization, and provides a similar developer experience to signals while maintaining a declarative programming model. </i>
>
> <i> It scales from a simple useState replacement to an enterprise TypeScript application with complex requirements. Plus there are plenty of utilities and extensions to help you along the way! </i>
>
> <i> Jotai is trusted in production at innovative companies like these. </i>

> Jotai는 전역 React 상태 관리에 원자적인 접근 방식을 채택합니다.
>
> 원자(atom)를 결합하여 상태를 구축하고, 렌더링은 원자의 종속성에 기반하여 자동으로 최적화됩니다. 이는 React 컨텍스트의 추가적인 재렌더링 문제를 해결하고, memoization의 필요성을 제거하며, signal과 유사한 개발자 경험을 제공하면서도 선언적인 프로그래밍 모델을 유지합니다.
>
> 이는 단순한 `useState` 대체부터 복잡한 요구 사항을 갖춘 기업용 TypeScript 애플리케이션까지 규모에 맞게 확장됩니다. 게다가 유틸리티와 확장 기능도 있습니다!

위와 같은 내용으로 상태 관리를 하는 라이브러리이다.

1. **원자적 상태 관리** <br>
   Jotai는 상태를 원자 단위로 관리한다. 상태의 독립적 조각을 정의하고 조합하여 애플리케이션의 전역 상태를 구성할 수 있다.
2. **리액트와 통합** <br>
   리액트와 긴밀하게 통합되어 있어서 컴포넌트와 함께 자연스럽게 사용 가능하다.
3. **선언적 프로그래밍**
4. **성능 최적화**
5. **TypeScript 기본 내장**

위와 같은 특징을 가지고 있다고 한다.

# 2️⃣ 상태 관리 라이브러리의 비교

1. **Redux** <br>
   Redux는 상태의 중앙화로 prop drilling의 문제를 해결하였다. 현재까지도 많은 인기를 끌고 있는 라이브러리이며, 리덕스 툴킷과 함께 사용 가능하다.<br> 장점으로는 신뢰성 있으며 관련 자료와 커뮤니티가 방대하다. 더불어 디버깅에 굉장히 용이하다는 장점이 있다. 큰 서비스일수록 디버깅의 역할이 돋보일 것이라고 한다.

2. **Context API** <br>
   이 경우 리액트에 내장되어 있기에 별도의 설치가 필요가 없다. 하지만 Provider의 값이 변경될 경우 그 Context를 구독하는 하위 모든 컴포넌트들의 불필요한 렌더링이 일어나기 때문에 반복, 복잡한 업데이트에서 사용할 경우 최적화에 신경 써야 할 부분이 늘어나 비효율적이라고 한다. <br>
   이 경우 상태 변경이 잦지 않은 애플리케이션에 적합하다고 생각한다.

3. **Recoil** <br>
   사실 Recoil은 정확히 모른다. 이는 페이스북에서 개발한 상태 관리 라이브러리라고 한다. 리액트처럼 사고하며 React hooks과 굉장히 비슷하다고 한다.<br> 하지만 어디서 봤는데 Recoil은 업데이트가 되지 않고 망했다는 소식을 어디서 접한 적이 있는 것 같다.

[**[Jotai는 조-타이 라고 읽습니다.]**](https://medium.com/pinkfong/jotai%EB%8A%94-%EC%A1%B0-%ED%83%80%EC%9D%B4-%EB%9D%BC%EA%B3%A0-%EC%9D%BD%EC%8A%B5%EB%8B%88%EB%8B%A4-6498535abe11) - 이 글의 비교글을 인용하였다. 굉장히 설명히 잘 되어 있는 글이다. 내 글을 읽다보면 한 번 들어가서 보는 것이 오히려 이득일 수도 있다.
<br>

---

# 3️⃣ Jotai Tutorial

자 이제 실질적으로 조타이 공식 문서에 있는 배워보기를 통해 글을 써보도록 하겠다. 나도 공부하면서 쓰는 글이라 서툴 수도 있으니 이해 바란다.(바랍니다.)

## 📍 Introduction

아톰(atom) 만들기
나는 처음에 atom이 있길래 만화 캐릭터 `아톰`을 이용한 방법인 줄 알았다. 하지만 그럴 리가 없었다.

Jotai의 atom은 작고 독립적인 상태 조각이다. 이상적으로 한 개의 아톰은 매우 작은 데이터를 포함한다.

시작하기 앞서 조타이를 설치하는 방법이다.

> - **npm**
>
> npm i jotai
>
> - **yarn**
>
> yarn add jotai
>
> - **pnpm**
>
> pnpm install jotai

이는 아톰을 생성하는 방법이다.

```js
import { atom } from "jotai";
const counter = atom(5);
```

React의 통합된 `useState()` 훅과 같이 사용하기 매우 간단하며, 모든 상태가 전역적으로 접근이 가능하다.(매우 신기)

```js
const [count, setCount] = useAtom(counter);
```

jotai의 `useAtom()` 함수를 사용하여 `useState()` 훅에 전달되며, 배열의 첫 번째 요소는 아톰의 값이며, 두 번째 요소는 아톰의 값을 설정하는 데 사용되는 함수이다. 정말 useState와 모양은 똑같다. 사용법도 비슷하다고 한다.

Jotai는 모든 것을 아톰으로 간주하기 때문에 객체, 배열, 중첩된 객체와 같은 어떤 유형의 아톰이든 원하는 대로 만들 수 있다.

```js
const friendObj = atom({ name: "Boong", online: false });
const cities = atom(["Seoul", "Paris", "LA"]);
const nestedObj = atom({ friend1: { name: "cho", age: 25 } });
```

<br>

## 📍 Theme Switch

개발자들이 다크 테마를 좋아하지만 (나는 밝은 게 좋던데 개발자가 아닌가보다.) 테마 설정은 앱 내에 많은 컴포넌트가 있고 테마 props를 컴포넌트 트리의 매우 깊은 곳에 전달해야 할 때 매우 복잡해질 수 있다.이로 인해 혼란스러울 수 있다.

Jotai는 앱에 대한 다양한 테마를 금방 설정 가능하다.

기본값을 가진 테마 아톰을 초기화한다.

```js
const theme = atom("light");
```

그 후 useState 훅에 아톰을 전달한다.

```js
const [appTheme, setAppTheme] = useAtom(theme);
```

글을 쓰다 보니까 기본 아톰 값을 필수적으로 선언 해줘야 하는 것 같다.

```js
const [appTheme, setAppTheme] = useAtom("light");
```

위와 같이 훅 안에 선언하는 것은 안되는 것 같다.<br>
Jotai의 `useAtom()` 훅에는 원자 객체가 전달 되어야 한다고 한다.

<br>

## 📍 Persisting state value

jotai의 아톰을 사용해서 상태 값을 로컬 저장소에 유지하는 방법이 있다고 한다.<br>
상태 값을 로컬 저장소에 유지하는 건 어려울 수 있다. 사용자의 선호도나 다음 세션을 위한 데이터를 유지하고 싶을 수 있다.

Jotai의 `atomWithStorage`는 특별한 아톰으로, 제공된 값과 로컬 저장소 또는 세션 저장소 간에 자동으로 동기화되며, 첫 번째로 로드될 때 값이 자동으로 선택된다. jotai/utils 모듈에서 사용 가능하다.

```js
import { useAtom } from "jotai";
import { atomWithStorage } from "jotai/utils";

const theme = atomWithStorage("dark", false);

export default function Page() {
  const [appTheme, setAppTheme] = useAtom(theme);
  const handleClick = () => setAppTheme(!appTheme);
  return (
    <div className={appTheme ? "dark" : "light"}>
      <h1>This is a theme switcher</h1>
      <button onClick={handleClick}>{appTheme ? "DARK" : "LIGHT"}</button>
    </div>
  );
}
```

위와 같이 사용하면 theme이 변경되더라도 새로고침 시 그 theme을 유지한다. 사용자 경험 측면에서 중요할 수 있다고 생각한다.

<br>

## 📍 Read Only atoms

읽기 전용 아톰. 읽기 전용도 있고 쓰기 전용도 있나보다. 신기하네.

읽기 전용 아톰은 다른 아톰의 값을 읽기 위해 사용된다. 직접 값을 설정하거나 변경할 수 없다. 왜냐하면 이러한 아톰들은 부모 아톰에 의존하기 때문이다.

```js
const textAtom = atom("readonly");
const uppercase = atom((get) => get(textAtom).toUpperCase());
```

이러한 아톰들은 get 매개변수를 사용하는 콜백을 사용한다. 이를 통해 다른 아톰의 값을 읽을 수 있다. 부모 값을 변경하면 파생된 아톰도 함께 업데이트된다.

```js
const firstName = atom("병찬");
const lastName = atom("조");
const fullName = atom((get) => get(firstName) + get(lastName));
```

이러한 아톰들은 단순히 다른 아톰의 값을 읽는 것 이상의 작업을 한다. 필터링하거나 정렬하거나 매핑하는 것도 가능하다. 이것이 Jotai의 아름다움이라고 한다. 오호. Jotai는 보다 더 이상한 아톰에서 파생된 더 이상한 아톰들을 우아하게 생성할 수 있도록 한다.

```js
import { atom, useAtom } from "jotai";

const textAtom = atom("readonly atoms");
const uppercase = atom((get) => get(textAtom).toUpperCase());

export default function Page() {
  const [lowercaseText, setLowercaseText] = useAtom(textAtom);
  const [uppercaseText] = useAtom(uppercase);
  const handleChange = (e) => setLowercaseText(e.target.value);
  return (
    <div className="app">
      <input value={lowercaseText} onChange={handleChange} />
      <h1>{uppercaseText}</h1>
    </div>
  );
}
```

![jotai1](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/967157db-c55b-4cc6-b830-eb60c035ea71)<br>

위와 같은 귀여운 예제를 볼 수 있다. 아직까지는 `useState()` 와 거의 동일하다.

<br>

## 📍 Write Only atoms

역시나 읽기 전용이 있으면 쓰기 전용도 존재했다.

쓰기 전용 아톰을 사용하면 해당 아톰이 의존하는 아톰을 수정할 수 있다. 이것은 사실상 양방향 데이터 바인딩인 셈이다. 파생된 아톰을 변경하면 부모 아톰도 변경되고 그 반대로 마찬가지로 변경되기 때문에 정말 신중하게 사용해야 한다.

```js
const textAtom = atom("write only atoms");
const uppercase = atom(null, (get, set) => {
  set(textAtom, get(textAtom).toUpperCase());
});
```

콜백의 첫 번째 인수는 항상 null이다. (write only)에서는. 두 번째 인수는 아톰 값을 수정하는 함수이다. get과 set을 매개변수로 삼아 변경 가능하다.

`set(a, b)` 이면 a를 b로 업데이트한다. 라는 의미라고 생각한다.

흥미로운 예제가 있다.

여기서는 캔버스에 그린 점들의 위치를 나타내는 `dotsAtom`과 그림을 그리는 동안 참인 `drawingAtom`을 정의한다.

```js
const dotsAtom = atom([]);
const drawingAtom = atom(false); // 그림 그릴 때 true
```

`handleMouseDownAtom` 및 `handleMouseUpAtom`은 `drawingAtom`의 값을 설정하기 위해 사용되는 두 개의 쓰기 전용 아톰이고, `handleMouseMoveAtom`은 쓰기 전용 아톰으로, 캔버스에 그림을 그릴 때 새로운 점의 위치를 dotsArray 아톰에 추가한다.

```js
const handleMouseMoveAtom = atom(null, (get, set, update: Point) => {
  if (get(drawingAtom)) {
    set(dotsAtom, (prev) => [...prev, update]);
  }
});
```

직접 아톰 값을 업데이트하는 대신 쓰기 전용 아톰을 사용하여 값을 업데이트하는 이유는 앱에서 추가적인 재렌더링을 방지하기 위해서라고 한다.

```js
import { atom, useAtom } from "jotai";

const dotsAtom = atom([]);

const drawingAtom = atom(false);

const handleMouseDownAtom = atom(null, (get, set) => {
  set(drawingAtom, true);
});

const handleMouseUpAtom = atom(null, (get, set) => {
  set(drawingAtom, false);
});

const handleMouseMoveAtom = atom(null, (get, set, update: Point) => {
  if (get(drawingAtom)) {
    set(dotsAtom, (prev) => [...prev, update]);
  }
});

const SvgDots = () => {
  const [dots] = useAtom(dotsAtom);
  return (
    <g>
      {dots.map(([x, y], index) => (
        <circle cx={x} cy={y} r="5" fill="blue" key={index} />
      ))}
    </g>
  );
};

const SvgRoot = () => {
  const [, handleMouseUp] = useAtom(handleMouseUpAtom);
  const [, handleMouseDown] = useAtom(handleMouseDownAtom);
  const [, handleMouseMove] = useAtom(handleMouseMoveAtom);
  return (
    <svg
      width="100vw"
      height="100vh"
      viewBox="0 0 100vw 100vh"
      onMouseDown={handleMouseDown}
      onMouseUp={handleMouseUp}
      onMouseMove={(e) => {
        handleMouseMove([e.clientX, e.clientY]);
      }}
    >
      <rect width="100vw" height="100vh" fill="#eee" />
      <SvgDots />
    </svg>
  );
};

const App = () => (
  <>
    <SvgRoot />
  </>
);

export default App;
```

![jotai2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/7a28aac0-cdfc-44e7-b485-02dc5cfe6ada)<br>

위와 같은 재밌는 예시가 있다. 코드 한 번씩 읽어보며 익혀보는 것도 좋을 것 같다. 마우스로 누르고 있으면 그려지고 떼는 순간 false로 바뀌며 그리는 것을 중단한다. 재밌다. 궁금한 분들은 해보시는게 좋을 것 같다.

<br>

## 📍 Read Write atoms

읽기 전용, 쓰기 전용 아톰도 있다면 읽기-쓰기 아톰도 존재한다.

이는 읽기 전용 및 쓰기 전용 아톰의 결합의 예시이다.

```js
const count = atom(1);
export const readWriteAtom = atom(
  (get) => get(count),
  (get, set) => {
    set(count, get(count) + 1);
  }
);
```

첫 번째 매개변수는 읽기, 두 번째 매개변수는 아톰 값을 수정하는 데 사용되는 쓰기이다. <br>
읽기-쓰기 아톰은 원래의 아톰 값을 읽고 설정할 수 있기 때문에 원래의 아톰을 더 작은 범위에서 숨기고 `readWriteAtom` 아톰만 내보낼 수 있다. 이렇게 함으로써 앱에서 처리해야 할 아톰의 수를 줄일 수 있다.

위 쓰기 전용 아톰의 예시였던 점 찍기 예시를 읽기-쓰기 아톰을 사용하여 업데이트하는 방법을 볼 수 있다.

```js
const handleMouseMoveAtom = atom(
  (get) => get(dotsAtom),
  (get, set, update: Point) => {
    if (get(drawingAtom)) {
      set(dotsAtom, (prev) => [...prev, update]);
    }
  }
);
```

이와 같이 읽기-전용 아톰을 사용할 수 있다.

자세한 코드는 [**[Read Write Atoms]**](https://tutorial.jotai.org/quick-start/read-write-atoms)
여기를 참조 바란다.

<br>

## 📍 Atom Creators

아톰 생성자는 단순히 아톰 또는 아톰 집합을 반환하는 함수를 의미한다. 이는 단순히 함수일 뿐이며 라이브러리가 제공하는 기능은 아니다. 하지만 상당히 복잡한 사용 사례를 만들기 위해 중요한 패턴이다.<br>
이는 첫 번째 상태를 업데이트하기 위해 다른 아톰을 설정해야 하는 번거로움을 피할 수 있게 한다.

```js
const fooAtom = atom(0);
const barAtom = atom(0);
const incFooAtom = atom(null, (get, set) => {
  set(fooAtom, (c) => c + 1);
});
const incBarAtom = atom(null, (get, set) => {
  set(barAtom, (c) => c + 1);
});
```

적절한 동작을 해당 아톰의 setter에 연결할 수 있지만, 코드에 더 많은 아톰이 있는 경우 보일러플레이트 코드가 증가하는 것을 볼 수 있다.

> **보일러 플레이트 코드란?** <br>
> 보일러 플레이트 코드란 뼈대 코드 또는 반복적으로 작성되는 코드를 말한다. 일반적으로 똑같거나 비슷한 코드가 반복되는 것을 의미하며 이를 줄이는 것은 코드를 더 깔끔하고 유지보수가 쉽도록 만드는 데 도움이 된다.
>
> 예를 들어서, 여러 개의 상태 값을 업데이트하는 데 필요한 많은 수의 아톰을 만들 때, 각각의 아톰에 대해 일일이 업데이트 함수를 작성하는 것은 보일러 플레이트 코드이다. <br> 이를 아톰 생성자 함수와 같은 방법으로 줄일 수 있다. 이렇게 하면 코드가 더욱 간결해지고 반복 작업이 줄어들어 유지보수가 쉬워진다.

```js
const incAllAtom = atom(null, (get, set, action)=>{
  if(action === 'inc1') // 첫 번째 아톰 증가
  if(action === 'inc2')
  ...
})
```

이를 아톰 생성자 함수로 대체 가능하다.

```js
const createCountIncAtoms = (initialValue) => {
  const baseAtom = atom(initialValue);
  const valueAtom = atom((get) => get(baseAtom));
  const incAtom = atom(null, (get, set) => set(baseAtom, (c) => c + 1));
  return [valueAtom, incAtom];
};
```

한 마디로 그냥 가비지 코드를 줄이고 일반적인 함수를 만들어서 적용시킨다는 말인 것 같다.

<br>

## 📍 Async Read Atoms

비동기 읽기 전용 아톰도 있다고 한다. (종류가 꽤나 많네? → 이런데 사용하기 편하고 좋은건가? → 그러니까 사용하겠지;)

비동기 아톰을 사용하면 아톰에서 직접 실제 데이터에 액세스할 수 있으며 놀라운 편리성으로 관리 가능하다.

비동기 아톰을 두 가지로 크게 나누는데 **비동기 읽기 전용 아톰**과 **비동기 쓰기 전용 아톰**이다.

여기서 알아볼 것은 비동기 읽기 아톰이다. <br>
아톰의 읽기 함수는 프로미스를 반환할 수 있다.

```js
const counter = atom(0);
const asyncAtom = atom(async (get) => get(counter) * 5);
```

Jotai는 기본적으로 비동기 흐름을 처리하기 위해 `Suspense`를 활용하고 있다.

```js
<Suspense fallback={<span>로딩 중...</span>}>
  <AsyncComponent />
</Suspense>
```

하지만 jotai/utils에 있는 `loadable API`를 사용하여 이를 더 jotai스럽게 처리할 수 있다. 단순히 아톰을 loadable 유틸로 래핑하면 값이 세 가지 상태 중 하나를 가진 값을 반환한다. 그건 바로 `loading`, `hasData`, `hasError` 이다.

```js
import { loadable } from "jotai/utils";

const countAtom = atom(0);
const asyncAtom = atom(async (get) => get(countAtom));
const loadableAtom = loadable(asyncAtom);
const AsyncComponent = () => {
  const [value] = useAtom(loadableAtom);
  if (value.state === "hasError") {
    return <div>{value.error}</div>;
  }
  if (value.state === "loading") {
    return <div>로딩 중...</div>;
  }
  return <div>Value: {value.data}</div>;
};
```

state에 따라 관리해 주는 것 같다.

<br>

## 📍 Async Write Atoms

이번에는 비동기 쓰기 아톰이다.

비동기 쓰기 아톰에서 아톰의 쓰기 함수는 프로미스를 반환한다.

```js
const counter = atom(0);
const asyncAtom = atom(null, async (set, get) => {
  // 어떤 작업을 기다린 후
  set(counter, get(counter) + 1);
});
```

> **참고** <br>
> 여기서 중요한 점은 비동기 쓰기 함수는 Suspense를 트리거하지 않는다는 점이다!

그러나 Jotai로 달성할 수 있는 흥미로운 패턴 중 하나는 원하는 때에 Suspense를 트리거하기 위해 비동기에서 동기로 전환하는 것이다...

```js
const request = async () => fetch("https://...").then((res) => res.json());
const baseAtom = atom(0);
const Component = () => {
  const [value, setValue] = useAtom(baseAtom);
  const handleClick = () => {
    setValue(request()); // 요청이 해결될 때까지 대기함. Suspense 발생.
  };
  // ...
};
```

비동기 함수를 동기적으로 호출하는 것은 Suspense를 트리거하여, UI를 데이터가 로드될 때까지 중단시키고 로딩 상태를 표시하는 데 사용한다.

```js
export default function AsyncSuspense() {
  return (
    <div className="app">
      <Suspense fallback={<span>loading...</span>}>
        <Component />
      </Suspense>
    </div>
  );
}
```

이런 식으로 Suspense 처리를 해주어야 오류가 발생하지 않는다.

<br>

## 📍 Official Utils

이번에는 "jotai/utils"에서 찾을 수 있는 원자 생성기 및 훅 유틸리티에 대한 내용이다.

- **atomWithReset** <br>
  `useResetAtom` 훅을 사용하여 `initialValue`로 재설정할 수 있는 atom을 생성한다. 이 기능은 기본 atom과 동일하게 작동하지면 특별한 값으로 RESET이 가능하다.

```js
import { atomWithReset } from "jotai/utils";

const counter = atomWithReset(1);
```

말 그대로 특정 값으로 리셋시키는 것이다.

- **selectAtom** <br>
  이름과는 달리, `selectAtom`은 탈출구로 제공된다. 이를 사용하면 100% 순수 atom 모델을 구축하는 것이 아니다. 가능한 한 파생된 atom을 사용하고, `equalityFn`이나 `prevSlice`를 피할 수 없을 때에만 `selectAtom`을 사용하는 것이 좋다.

```ts
function selectAtom<Value, Slice>(
  anAtom: Atom<Value>,
  selector: (v: Value, prevSlice?: Slice) => Slice,
  equalityFn: (a: Slice, b: Slice) => boolean = Object.is
): Atom<Slice>;
```

이는 공식 문서에 나온 설명이다. TS로 이루어져 있어 TS를 다루지 못하는 나에게는 이해가 어렵다.

---

> anAtom: Atom<Value> <br>

이 매개변수는 `Atom` 타입의 값인 `anAtom`을 받는다. `<Value>`에 해당하는 값의 타입을 지정한다.

> selector: (v: Value, prevSlice?: Slice) => Slice <br>

이 매개변수는 `Value` 타입의 값을 입력으로 받고, `Slice` 타입의 값을 반환하는 함수이다. `prevSlice`는 선택적 매개변수로, 해당 함수가 호출될 때 이전 슬라이스 값을 사용할 수 있다.

> equalityFn: (a: Slice, b: Slice) => boolean <br>

이 매개변수는 `Slice` 타입의 두 값을 입력으로 받고, 불리언 값을 반환하는 함수이다. 기본값으로는 `Obeject.is` 함수가 사용된다.

> Atom<Slice> <br>

이 함수의 반환 값은 `Atom` 타입의 `Slice`를 가지는 값이다.

---

위 코드는 TypeScript에서 함수의 타입을 명확히 정의하여 타입 안정성을 확보하는 데 사용한다고 한다.

이 함수는 원래 atom 값의 함수인 파생된 atom을 생성한다. 선택자 함수는 원래 atom이 변경될 때마다 실행되며, `equalityFn` 이 파생된 값이 변경되었음을 보고할 때만 해당 값을 업데이트한다. 기본적으로 `equalityFn`은 참조 동등성을 사용하지만, 필요한 경우 `deepEquals` 함수를 제공할 수 있다.

```js
const defaultPerson = {
  name: {
    first: "Byeongchan",
    last: "CHO",
  },
  birth: {
    year: 2000,
    month: "Jan",
    day: 3,
    time: {
      hour: 1,
      minute: 1,
    },
  },
};

// 원본 atom
const personAtom = atom(defaultPerson);

// person.name을 추적함. person.name 객체가 변경될 때 업데이트 된다.
// name.first나 name.last가 실제로 변경되지 않아도 업데이트 된다.
const nameAtom = selectAtom(personAtom, (person) => person.name);

// person.birth를 추적함. year, month, day, hour, minute이 변경될 때 업데이트 된다.
// deepEquals를 사용하면 birth 필드가 동일한 데이터를 포함하는 새 객체로 대체되어도
// 이 atom은 업데이트 되지 않는다. 예를 들어 db에서 person을 다시 읽어올 때이다.
const birthAtom = selectAtom(personAtom, (person) => person.birth, deepEquals);
```

**안정적 참조 유지** <br>
렌더링 사이클에서 `useAtom()`을 사용할 때 무한 루프를 방지하기 위해 항상 안정적인 atom 참조를 제공해야 한다. `selectAtom`의 경우, 기본 원자와 선택자 모두가 안정적이어야 한다.

```js
const [value] = useAtom(selectAtom(atom(0), (val) => val));
```

위와 같은 경우 무한 루프가 발생한다. 누가 봐도 atom 초기화하지 않은 것부터 이상해 보인다.

```js
const baseAtom = atom(0); // 안정적
const baseSelector = (v) => v; // 안정적
const Component = () => {
  // 해결 방법 1 : "useMemo()"를 사용하여 전체 결과 원자를 memoization
  const [value] = useAtom(useMemo(() => SelectAtom(baseAtom, (v) => v), []));

  // 해결 방법 2 : "useCallback()"을 사용하여 인라인 콜백을 memoization
  const [value] = useAtom(
    selectAtom(
      baseAtom,
      useCallback((v) => v, [])
    )
  );

  // 해결 방법 3 : 모든 제약 조건이 이미 충족됨
  const [value] = useAtom(selectAtom(baseAtom, baseSelector));
};
```

너무 복잡해서 정확하게 무슨 소리인지 잘 이해가 가지 않는다.

하지만 `selectAtom`은 원시 atom 값에 기초한 파생된 atom을 생성하는 것이고 사용하는 이유는 크게 **파생 상태 관리**, **복잡한 파생 로직의 분리**, **성능 최적화**, **재사용성 및 유지보수성 높임** 이라고 보면 될 것 같다. 결국 상태 관리를 더 유연하고 효율적으로 처리할 수 있도록 돕는 유틸인 것 같다.

이 외에도 다양한 util 함수가 존재한다.
자세한 사항은 [**[Jotai]**](https://jotai.org/docs/utilities)를 참조하길 바란다.

---

여기까지 Jotai에 대한 기본 튜토리얼에 대해 알아보았습니다❗️ 말투가 왜 다시 바뀌었냐고요? 글 쓸 때는 저렇게 쓰는게 가독성도 있고 이해하기도 빠른 것 같아서 저렇게 써 보았습니다✨

조-타이는 더욱 많은 정보를 갖고 있기 때문에 이것으로는 많이 부족하며 이것은 단지 초기 이해를 위한 자료에 불과합니다. 조-타이와 관련한 게시글들은 찾아보면 많이 있으니 이 글과 함께 참고해서 봐주시면 좋을 것 같습니다~

오늘도 게시글 읽어 주셔서 감사합니다🎶<br>
새해 복 많이 받으세요🙇
