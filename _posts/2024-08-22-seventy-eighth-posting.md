---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 4일차"
categories: [udemy-sniperfactory, react]
tags:
  [
    유데미,
    udemy,
    웅진씽크빅,
    스나이퍼팩토리,
    인사이드아웃,
    미래내일일경험,
    프로젝트캠프,
    부트캠프,
    React,
    리액트프로젝트,
    프론트엔드개발자양성과정,
    개발자교육과정,
  ]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 일흔 여덟 번째 포스팅

안녕하세요! 일흔 여덟 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 4일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ React의 기본

## 💧 Children

React에서 `children`은 컴포넌트의 props 중 하나로, 부모 컴포넌트가 자식 컴포넌트 사이에 포함시킨 내용을 참조할 수 있도록 하는 속성이다.

이 속성을 통해서 부모 컴포넌트는 자식 컴포넌트 내부에 JSX나 다른 React 컴포넌트를 포함시킬 수 있다.

TypeScript에서 `children`은 `ReactNode` 타입으로 정의된다. `ReactNode`는 `JSX.Elemnt`, `number`, `string` 등 여러 타입을 포함할 수 있는 타입이다.

```jsx
type ChildComponentProps = {
  children: React.ReactNode,
};

const ChildComponent = ({ children }: ChildComponentProps) => {
  return (
    <div>
      <p>안녕! 나는 자식 컴포넌트야!</p>
      <div>{children}</div>
    </div>
  );
};

const ParentComponent = () => {
  return (
    <>
      <h1>안녕! 나는 부모 컴포넌트야!</h1>
      <ChildComponent>
        <p>나는 자식 컴포넌트 안에 있는 텍스트야</p>
      </ChildComponent>
    </>
  );
};

export default ParentComponent;
```

위와 같이 `children`은 기본적으로 부모 컴포넌트가 자식 컴포넌트 사이에 포함시킨 내용을 참조하는 속성이다.

![image](https://github.com/user-attachments/assets/6f927e3b-ecc8-41f2-a807-13d0c772b98f) <br />

이렇게 부모 컴포넌트에서 `h1` 태그를 먼저 보여준 후 `ChildComponent`로 이동하여 두 번째 문장을 렌더링한 후 `children` 요소였던 마지막 문장을 렌더링하게 되는 것이다.

<br />

## 💧 간단한 실습

![image](https://github.com/user-attachments/assets/bfa16afd-38c1-461a-b96e-32ad1186a146)<br />

`children`과 `props`, `tailwindCSS` 등 지금까지 배운 것들을 토대로 재사용 가능한 컴포넌트들을 제작하는 실습이었다.

위 그림은 내가 만든 뽀짝한 예제이다.

```jsx
import { twMerge as cn } from "tailwind-merge";

type ButtonProps = {
  type: "button" | "submit" | "reset",
  children: React.ReactNode,
  className: string,
  disabled?: boolean,
  onClick?: () => void,
};

const Button = ({
  children,
  className,
  type,
  disabled,
  onClick,
}: ButtonProps) => {
  return (
    <button
      className={cn(
        "transtion-all h-10 w-28 rounded-lg px-4 py-2 duration-200",
        className
      )}
      type={type}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
};

export default Button;
```

내가 만들었던 `Button` 컴포넌트이다. 하지만 이것보다 더 간단하고 재사용성이 돋보이게 사용할 수 있었다.

```jsx
import { twMerge as cn } from "tailwind-merge";

type TButtonProps = React.ComponentPropsWithoutRef<"button">;

const Button = ({ children, className, ...rest }: TButtonProps) => {
  return (
    <button
      className={cn(
        "transtion-all h-10 w-28 rounded-lg px-4 py-2 duration-200",
        className
      )}
      {...rest}
    >
      {children}
    </button>
  );
};

export default Button;
```

위처럼 수정하면 `button` 태그의 모든 속성을 가져다 사용할 수 있다.

```tsx
type TButtonProps = React.ComponentPropsWithoutRef<"button">;
```

`TButtonProps`는 `React.ComponentPropsWithRef<"button">`를 사용하여 버튼 컴포넌트에 전달할 수 있는 모든 기본 HTML 속성과 `ref`를 포함하지 않은 타입을 정의한다.

이 타입을 사용하면 `button` 요소에서 허용되는 모든 속성을 지원하기 때문에 지원하는 속성을 필요할 때마다 사용 가능하다.

`...rest`는 `props`로 전달된 나머지 모든 속성을 담고 있으며 이 속성들은 최종적으로 `button` 요소에 그대로 전달된다.

이렇게 하면 정말 지정된 스타일을 제외하고 모든 버튼을 이 컴포넌트로 사용 가능하기 때문에 재사용성이 매우 높다.

<br />

## 💧 재사용성에 대한 고찰

### 👏 Omit

`Omit`은 TypeScript의 유틸리티 타입 중 하나로, 특정 타입에서 일부 속성을 제거한 새로운 타입을 생성할 때 사용한다.

기본 문법은 `Omit<T,K>`이다.

`T`는 원본 타입을 의미하고, `K`는 제거할 속성의 키 목록을 의미한다. `K`는 하나 이상의 속성 키를 가질 수 있다고 한다.

```tsx
type InputProps = Omit<React.ComponentPropsWithoutRef<"input">, "type">;
```

`Omit<... , "type">`에서 `Omit`을 사용하여 `type` 속성을 제거한다. 위와 같은 경우 `InputProps` 타입에는 `input` 요소에서 `type` 속성을 제외한 나머지 속성들이 포함되는 것이다.

즉, `type` 속성을 사용할 수 없는 `input` 관련 속성들만 포함시키는 것이다.

### 👏 Omit 사용 예시

`Omit`을 사용하는 목적은 컴포넌트 사용 과정에서 특정 HTML 속성을 제거하거나, 컴포넌트의 API를 제한해서 의도치 않은 속성 사용을 방지하는 것이다.

```jsx
type InputProps = Omit<React.ComponentPropsWithoutRef<"input">, "type"> & {
  type: "text" | "password" | "email" | "number" | "date",
};

const Input = ({ ...rest }: InputProps) => {
  return (
    <input
      className="w-full rounded-lg border-2 border-black px-4 py-3 ring-offset-2 transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-gray-600"
      {...rest}
    />
  );
};

export default Input;
```

`type` 속성은 HTML의 `input` 요소에서 필수적인 요소이다. `Omit`을 사용해서 `Input` 컴포넌트에서 `type` 속성을 제거한다.

대신 뒤에 `type` 속성을 명시적으로 정의해서 허용된 선택지 중 하나로 제한할 수 있다.

결론적으로 불필요하거나 잘못된 값이 전달되는 것을 방지해서 컴포넌트의 안전성과 일관성을 유지하는 것이다. 만일 명시적으로 정의한 `type` 이외에 다른 값이 전달되면 타입 오류를 발생시킨다.

```jsx
type CheckboxProps = React.ComponentPropsWithoutRef<"input"> & {
  label: string,
  id: string,
};

const Checkbox = ({ label, id, ...rest }: CheckboxProps) => {
  return (
    <div className="flex cursor-pointer items-center gap-2">
      <input
        type="checkbox"
        className="h-5 w-5 cursor-pointer appearance-none rounded-lg border-2 border-gray-300 bg-gray-200 checked:bg-blue-500"
        id={id}
        {...rest}
      />
      <label htmlFor={id} className="cursor-pointer text-lg">
        {label}
      </label>
    </div>
  );
};

export default Checkbox;
```

위 `Checkbox` 컴포넌트에서도 `input` 요소가 사용된다. 그러면 여기에도 `Input` 컴포넌트를 통해서 재사용성을 높이면 되지 않을까?

**아니다.**

`Checkbox`에서의 `input` 요소는 `checkbox`를 타입으로 받는다. 이는 `Input` 컴포넌트에서 선언되지 않은 타입이기 때문에 사용할 수 없다.

결국 의도하지 않은 방식으로 사용되는 것을 막기 위해 사용할 수 있는 것이다.

### 👏 컴포넌트 재사용성과 일관성

`Omit`을 사용하여 컴포넌트의 API를 명확히 정의하고, 사용자가 어떤 속성을 사용할 수 있는지 명확하게 알 수 있다. 이로 인해 컴포넌트 사용 시 혼동을 줄이고, 사용의 일관성을 높일 수 있다.

**또한**, 불필요하거나 잘못된 속성이 전달되는 것을 방지해서 **컴포넌트의 동작을 예측 가능하고 안정적으로 유지**할 수 있다.

**특히**, 내부에서 사용하는 속성이나 로직에 의존하는 경우에 외부에서의 **잘못된 속성 사용을 방지**하는 데 유용하다❗️

### 👏 useId 훅

```jsx
type CheckboxProps = Omit<React.ComponentPropsWithoutRef<"input">, "type"> & {
  type: "checkbox",
  label: string,
  id: string,
};

const Checkbox = ({ label, id, ...rest }: CheckboxProps) => {
  return (
    <div className="flex cursor-pointer items-center gap-2">
      <input
        className="h-5 w-5 cursor-pointer appearance-none rounded-lg border-2 border-gray-300 bg-gray-200 checked:bg-blue-500"
        id={id}
        {...rest}
      />
      <label htmlFor={id} className="cursor-pointer text-lg">
        {label}
      </label>
    </div>
  );
};

export default Checkbox;
```

위 코드에서 `id`값을 항상 넘겨서 받아 처리해야 하는 귀찮음이 존재한다.

React에서는 `useId`훅이 존재한다. 이 훅은 고유한 `id`를 생성해준다. 이를 통해서 `id` 값을 명시적으로 전달하지 않아도 컴포넌트에서 고유한 `id`를 사용할 수 있다.

```jsx
import { useId } from "react";

type CheckboxProps = Omit<
  React.ComponentPropsWithoutRef<"input">,
  "type" | "id"
> & {
  type: "checkbox",
  label: string,
};

const Checkbox = ({ label, ...rest }: CheckboxProps) => {
  const id = useId();

  return (
    <div className="flex cursor-pointer items-center gap-2">
      <input
        className="h-5 w-5 cursor-pointer appearance-none rounded-lg border-2 border-gray-300 bg-gray-200 checked:bg-blue-500"
        id={id}
        {...rest}
      />
      <label htmlFor={id} className="cursor-pointer text-lg">
        {label}
      </label>
    </div>
  );
};

export default Checkbox;
```

위와 같이 React에 내장된 훅을 사용해서 각 컴포넌트가 고유한 `id`를 가지도록 보장한다.

<br />

## 💧 조건부 렌더링

React에서 조건부 렌더링은 특정 조건에 따라 컴포넌트나 요소를 렌더링할지 여부를 결정하는 방법이다. JavaScript의 조건문을 활용하여 JSX에서 동적으로 UI를 렌더링할 수 있다.

가장 흔히 사용되는 조건부 렌더링 방식에는 `if`문, 삼항 연산자, 논리 연산자 등이 있다.

> 💪 **if문을 사용한 조건부 렌더링**

```jsx
const Welcome = ({ isLoggedIn }: { isLoggedIn: boolean }) => {
  if (isLoggedIn) {
    return <h1>환영합니다!</h1>;
  } else {
    return <h1>로그인 해주세요.</h1>;
  }
};

const App = () => {
  const isLoggedIn = false;

  return (
    <div>
      <Welcome isLoggedIn={isLoggedIn} />
    </div>
  );
};

export default App;
```

> 💪 **삼항 연산자를 사용한 조건부 렌더링**

삼항 연산자(`condition ? ex1 : ex2`)를 사용하면 짧고 간결히 작성할 수 있다.

```jsx
const Welcome = ({ isLoggedIn }: { isLoggedIn: boolean }) => {
  return <h1>{isLoggedIn ? "환영합니다!" : "로그인 해주세요."}</h1>;
};

const App = () => {
  const isLoggedIn = false;

  return (
    <div>
      <Welcome isLoggedIn={isLoggedIn} />
    </div>
  );
};

export default App;
```

> 💪 **논리 연산자 `&&`를 사용한 조건부 렌더링**

논리 연산자 `&&`를 사용하면 조건이 참일 때만 특정 요소를 렌더링할 수 있다.

```jsx
const Notification = ({ isUnreadMessages }: { isUnreadMessages: boolean }) => {
  return (
    <div>
      {isUnreadMessages && <p>읽지 않은 메시지가 있어요! 확인 부탁드립니다!</p>}
    </div>
  );
};

const App = () => {
  const unreadMessages = 5;

  return (
    <div>
      <Notification isUnreadMessages={unreadMessages > 0} />
    </div>
  );
};

export default App;
```

위와 같이 조건에 따라서 컴포넌트나 요소를 렌더링할 수 있다. 이를 통해 다음에 배울 React 훅을 사용하여 동적으로 변화하는 UI를 제작할 수 있다.

<br />

## 💧 반복 렌더링

React에서 반복 렌더링은 배열이나 객체를 반복해 여러 개의 컴포넌트를 렌더링할 때 사용된다. 이 때 `key` 속성을 사용해서 각 항목에 고유한 식별자를 부여해야 한다.

> 🤘 **배열 반복 렌더링**

```jsx
const App = () => {
  const fruits = ["사과", "바나나", "수박", "파인애플"];

  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
};

export default App;
```

위 코드는 배열의 각 항목을 `li`로 렌더링하고 `key`값을 `index`로 사용하고 있다.

> 🤘 **객체 반복 렌더링**

```jsx
const people = [
  { id: 1, name: "조병찬", age: 25 },
  { id: 2, name: "조붕", age: 26 },
  { id: 3, name: "붕라니", age: 20 },
];

const App = () => {
  return (
    <ul>
      {people.map((person) => (
        <li key={person.id}>
          {person.name} - {person.age}살
        </li>
      ))}
    </ul>
  );
};

export default App;
```

> 🤘 **key로 index를 사용하면 안되는 이유**

`key`는 React가 리스트를 렌더링할 때 각 항목을 식별하는 데 사용되는 고유한 식별자이다. React는 `key`를 기반으로 어떤 항목이 추가, 변경, 제거되었는지를 추적한다.

하지만 `key`로 `index`를 사용하면 안된다.

💩 **리스트의 순서가 변경될 때 문제 발생**

`key`로 `index`를 사용하면 리스트의 순서가 변경될 때 React는 항목들이 재정렬되었음을 인식하지 못하고 잘못된 요소를 업데이트할 수 있다.

```jsx
import { useState } from "react";

const initialItems = ["사과", "바나나", "수박", "파인애플"];

const FruitList = () => {
  const [fruits, setFruits] = useState(initialItems);

  const removeItem = (indexToRemove: number) => {
    setFruits(fruits.filter((_, index) => index !== indexToRemove));
  };

  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>
          {fruit}
          <button onClick={() => removeItem(index)}>과일 제거</button>
        </li>
      ))}
    </ul>
  );
};

export default FruitList;
```

위처럼 `index`를 `key`로 사용하면 항목을 제거할 때 리스트의 나머지 항목들의 `index`가 재정렬된다.

이 때 React는 항목의 순서만 바뀐 것이 아니라 모든 항목이 새롭게 갱신된 것으로 오해할 수가 있다. 결과로 예상치 못한 UI 업데이트나 성능의 저하가 이루어질 수 있게 된다.

💩 **동일 값이 여러 번 나타날 때 문제 발생** <br />

```jsx
const fruits = ["사과", "바나나", "바나나", "파인애플"];
```

위와 같이 배열에 동일 값이 여러 번 나타날 경우, `index`를 `key`로 사용하면 React는 각 항목을 고유하게 식별하지 못할 수 있다.

> 🤘 **그럼 어떻게 하는데요**

데이터가 고유한 `id`값을 포함하는 것을 `key` 값으로 사용하면 된다. ~~정말 간단하죠? 키키~~

무작위 값을 사용할 수도 있지만 비추천한다. **그러니 고유한 것으로 알잘딱깔센 느낌 몽말알❓**

---

# 3️⃣ 느낀점

오늘은 어제에 이어 React의 기본 문법에 대해 이어갔다. `children`에 대해 배우고 간단한 실습을 하였다. 지금까지 배운 React의 문법을 사용하여 재사용 가능한 컴포넌트를 만들었다.

또한, 위 실습을 통한 재사용성에 대한 고찰을 하였다. `Omit`이라는 TypeScript 유틸 함수를 사용하여 의도치 않은 속성 사용을 방지하였다.

그리고 조건부, 반복 렌더링 등을 살펴보며 React의 기본 문법을 익혔다.

![image](https://github.com/user-attachments/assets/33c34fff-4772-4747-b196-da7115a8d1ac) <br />

수업시간 내내 저기 가운데서 춤을 추었다 왕귀염😮

내일도 화이팅 해봅시다람지 💫👋

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
