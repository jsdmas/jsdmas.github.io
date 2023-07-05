---
title: 'React-ContextApi'
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-07-05
---

# Context API

> Context API란 React에서 제공하는 내장 API로서 컴포넌트들에게 동일한 Context(맥락)을 전달하는데 사용할 수 있습니다.

일반적으로 리액트에서 데이터를 전달하는 기본 원칙은 `단방향성`입니다. 그 말은 부모 컴포넌트에서 자식 컴포넌트 방향으로만 데이터를 전달할 수 있다는 의미입니다.

단방향성은 애플리케이션의 안정성을 높이고 흐름을 단순화하는데 유용하지만 때떄로 너무 많은 단계를 거쳐서 자식 컴포넌트에 데이터를 전달해야 한다는 문제점을 야기하기도 합니다.(props drilling)

예를들어 5단계 아래에 위치한 자식 컴포넌트에게 데이터를 넘겨야 한다면, 중간에 4개의 컴포넌트는 해당 데이터를 사용하지 않을지라도 props를 계속해서 넘겨줘야하는 문제가 발생하는 것입니다. 또한, 형제 관계나 특정 범위 안에 있는 컴포넌트들에게 데이터를 넘기기 위해서는 더 복잡한 상황이 발생하기도 합니다.

컴포넌트의 구조를 잘 설계하고 합성을 적극적으로 활용해 데이터를 계속해서 넘겨줘야 하는 상황을 안만드는 것이 1옵션이지만, 해당 방법으로 해결이 안될 때는 Context API를 사용할 수 있습니다.

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

Context API를 사용하기 위해서는 먼저 공유할 Context를 만들어줘야 합니다.  
Context는 `createContext`라는 함수를 통해서 사용할 수 있습니다.

```jsx
import { createContext } from 'react';

const MyContext = createContext(1);
```

- createContext 함수를 호출하면 Context 객체가 리턴됩니다.
- 함수를 호출할 때는 defaultValue를 인자로 전달할 수 있습니다.

**이떄 defaultValue는 Context Value의 초기값이 아닌, 다른 컴포넌트에서 Context에 접근하려고 하지만 Provider로 감싸져 있지 않은 상황에서 사용될 값을 의미합니다.**

## 2. Provider 생성

만들어진 Context를 통해서 특정한 값을 전달하기 위해서는 Provider 컴포넌트를 이용해야 합니다.

Context 객체에는 Provider라는 프로퍼티가 있으며 이는 리액트 컴포넌트입니다.

Provider 컴포넌트는 value라는 props을 가지고 있으며, value에 할당된 값을 Provider 컴포넌트 하위에 있는 어떤 컴포넌트든 접근할 수 있게 해주는 기능을 가지고 있습니다.

```jsx
const UserContext = createContext(null);

const user = { name: 'yeonuk' };

<UserContext.Provider value={user}>
  <Child />
</UserContext.Provider>;
```

## 3. useContext

Class 컴포넌트에서 Context를 통해 공유된 값에 접근하려면, Consumer라는 다소 복잡한 방식을 통해서 접근해야 합니다. 하지만 함수 컴포넌트에서는 useContext라는 내장 Hook을 이용해 Context Value에 접근할 수 있습니다.

```tsx
const UserContext = createContext(null);

const user = { name: 'yeonuk' };

<UserContext.Provider value={user}>
  <Child />
</UserContext.Provider>;

function Child() {
  const user = useContext(UserContext);

  return <h1>{user.name}</h1>;
}
```

# Context API 단점

Provider의 value prop에 있는 state와 dispatch가 변할 떄 마다, Provider를 구독하고 있는 모든 컴포넌트들이 리렌더링 됩니다.  
useMemo를 통해 Provider의 value props를 메모이제이션 하거나, 독립적인 context를 만들어주는 방법이 있습니다.
