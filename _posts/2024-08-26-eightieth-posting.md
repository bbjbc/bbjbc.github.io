---
title: "[유데미x스나이퍼팩토리] 프로젝트 캠프: React 2기 - 사전직무교육 6일차"
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

# 여든 번째 포스팅

안녕하세요! 여든 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 여든💫

오늘의 포스팅 내용은 **[유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 - 사전직무교육 6일차**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ React 훅

## 💧 useEffect

`useEffect`는 React의 함수형 컴포넌트에서 사이드 이펙트를 관리하기 위해 사용하는 훅이다.

이 훅은 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 하거나, 특정 상태나 props가 변경될 때만 작업을 수행하도록 설정할 수 있다. 이를 통해서 데이터 페칭, 구독, 수동으로 DOM을 조작하는 등 다양한 효과를 가질 수 있도록 한다.

```jsx
useEffect(()=>{
  // 수행할 작업 (side effect)

  return () => {
      // Cleanup 작업 (선택)
  }
}, [의존성 배열])
```

위 구조에서 첫 번째 매개변수로 전달된 함수가 실제로 수행할 작업을 정의한다. 이 함수는 컴포넌트가 처음 렌더링될 때 실행되고, 이후 의존성 배열에 명시된 값이 변경될 때마다 다시 실행된다.

컴포넌트가 언마운트되거나 다음 효과가 실행되기 전에 정리 작업을 할수 있는 함수도 반환할 수 있다.

> 👦 **동작 원리가 몽데요**

1. **초기 렌더링 시 실행**
   - 컴포넌트가 처음 렌더링될 때 `useEffect`의 콜백 함수가 실행된다.
2. **의존성 배열**
   - `useEffect`의 두 번째 매개변수로 의존성 배열을 전달할 수 있다. 이 배열에 포함된 값이 변경될 때마다 `useEffect`가 다시 실행된다.
   - 의존성 배열이 빈 배열로 설정되면, `useEffect`는 컴포넌트가 처음 렌더링될 때만 실행되고, 그 이후에는 다시 실행되지 않는다.
   - 의존성 배열을 생략하면 `useEffect`는 컴포넌트가 렌더링될 때마다 실행된다.
3. **정리 작업**
   - `useEffect`는 컴포넌트가 언마운트될 때 또는 다음 번에 동일한 `useEffect`가 다시 실행되기 전에 선택적으로 정리 작업을 수행할 수 있다. 이것은 메모리 누수를 방지하고, 이전에 설정한 효과를 제거하는 데 유용하다.

> 👦 **예제**

```jsx
import { useState, useEffect } from "react";

const CountLogger = () => {
  const [count, setCount] = useState(0);

  // 컴포넌트가 마운트될 때와 언마운트될 때 실행되는 useEffect
  useEffect(() => {
    console.log("Component has mounted!");

    return () => {
      console.log("Component will unmount!");
    };
  }, []);

  // count 값이 변경될 때마다 실행되는 useEffect
  useEffect(() => {
    console.log(`Count: ${count}`);

    return () => {
      console.log(`Cleanup after count changed: ${count}`);
    };
  }, [count]);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
};

export default CountLogger;
```

위 예제는 두 개의 `useEffect`를 사용하여 첫 번째 `useEffect`는 컴포넌트가 마운트될 때와 언마운트될 때 실행되고, 두 번째 `useEffect`는 `count` 상태가 변경될 때마다 실행된다.

첫 번째 `useEffect`에서는 의존성 배열을 빈 배열로 설정했기 때문에 컴포넌트가 처음 렌더링될 때 한 번만 실행된다.

![image](https://github.com/user-attachments/assets/acd5e41a-7b3e-478b-a798-647fb312a9e4) <br />

처음 마운트 될 때의 모습이다. 이후 버튼을 눌러보겠다.

두 번째 `useEffect`에서는 `count` 상태가 변경될 때마다 새로운 `count` 값으로 렌더링 및 콘솔에 출력된다.

![image](https://github.com/user-attachments/assets/563c1d99-fd95-454f-81b0-c8d5bb72c7b0) <br />

위와 같이 첫 번째 `useEffect`의 콘솔은 출력되지 않고 `count` 값만 바뀌었으니 두 번째의 `useEffect`만 작동하는 것을 확인할 수 있다.

또한 두 번째 `useEffect`에서 두 번째 콘솔부터 출력되는 이유는 React의 `useEffect`가 동작하는 방식 때문이다. 상태 변경이 일어나면 이전 값에 대한 **클린업 함수**가 먼저 실행되고, 이후에 새로운 값에 대한 **효과 함수**가 실행된다.

이로 인해 이전 값에 대한 콘솔 로그가 먼저 출력되고 그 후에 새로운 값에 대한 로그가 출력되는 것이다❗️

<br />

## 💧 useLayoutEffect

`useLayoutEffect`훅은 컴포넌트가 **렌더링된 직후**에 동기적으로 실행되는 효과를 처리할 때 사용한다. `useEffect`와 비슷하게 작동하지만, 브라우저가 화면에 **변경 사항을 페인팅 직전**에 실행된다는 점에서 차이가 있다.

이 훅은 주로 DOM 조작이나 레이아웃 측정과 같은 작업을 수행할 때 사용된다. 이런 작업은 브라우저가 화면에 변화를 그리기 전에 이루어져야 한다.

> 🙆 **`useLayoutEffect`와 `useEffect`의 차이가 몽데요**

`useEffect`는 **컴포넌트가 렌더링된 후에 비동기적으로 실행**된다. 브라우저가 렌더링을 완료하고 화면에 변경 사항을 페인팅한 뒤에 실행된다. 이는 사용자에게 더 나은 성능을 제공할 수 있다. (ex. API 호출, 이벤트 리스너 등록, 외부 데이터 업데이트 등 브라우저가 렌더링한 후에 수행할 수 있는 작업에 사용됨)

`useLayoutEffect`는 컴포넌트가 **렌더링된 직후에 동기적으로 실행**된다. 브라우저가 화면을 페인팅하기 전에 실행되기 때문에 DOM 요소를 직접 조작하거나 레이아웃을 계산해야 할 때 유용하다. (ex. 스크롤 위치 설정, 레이아웃 계산 등 레이아웃의 변경이 화면에 그려지기 전에 이루어져야 함.)

{% raw %}

```tsx
import { useState, useRef, useLayoutEffect } from "react";

import { twMerge as cn } from "tailwind-merge";

const App = () => {
  const buttonRef = useRef<HTMLButtonElement>(null);
  const [buttonOffset, setButtonOffset] = useState(0);

  useLayoutEffect(() => {
    if (buttonRef.current) {
      // 버튼의 너비를 측정하고 중앙으로 이동하기 위해 offset 설정
      const { width } = buttonRef.current.getBoundingClientRect();
      setButtonOffset((window.innerWidth - width) / 2);
    }
  }, []); // 빈 배열로 설정하여 마운트 시에만 한 번 실행

  return (
    <div className="pt-32">
      <button
        ref={buttonRef}
        className={cn("relative transition-transform duration-300")}
        style={{
          transform: `translateX(${buttonOffset}px)`,
        }}
      >
        중앙 버튼
      </button>
    </div>
  );
};

export default App;
```

{% endraw %}

위 예제는 GPT 선생님께서 짜주신 예제이다.

![예제](https://github.com/user-attachments/assets/3135e8ff-31e7-41b0-bf52-704f019c6248) <br />

위와 같이 동작하게 된다.

컴포넌트가 마운트된 후에 버튼의 너비를 계산하고, 이를 이용해 버튼이 화면 중앙에 위치하도록 스타일을 조정한다.

`useLayoutEffect`는 화면에 변경 사항이 그려지기 전에 동기적으로 실행되므로, 버튼의 위치가 화면에 깜빡임 없이 바로 반영되는 것이다. 신기하지❗️ㅋㅋ

> 🙆 **이건 언제 사용하는 게 좋은데용**

`useLayoutEffect`는 일반적으로 DOM 요소의 크기, 위치 등을 계산하고 이를 기반으로 스타일을 설정하거나 조정해야 할 때 사용한다.

- 컴포넌트가 마운트된 후 DOM 요소의 크기를 측정해야 할 때
- 스크롤 위치를 계산하고 설정해야 할 때
- 레이아웃이 변경되기 전에 DOM을 직접 조작해야 할 때

하지만 불필요하게 `useLayoutEffect`를 남발하면 성능 저하를 초래할 수 있다. 그러니 꼭 필요한 경우에만 사용하는 것이 좋다. 일반적인 효과나 비동기 작업에는 `useEffect`를 사용하는 것이 적절하다❗️

<br />

## 💧 예제(장바구니)

```jsx
const Basket = () => {
  return (
    <>
      <div className="flex items-center justify-center min-h-screen bg-gray-500">
        <div className="flex gap-4">
          <div className="bg-white p-6 rounded-lg shadow">
            <h2 className="text-xl font-bold mb-4">Add New Item</h2>
            <form className="grid gap-4">
              <div className="grid grid-cols-2 gap-4">
                <div className="space-y-2">
                  <label className="text-sm font-medium" htmlFor="name">
                    Name
                  </label>
                  <input
                    className="flex h-10 w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
                    id="name"
                    name="name"
                    autoComplete="off"
                  />
                </div>
                <div className="space-y-2">
                  <label className="text-sm font-medium" htmlFor="quantity">
                    Quantity
                  </label>
                  <input
                    className="flex h-10 w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
                    id="quantity"
                    type="number"
                    name="quantity"
                    autoComplete="off"
                  />
                </div>
              </div>
              <div className="space-y-2">
                <label className="text-sm font-medium" htmlFor="description">
                  Description
                </label>
                <textarea
                  className="flex min-h-[80px] w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
                  id="description"
                  name="description"
                  autoComplete="off"
                />
              </div>
              <div className="space-y-2">
                <label className="text-sm font-medium" htmlFor="price">
                  Price
                </label>
                <input
                  className="flex h-10 w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
                  id="price"
                  type="number"
                  name="price"
                  autoComplete="off"
                />
              </div>
              <button className="inline-flex items-center justify-center rounded-md text-sm font-medium bg-[#f0f0f0] hover:bg-primary/90 h-10 px-4 py-2">
                Add Item
              </button>
            </form>
          </div>
          <div className="bg-white p-6 rounded-lg shadow">
            <h2 className="text-xl font-bold mb-4">Current Inventory</h2>
            <div className="mb-4">
              <input
                className="flex h-10 rounded-md border bg-background px-3 py-2 text-sm w-full"
                placeholder="Search inventory..."
                type="search"
                autoComplete="off"
              />
            </div>
            <div className="overflow-auto">
              <div className="relative w-full overflow-scroll max-h-[280px]">
                <table className="w-full caption-bottom text-sm">
                  <thead>
                    <tr className="border-b">
                      <th className="h-12 px-4 font-medium cursor-pointer">
                        Name
                      </th>
                      <th className="h-12 px-4 font-medium cursor-pointer">
                        Description
                      </th>
                      <th className="h-12 px-4 font-medium cursor-pointer">
                        Quantity
                      </th>
                      <th className="h-12 px-4 font-medium cursor-pointer">
                        Price
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr className="border-b">
                      <td className="p-4 font-medium">Widget A</td>
                      <td className="p-4">
                        A high-quality widget for industrial use
                      </td>
                      <td className="p-4 text-right">50</td>
                      <td className="p-4 text-right">$9.99</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </>
  );
};

export default Basket;
```

위의 지저분하고 정리 되지 않은 코드에서 폼을 제출하면 `inventory`에 렌더링 되도록 하고 `inventory`에서 입력 결과에 따라 필터링 되어 렌더링 되도록 만들어야 한다.

그래서 먼저 컴포넌트를 분리하고 작업을 해야 한다.

> 💻 **`basket.ts`**

```ts
export type Item = {
  name: string;
  quantity: number;
  description: string;
  price: number;
};

export type AddItemProps = {
  onAddItem: (item: Item) => void;
};

export type InventoryProps = {
  items: Item[];
};
```

위와 같이 타입을 한 번에 한 파일 안에 지정해준다.

> 💻 **`add-item.tsx`**

```jsx
import { useState } from "react";

import { twMerge as cn } from "tailwind-merge";

import { AddItemProps } from "../../types/basket";

const AddItem = ({ onAddItem }: AddItemProps) => {
  const [formState, setFormState] = useState({
    name: "",
    quantity: "",
    description: "",
    price: "",
  });

  const onChangeHandler = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ) => {
    const { name, value } = e.target;
    setFormState({
      ...formState,
      [name]: value,
    });
  };

  const submitHandler = (e: React.FormEvent) => {
    e.preventDefault();

    const { name, quantity, description, price } = formState;
    if (name && quantity && description && price) {
      const newItem = {
        name,
        quantity: Number(quantity),
        description,
        price: Number(price),
      };
      onAddItem(newItem);
      setFormState({
        name: "",
        quantity: "",
        description: "",
        price: "",
      });
    }
  };

  const isFilled =
    formState.name &&
    formState.quantity &&
    formState.description &&
    formState.price;

  return (
    <div className="rounded-lg bg-white p-6 shadow">
      <h2 className="mb-4 text-xl font-bold">새 상품 추가하기</h2>

      <form className="grid gap-4" onSubmit={submitHandler}>
        <div className="grid grid-cols-2 gap-4">
          <div className="space-y-2">
            <label className="text-sm font-medium" htmlFor="name">
              상품명
            </label>
            <input
              className="flex h-10 w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
              id="name"
              name="name"
              value={formState.name}
              placeholder="상품명을 입력하세요"
              onChange={onChangeHandler}
              autoComplete="off"
            />
          </div>

          <div className="space-y-2">
            <label className="text-sm font-medium" htmlFor="quantity">
              수량
            </label>
            <input
              className="flex h-10 w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
              id="quantity"
              type="number"
              name="quantity"
              value={formState.quantity}
              placeholder="상품의 수량을 입력하세요"
              onChange={onChangeHandler}
              autoComplete="off"
            />
          </div>
        </div>

        <div className="space-y-2">
          <label className="text-sm font-medium" htmlFor="description">
            설명
          </label>
          <textarea
            className="flex min-h-[80px] w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
            id="description"
            name="description"
            value={formState.description}
            placeholder="상품의 설명을 입력하세요"
            onChange={onChangeHandler}
            autoComplete="off"
          />
        </div>

        <div className="space-y-2">
          <label className="text-sm font-medium" htmlFor="price">
            가격
          </label>
          <input
            className="flex h-10 w-full rounded-md border bg-[#f0f0f0] px-3 py-2 text-sm"
            id="price"
            type="number"
            name="price"
            value={formState.price}
            placeholder="상품의 가격을 입력하세요"
            onChange={onChangeHandler}
            autoComplete="off"
          />
        </div>

        <button
          className={cn(
            "inline-flex h-10 items-center justify-center rounded-md px-4 py-2 text-sm font-medium",
            !isFilled
              ? "cursor-not-allowed bg-gray-200 text-gray-500"
              : "tra nsition-all bg-gray-700 text-white duration-300 hover:bg-gray-400"
          )}
        >
          상품 추가하기
        </button>
      </form>
    </div>
  );
};

export default AddItem;
```

위와 같이 Item을 추가할 컴포넌트를 따로 분리한다. `onChangeHandler`를 통해서 여러 개의 상태를 지정할 필요가 없다.

> 💻 **`inventory.tsx`**

```tsx
import { useState } from "react";

import { InventoryProps } from "../../types/basket";

const Inventory = ({ items }: InventoryProps) => {
  const [searchTerm, setSearchTerm] = useState<string>("");

  const searchChangeHandler = (e: React.ChangeEvent<HTMLInputElement>) => {
    setSearchTerm(e.target.value);
  };

  const filteredItems = searchTerm
    ? items.filter((item) =>
        item.name.toLowerCase().includes(searchTerm.toLowerCase())
      )
    : items;

  return (
    <div className="rounded-lg bg-white p-6 shadow">
      <h2 className="mb-4 text-xl font-bold">내 장바구니 목록</h2>
      <div className="mb-4">
        <input
          className="bg-background flex h-10 w-full rounded-md border px-3 py-2 text-sm"
          placeholder="상품을 검색해보세요!"
          type="search"
          onChange={searchChangeHandler}
          autoComplete="off"
        />
      </div>
      <div className="overflow-auto">
        <div className="relative max-h-[280px] w-full overflow-scroll">
          <table className="w-full caption-bottom text-sm">
            <thead>
              <tr className="border-b">
                <th className="h-12 cursor-pointer px-4 font-medium">상품명</th>
                <th className="h-12 cursor-pointer px-4 font-medium">설명</th>
                <th className="h-12 cursor-pointer px-4 font-medium">수량</th>
                <th className="h-12 cursor-pointer px-4 font-medium">가격</th>
              </tr>
            </thead>
            <tbody>
              {items.length === 0 ? (
                <tr>
                  <td colSpan={4} className="animate-pulse py-4 text-center">
                    상품이 존재하지 않습니다.
                  </td>
                </tr>
              ) : (
                filteredItems.map((item, index) => (
                  <tr key={index} className="border-b">
                    <td className="p-4 font-medium">{item.name}</td>
                    <td className="p-4">{item.description}</td>
                    <td className="p-4 text-right">{item.quantity}</td>
                    <td className="p-4 text-right">
                      {item.price.toLocaleString()}원
                    </td>
                  </tr>
                ))
              )}
            </tbody>
          </table>
        </div>
      </div>
    </div>
  );
};

export default Inventory;
```

오른쪽에 표 형식으로 렌더링될 컴포넌트를 위와 같이 작성한다.

> 💻 **`basket.tsx`**

```jsx
import { useState } from "react";

import AddItem from "./add-item";
import Inventory from "./inventory";
import { Item } from "../../types/basket";

const Basket = () => {
  const [items, setItems] = useState<Item[]>([]);

  const addItemHandler = (item: Item) => {
    setItems([...items, item]);
  };

  return (
    <>
      <div className="flex min-h-screen items-center justify-center bg-gray-500">
        <div className="flex gap-4">
          <AddItem onAddItem={addItemHandler} />
          <Inventory items={items} />
        </div>
      </div>
    </>
  );
};

export default Basket;

```

위와 같이 이제 나누었던 컴포넌트를 하나의 컴포넌트 안에 받아낸다. 이 곳에서 `items`에 관한 상태를 통해 전달 받아 렌더링한다.

> 💻 **`App.tsx`**

```jsx
import Basket from "./components/basket/basket";

const App = () => {
  return <Basket />;
};

export default App;
```

마무리로 `App` 컴포넌트에 띄운다.

![basket](https://github.com/user-attachments/assets/d6d56d3f-65e4-4c0a-845e-f1f5375224a3) <br />

위는 최종 동작하는 영상이다. 이상.

<br />

## 💧 예제(ToDoList)

퍼블리싱 된 코드는 긴 관계로 여기에선 생략하도록 하겠다.

> 💻 **`to-do.tsx`**

```tsx
import { useState } from "react";

import TodoForm from "./to-do-form";
import ToDoList from "./to-do-list";
import { TodoItem } from "../../types/todo";

const Todo = () => {
  const [todos, setTodos] = useState<TodoItem[]>([]);

  const addTodo = (text: string) => {
    setTodos([...todos, { id: Math.random(), text, completed: false }]);
  };

  const toggleTodo = (id: number) => {
    const newTodos = todos.map((todo) =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    );
    setTodos(newTodos);
  };

  const removeTodo = (id: number) => {
    setTodos(todos.filter((todo) => todo.id !== id));
  };

  return (
    <div className="flex min-h-screen items-center justify-center bg-black">
      <div className="inter h-[502px] w-[375px] rounded-lg border border-[#d1d1d1] bg-white px-[25px] py-10 text-[#4f4f4f]">
        <h1 className="mb-[10px] text-xl font-bold">투두 리스트</h1>
        <p className="mb-5 text-sm">할 일을 등록해보아요</p>
        <TodoForm onAddTodo={addTodo} />
        <ToDoList
          todos={todos}
          onToggleTodo={toggleTodo}
          onRemoveTodo={removeTodo}
        />
      </div>
    </div>
  );
};

export default Todo;
```

위 컴포넌트는 대부분의 상태를 관리하는 컴포넌트이다. 매우 중요한 컴포넌트이다.

> 💻 **`to-do-form.tsx`**

```jsx
import { useState } from "react";

import { twMerge } from "tailwind-merge";

import Button from "../child/button";
import Input from "../child/input";
import { ToDoFormProps } from "../../types/todo";

const ToDoForm = ({ onAddTodo }: ToDoFormProps) => {
  const [todoInput, setTodoInput] = useState("");

  const onChangeHandler = (e: React.ChangeEvent<HTMLInputElement>) => {
    setTodoInput(e.target.value);
  };

  const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    if (todoInput.trim()) {
      onAddTodo(todoInput);
      setTodoInput("");
    }
  };

  return (
    <form action="" className="grid gap-4" onSubmit={submitHandler}>
      <div className="flex gap-2">
        <Input
          type="text"
          placeholder="당신의 투두 리스트를 입력하세요"
          value={todoInput}
          onChange={onChangeHandler}
        />
        <Button
          type="submit"
          className={twMerge(
            "w-[77px] shrink-0 rounded-lg text-white",
            todoInput.trim()
              ? "bg-[#4f4f4f]"
              : "cursor-not-allowed bg-[#d1d1d1]"
          )}
        >
          추가
        </Button>
      </div>
    </form>
  );
};

export default ToDoForm;
```

위 컴포넌트는 `form`을 관리하는 컴포넌트이다. `input` 값에 따라 `form`을 제출할 수 있도록 한다.

> 💻 **`to-do-list.tsx`**

```jsx
import { twMerge } from "tailwind-merge";
import { FaTrashAlt } from "react-icons/fa";

import Checkbox from "../child/checkbox";
import Button from "../child/button";
import { ToDoListProps } from "../../types/todo";

const ToDoList = ({ todos, onToggleTodo, onRemoveTodo }: ToDoListProps) => {
  return (
    <ul className="mt-4 flex max-h-[284px] flex-col gap-4 overflow-y-auto">
      {todos.length === 0 ? (
        <p className="flex animate-bounce justify-center pt-20 text-base">
          할 일이 없어요
        </p>
      ) : (
        <>
          {todos.map((todo) => (
            <li
              key={todo.id}
              className="flex h-12 shrink-0 select-none items-center justify-between rounded-lg border border-[#4F4F4F] bg-[rgba(53,56,62,0.05)] px-[15px]"
            >
              <Checkbox
                type="checkbox"
                checked={todo.completed}
                onChange={() => onToggleTodo(todo.id)}
              >
                <span
                  className={twMerge(
                    "text-[#35383E]",
                    todo.completed ? "line-through" : ""
                  )}
                >
                  {todo.text}
                </span>
              </Checkbox>
              <Button
                className="flex h-[23px] w-10 items-center justify-center rounded border border-[#4f4f4f] transition-all duration-200 hover:bg-slate-300"
                onClick={() => onRemoveTodo(todo.id)}
              >
                <span className="relative">
                  <FaTrashAlt />
                </span>
              </Button>
            </li>
          ))}
        </>
      )}
    </ul>
  );
};

export default ToDoList;
```

위 컴포넌트는 제출한 폼에 따라 체크박스가 렌더링된다. 체크박스와 관련된 `li` 태그를 별도의 컴포넌트로 분리할 수도 있지만, 현재 상태에서는 두 개 이상의 컴포넌트를 거쳐 `props`를 전달하게 되어 `prop-drilling`이 발생한다. 하지만 이 경우는 `prop-drilling`이 짧은 편이므로 굳이 컴포넌트를 분리하지 않아도 된다고 생각한다.

향후 상태 관리 라이브러리나 더 복잡한 상태 관리 방법을 배우게 되면, 그 때 더욱 적절한 방법으로 상태를 관리하고 컴포넌트를 분리하는 것이 좋을 것 같다.

> 💻 **`todo.tsx`**

```ts
export type TodoItem = {
  id: number;
  text: string;
  completed: boolean;
};

export type ToDoFormProps = {
  onAddTodo: (text: string) => void;
};

export type ToDoListProps = {
  todos: TodoItem[];
  onToggleTodo: (index: number) => void;
  onRemoveTodo: (index: number) => void;
};
```

위는 각 타입을 모아둔 파일이다.

![todo](https://github.com/user-attachments/assets/9078a83f-9683-4547-97a5-dbbd36ece463) <br />

위는 구현한 모습 영상이다.

<br />

## 💧 useTransition

`useTransition` 훅은 React 18에서 도입된 훅으로 비동기적인 상태 업데이트를 다루는 데 도움을 준다. 이 훅은 상태 업데이트가 느리거나 시간이 걸릴 수 있는 작업을 최적화하여 사용자 경험을 개선하는 데 유용하다.

이 훅은 상태 업데이트를 비동기적으로 처리할 수 있도록 하여, 사용자 인터페이스의 응답성을 높인다. 비동기 상태 업데이트는 UI가 즉시 업데이트 되는 것이 아니라, 사용자가 상호작용할 때 UI가 매끄럽게 변하도록 한다.

또한, 상태 업데이트가 완료되기 전에 로딩 상태를 관리할 수 있도록 한다. 이를 통해 사용자에게 로딩 알림을 제공할 수 있다.

```jsx
import { useState } from "react";

const UseTransition = () => {
  const [input, setInput] = useState("");
  const [dummy, setDummy] = useState<number[]>([]);

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setInput(e.target.value);
    const i: number[] = [];
    for (let j = 0; j < 20000; j++) {
      i.push(j);
    }
    setDummy((d) => [...d, ...i]);
  };
  return (
    <>
      <h1>UseTransition Hook</h1>
      <input type="text" value={input} onChange={onChange} />
      <ul>
        {dummy.map((d) => (
          <li key={d}>{d}</li>
        ))}
      </ul>
    </>
  );
};
export default UseTransition;
```

위와 같이 코드를 작성하면 정말 오래 걸리는 것을 확인할 수 있다.

![useTransition1](https://github.com/user-attachments/assets/1d6fb056-971a-4b55-b734-cf40636b96ea) <br />

여기서 보이는 것처럼 입력은 미리 했는데 굉장히 오랜 기다림 끝에 렌더링 되는 것을 확인할 수 있다.

```jsx
import { useState, useTransition } from "react";

const UseTransition = () => {
  const [input, setInput] = useState("");
  const [dummy, setDummy] = useState<number[]>([]);
  const [isPending, startTransition] = useTransition();

  const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setInput(e.target.value);

    startTransition(() => {
      const i: number[] = [];
      for (let j = 0; j < 20000; j++) {
        i.push(j);
      }
      setDummy((d) => [...d, ...i]);
    });
  };

  return (
    <>
      <h1>UseTransition Hook 사용하기</h1>
      <input type="text" value={input} onChange={onChange} />
      {!isPending ? (
        <ul>
          {dummy.map((d, i) => (
            <li key={i}>{d}</li>
          ))}
        </ul>
      ) : (
        <p>로딩 중입니다...</p>
      )}
    </>
  );
};

export default UseTransition;

```

위와 같이 `useTransition` 훅을 사용하여 `isPending`과 `startTransition`을 반환한다. `isPending`은 트랜지션이 진행 중인지 여부를 나타내며, `startTransition`은 비동기 상태 업데이트를 시작한다.

`isPending`이 `true`가 되어 로딩 인디케이터를 표시하는 것이다.

![useTransition2](https://github.com/user-attachments/assets/5d4abca7-9d7e-473d-9de9-8904a09fd722) <br />

위 코드를 실행하면 위와 같이 로딩 인디케이터를 볼 수 있다.

### 💢 동시성 모드가 몽데요

`useTransition`훅은 React의 동시성 모드와 함계 사용될 때 유용하다. **동시성 모드는 React의 렌더링 작업을 보다 효율적으로 관리할 수 있게 도와주는 기능**으로, 사용자 인터페이스의 반응성을 개선하고 복잡한 상태 업데이트를 보다 원활하게 처리할 수 있다.

1. **비동기 렌더링**- React는 동시성 모드에서 여러 렌더링 작업을 동시에 진행할 수 있다. 이는 상태 업데이트가 긴 작업으로 인해 UI가 멈추는 것을 방지한다.
2. **우선 순위 조정**- 동시성 모드는 렌더링 작업의 우선 순위를 조정할 수 있다. 사용자가 상호작용할 때 중요한 업데이트를 우선적으로 처리하고, 덜 중요한 업데이트는 나중에 처리한다.
3. **중단 가능한 렌더링**- 렌더링 작업을 중단하고, 더 중요한 작업이 필요할 때 작업을 재개할 수 있다. 이는 긴 렌더링 작업이 사용자 상호작용을 방해하지 않도록 한다.

**결론적으로 `useTransition` 훅을 통해 동시성 모드의 장점을 활용하면 React 애플리케이션의 성능과 사용자 경험을 향상시킬 수 있다❗️**

---

# 2️⃣ 느낀점

오늘은 어제에 이어 `useEffect`, `useLayoutEffect`, `useTransition` 훅에 대해 학습했다.

지금까지 배웠던 내용을 토대로 원래 예제의 난이도보다 조금 더 응용된 실습 2개를 진행하며 `props의 전달법`, `useState의 활용`, `useEffect의 활용`에 대해서 더욱 더 정확하게 알 수 있었다.

이러한 실습을 진행하며 복습이 중요하다는 것을 깨달았고 더욱 연습이 필요함을 느끼게 되었다.

![image](https://github.com/user-attachments/assets/eaf8430f-1700-486e-bf44-35a3c4f07fa3)<br />

내일은 드디어 팀 배정 결과가 나온다. 좋은 사람들과 팀이 되도록 기도해보며, 누구와 팀이 되든 최선을 다할 것이다 🔥👋

---

> **본 후기는 본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : React 2기 과정(B-log) 리뷰로 작성 되었습니다.**
