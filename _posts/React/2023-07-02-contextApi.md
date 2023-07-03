---
title: 'React-ContextApi'
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-07-02
---

참고  
[https://velog.io/@benis/Context-API](https://velog.io/@benis/Context-API)

# Context API

> context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있습니다.

리액트에서 제공하는 built-in API로 State 관리를 외부 라이브러리 없이 할 수 있다.  
또한 리액트에서 Context API를 위해 훅스도 제공한다.

State Management 라이브러리로는 redux, Mobx, Recoil.js 등이 있다.

# context가 해결해주는 문제

> props drilling을 막는 데 도움을 줍니다.

## props drilling

중첩된 여러 계층의 컴포넌트에게 props를 전달해 주는것을 의미합니다. (해당 porps를 사용하지 않는 컴포넌트들에게 까지)

```jsx
export default function App({ theme }) {
  return (
    <>
      <Header theme={theme} />
      <Main theme={theme} />
      <Sidebar theme={theme} />
      <Footer theme={theme} />
    </>
  );
}
function Header({ theme }) {
  return (
    <>
      <User theme={theme} />
      <Login theme={theme} />
      <Menu theme={theme} />
    </>
  );
}
```

# how to use ?

## 1. createContext

redux에서의 store와 같은 하나의 Context 객체가 필요합니다.

```jsx
import { createContext } from 'react';

export const MyContext = createContext(1);
```

createContext를 실행하면 Provider와 Consumer을 담고 있는 컨텍스트 객체가 생성됩니다.

Provider는 state나 action.type에 따른 dispatch 함수들을 value prop에 넣어서 제공하는 역할입니다.

Consumer는 Provider에 담긴 state와 dispatch 함수들을 필요한 컴포넌트에서 접근할 수 있게 만드는 역할입니다.

## 2. Provider 생성

위 처럼 context 객체를 생성하고 export 하였다면 Provider를 생성할 수 있습니다.

Provider는 context의 뿌리라고 할 수 있습니다. 필요한 모든 것을 담고 있고, Consumer로 wrapping된 컴포넌트는 Provider에 접근할 수 있습니다.

Provider를 redux처럼 자식 컴포넌트들을 wrapping 합니다.

여기서 약간의 차이점이라면 redux는 하나의 store를 사용하는 것이 기본적인 룰인데 반해, Context API는 다수의 Context를 만들 수 있습니다.

```tsx
const MarketContext = createContext();
function foo() {
  const [cartState, cartDispatch] = useReducer(cartReducer, initialState.cartItems);

  const [checkState, checkDispatch] = useReducer(
    checkReducer,
    cartState.map((el) => el.itemId)
  );

  return (
    <MarketContext.Provider
    // value={{
    //   items: items,
    //   cart: { cartState, cartDispatch },
    //   check: { checkState, checkDispatch },
    // }}
    >
      {children}
    </MarketContext.Provider>
  );
}
```

## 3. Context에 접근 with Hooks

리액트의 빌트인 훅인 useContext를 통해 컴포넌트는 간단하게 자신을 wrapping 하고 있는 Provider의 value에 접근 가능합니다.

```tsx
function ItemList() {
  const { items, cart, check } = useContext(MarketContext);
  const { cartDispatch, cartState } = cart;
  const { checkDispatch } = check;
  return (
    <div id='item-list-container'>
      <div id='item-list-body'>
        <div id='item-list-title'>선물 모음</div>
        {items.map((item, idx) => (
          <Item item={item} key={idx} handleClick={() => handleClick(item.id)} />
        ))}
      </div>
    </div>
  );
}
```

useContext훅은 createContext를 사용하고 리턴받은 객체를 인자로 받습니다.  
이렇게 해서 ItemList는 state, dispatch 함수에 직접 접근을 할 수 있습니다.

# Context API 단점

Provider의 value prop에 있는 state와 dispatch가 변할 떄 마다, Provider를 구독하고 있는 모든 컴포넌트들이 리렌더링 됩니다.  
useMemo를 통해 Provider의 value props를 메모이제이션 하거나, 독립적인 context를 만들어주는 방법이 있습니다.

# Context + useReducer

둘을 함꼐 사용하면 더 직관적이고 코드 양도 눈에 띄게 줄어듭니다.

```jsx
const cartReducer = (state, { type, id, quantity }) => {
  switch (type) {
    case ADD_ITEM:
      return [...state, { itemId: id, quantity: 1 }];
    case INCRE_QUANTITY:
      return state.map((item) =>
        item.itemId === id ? { ...item, quantity: item.quantity + 1 } : item
      );
    case DELETE_ITEM:
      return state.filter((item) => item.itemId !== id);

    case CHANGE_QUANTITY:
      return state.map((item) =>
        item.itemId === id ? { ...item, quantity } : item
      );
    default:
      return state;
  }
};


export const MarketContextProvider = ({ children }) => {
  const [cartState, cartDispatch] = useReducer(
    cartReducer,
    initialState.cartItems
  );

  const [checkState, checkDispatch] = useReducer(
    checkReducer,
    cartState.map((el) => el.itemId)
  );
```
