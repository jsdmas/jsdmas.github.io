---
title: 'Redux ?'
toc: true
toc_sticky: true
categories:
  - Redux
tags:
  - Redux
last_modified_at: 2023-07-16
---

# Design Pattern

소프트웨어를 설계하며 자주 발생하는 문제에 대한 모법답안을 `디자인 패턴`이라고 합니다.  
이런 디자인 패턴중에는 일부 코드를 해결하기 위한 비교적 작은 범위를 다루는 것들도 존재하며, 애플리케이션 전체를 설계할 떄 사용하는 큰 범위를 다루는 것들도 존재합니다.

애플리케이션 전체를 다루기 위한 디자인 패턴들은 통상 여러 작은 범위의 디자인 패턴들을 함계 사용해서 만들어지기에 복합패턴이라고도 부릅니다.  
시간이 흐름에 따라 개발자들이 다루는 애플리케이션의 규모는 점점 다양해지고 거대해지며 자연스레 이를 해결하기 위한 여러가지 디자인 패턴들도 많이 나오게 되었습니다.  
그 중 모든 복합 패턴의 근간이라고 부를 수 있는 패턴이 **MVC 패턴**입니다.

# MVC

![](https://github.com/jsdmas/jsdmas.github.io/assets/105098581/0238029d-4296-429e-81ba-f8a9af391aea)

MVC 패턴은 애플리케이션을 Model, View, Controller 세가지 구성요소로 나눠서 설계하는 패턴입니다.  
구성요소들의 역할은 아래와 같습니다.

1. Model : 데이의 형태를 정의하고, 데이터를 수정하는 역할을 담당
2. Controller : 유저의 입력을 받아서 애플리케이션 내에서 어떻게 처리할지 판단 및 가공해서 모델 또는 뷰를 조작
3. View : 모델을 UI로 표현, 사용자의 입력을 받아서 Controller에 전달

View는 기본적으로 Model에게 접근해서 데이터를 받아오지만, 때로는Controller에서 Model을 거치지 않고 바로 View를 변화시키기도 합니다.

MVC 패턴에서 각 구성요소들은 서로에게 접근할 수 있습니다. 즉, `양방향 통신`이 발생하고 있습니다.

MVC 패턴은 애플리케이션의 구성요소를 역할에 따라 분리하였습니다. 그리고 모든 애플리케이션은 결국 본질적으로 데이터(모델)을 잘 조작해서 화면(View)에 보여주는 것이기에 모든 디자인패턴의 대부, 근본이 MVC 패턴이 되었습니다.

하지만, 애플리케이션의 규모가 커지고, 세분화되면서 각 영역에 따라서 전통적인 MVC 패턴을 그대로 사용하기에는 어려움을 겪기도 합니다. 특히 프론트엔드라는 분야가 새롭게 생겨났고, 이 프론트엔드 내에서는 유저와 수많은 상호작용이 발생하고, 데이터를 빈번하게 수정해야 했으며 백엔드로부터 받아오는 데이터와, 클라이언트단에서 관리하는 데이터들을 모두 잘 관리해야 하는 복잡한 요구사항에 직면하게 됩니다.

그리고 이러한 복잡한 상황속에서 프론트엔드에서 기존의 MVC 패턴을 그대로 사용하기는 한계가 있었습니다.

React를 만들고, 유지보수하는 Meta(구 Facebook)에서도 이러한 어려움을 겪게 됩니다.

당시 Facebook에서는 Direct Message 기능이 있었습니다. 그리고 이 DM 기능은 처음에는 단순하게 시작했지만, 시간이 흐르면서 채팅창 목록을 볼 수 있어야 하고, 안 읽은 메세지를 카운트 할 수 있어야 하는 등 여러 요구사항이 추가되면서 복잡한 시스템이 되어갔습니다. 그런데 이런 기능을 개발해서 운영하던 중 `안 읽은 메세지가 존재한다는 표시가 있는데 실제 채팅 시스템을 들어가서 확인해보면 안읽은 메세지가 없는`버그가 발생하게 됩니다.

개발자들을 이러한 버그를 인식하고 해결했지만, 해결 뒤에도 시간이 조금 흐르면 동일한 버그가 계속해서 발생하게 됩니다. Meta팀 개발자들은 이렇게 동일한 버그가 계속 발생하고, 이를 해결하지 못하는 근본적 원인을 `MVC 패턴의 양방향성`으로 정의하게 됩니다.

![](https://github.com/jsdmas/jsdmas.github.io/assets/105098581/2a1829fe-a131-42c0-98cb-2eb853cb9040)

MVC 패턴은 각 구성요소들끼리 양방향으로 통신하게 되어있습니다. 이러한 양방향 통신은 애플리케이션의 규모가 단순할때는 큰 문제가 되지 않습니다. 하지만, 시간이 흐름에 따라 많은 Model과 View들이 생기게 되면 `특정 View A에서 발생한 동작이 Model A를 수정하게 되고, Model A가 수정되었으니 이에 영향을 받는 View B가 수정되게 되고, View B가 수정되니 Model B를 수정하게 되고 ....` 등 연쇄적인 변화가 발생하게 되고 결국 애플리케이션의 동작 흐름을 분석하거나 예측할 수 없는 문제가 발생하게 됩니다.

# Flux

이러한 상황속 당시의 페이스북 개발자들은 이 문제를 해결하기 위한 새로운 디자인 패턴이 필요하다 생각하게 되었습니다. 그리고 시간이 흐른 뒤 새로운 디자인 패턴을 발표하면서 그 이름을 Flux라고 지었습니다.

Flux의 핵심은 단방향입니다. 앞서 문제의 원인을 양방향으로 인한 연쇄적인 변화로 규정하였기에 자연스레 Flux는 단방향으로 애플리케이션의 변화의 흐름을 최대한 단순화하고 예측가능하게 하는데에 목표를 두었습니다.

좀 더 자세히 알아보자면, Flux 패턴은 4가지 구성요소르 이루어져있습니다. 그리고 각 구성요소들은 한가지 방향으로만 상호작용 하고 있습니다.

![](https://github.com/jsdmas/wanted-pre-onboarding-11th-4week-searchBar/assets/105098581/50071951-d233-4d01-a2d5-c4e2ab9a6072)

- Action : 어떤 변화를 발생시킬지 정의하는 `type` property와 변화에 필요한 데이터를 담고있는 단순한 객체입니다.
- Dispatcher : Action을 받아서 모든 Store에 전달하는 역할을 수행합니다.
- Store : 애플리케이션의 데이터를 저장하고, Dispatch에 전달된 Action에 따라 수정합니다.
- View : Store에 저장된 데이터를 받아서, UI로 표현하고, 유저의 동작에 따라서 Action을 생성합니다.

![](https://github.com/jsdmas/wanted-pre-onboarding-11th-4week-searchBar/assets/105098581/51643965-f068-48a6-8e4e-5cf187ff450b)

단방향성으로 인해 애플리케이션은 특정한 순서에 따라서만 데이터와 UI가 변화하게 되었고 개발자들은 애플리케이션에서 어떤 변화가 일어나는지 파악하기가 쉬워졌으며, 나아가 애플리케이션의 동작을 예측하기도 쉬워졌습니다.

# Redux

Flux가 컨버런스에서 처음 발표되고 난 후, 많은 개발자들이 Flux에 관심을 가지기 시작했습니다. 하지만 Flux는 라이브러리나, 프레임워크가 아닌 디자인 패턴입니다.

즉 Flux 아키텍쳐를 사용하기 위해서는 개발자들이 직접 이 아키텍쳐에 맞게 코드로 구현해야 한다는 뜻입니다. 초창기에는 Flux 패턴을 각자의 방법대로 구현한 여러가지 라이브러리들이 쏟아져나왔습니다. 하지만 현재 Flux 패턴을 근간으로 하는 라이브러리의 표준은 Redux로 정립되었습니다.

Redux는 Flux, CQRS, Event Sourcing의 개념을 사용해서 만든 라이브러리로서 `JavaScript 앱을 위한 예측 가능한 상태 컨테이너`를 핵심 가치로 삼고 있습니다.

Redux는 모든 상태를 관리하는 컨테이너로서의 역할을 수행하고, 애플리케이션 내의 구성요소들은 컨테이너에 접근해서 상태를 읽어올 수 있기에 자바스크립트 앱에서 전역 상태 관리를 수행하기 위해서 사용할 수 있습니다.

Redux는 Flux 패턴의 단방향성을 차용했기에 Redux 내에서 발생하는 상태의 변화는 모두 예측 가능합니다.  
이런 특성으로 인해 프론트엔드에서 발생하는 복잡한 상태들의 변화를 관리하기에 적격으로 판단되었으며, Redux가 발표되고 난 후 한동안 프론트엔드의 상태관리 표준은 Redux로 자리잡았습니다.

## Redux의 3가지 원칙

Redux는 3가지 기본 원칙으로 이루어져있습니다.

### Single source of truth

Redux 내의 모든 전역 상태는 하나의 객체 안에 트리구조로 저장되고, 이 객체를 Store라 부릅니다.  
모든 상태(State)가 하나의 객체에 저장되기에 애플리케이션이 단순해지고, 예측하기 쉬워집니다. 또한 하나의 객체의 변화만 추적하면 되기에 Undo, Redo 등의 기능을 구현하기도 쉬워집니다.

```js
{
  visibilityFilter: 'SHOW_ALL', // state 1
  todos: [                      // state 2
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
```

### State is read-only

Redux의 State를 변화시키는 유일한 방법은 `Action 객체를 Dispatch를 통해서 전달하는 것입니다.`그 외에 Store에 직접 접근해서 상태를 수정하는 등의 행위는 허용되지 않습니다.

Redux는 위와 같이 state를 불변하게 다루고, 변화시킬 수 있는 방법을 제약함으로서 **안정성과 예측 가능성을 중대시킵니다.**

모든 변화는 Dispatch를 통해서 중앙화되고, 순서대로 수행되기에 여러곳에서 동시에 데이터를 수정하면서 발생하는 race condition 문제 등이 발생하지 않게 됩니다.

또한, Action을 통해서 변화의 의도를 표현합니다. Action은 단순한 형태의 객체이기 떄문에 이를 추적하거나, 로깅, 저장하는 등이 동작을 수행하기 용이하기에 디버깅을 손쉽게 할 수 있으며, 추후 테스트 코드를 작성하기도 용이합니다.

```js
const action = {
  type: 'COMPLETE_TODO',
  index: 1,
};

store.dispatch(action);

// ----------------------

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED',
});
```

### Changes are made with pure function

앞서, Redux의 State를 변화시키는 유일한 방법은 Action 객체를 Dispatch를 통해서 전달하는 것이라고 했습니다.  
Action이 Store에 전달된 후 **실질적으로 Action을 통해서 Store를 변경시키는 동작은 Reducer라 불리는 순수함수를 통해서 수행됩니다.**

순수함수란 동일한 Input을 받았을 경우 항상 동일한 Output을 내는것이 보장되어있는 함수를 의미합니다.  
순수함수가 되기 위해서는 함수 내에 사이드이펙트가 없어야 합니다. 만약 사이드 이펙트가 있는 경우 그 함수는 해당 사이드 이펙트에 의해서 같은 Input이라도 다른 Output을 리턴할 수가 있습니다.

```js
// pure function
function sum(x, y) {
  return x + y;
}

sum(1, 2); // 3
sum(1, 2); // 3
sum(1, 2); // 3

// non-pure function
function sum(x) {
  return x + Math.random();
}

sum(1); // ?
sum(1); // ?
sum(1); // ?
```

Reducer는 이전 state 값과, action 객체를 인자로 받아서 새로운 state를 리턴하는 순수 함수입니다.

여기서 주목해야 할 점은 새로운 state를 리턴한다는 점입니다. 즉, 기존의 state 객체를 수정하는 것이 아니라, 기존의 state 객체를 이용해서 새로운 state 객체를 만들어내는식으로 동작한다는 점입니다.

Redux는 Store가 하나이기에 이를 관리하는 Reducer 또한 하나여야 합니다. 하지만 각기 다른 관심사가 하나의 함수에 모두 들어가게 되면 유지보수에 좋지 않기에 애플리케이션이 커지면 여러개의 Reducer 함수(slice reducer)로 분리해서 코드를 작성한 다음 하나의 reducer(root reducer)로 통합하는 방식을 활용합니다.

```js
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter;
    default:
      return state;
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false,
        },
      ];
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true,
          });
        }
        return todo;
      });
    default:
      return state;
  }
}

import { combineReducers, createStore } from 'redux';
// root reducer(visibilityFilter, todos)를 통합해서 rootReducer 생성
const reducer = combineReducers({ visibilityFilter, todos });

// root reducer를 통해서 store 생성
const store = createStore(reducer);
```

## Redux의 구성요소와 데이터 흐름

Redux의 구성요소는 아래와 같습니다.

### View

유저에게 보이는 UI를 의미하며, store의 state를 기반으로 그려집니다.

### Action

type property를 가지고 있는 자바스크립트 객체입니다. Action 객체는 애플리케이션 내에서 어떤 일이 일어났는지를 묘사하는 객체로 생각할 수 있습니다.

type property는 어떤 변화가 발생했는지 묘사하는 string 입니다. 통상 `domain/eventName`의 현태를 따릅니다. 첫번쨰 domain 파트는 이 이벤트가 어떤 카테고리에 속하는지 표시하기 위함이며, eventName은 어떤 일이 발생했는지를 표현하는 부분입니다.

Action 객체는 type property는 필수적으로 포함하고 있어야 하며, 그 외에 추가적으로 전달할 데이터가 있을 시 다른 property를 객체 안에 포함시킬 수 있습니다. 통상적으로 추가적인 정보를 전달하는 property의 이름은 `payload`로 표현합니다.

```js
{
	type: 'TODO/ADD_TODO',
	payload:"Learn Redux"
}

{
  type: 'TODO/SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
}
```

### Action Creator

Action Creator는 Action을 생성하는 함수입니다. 매번 액션 객체를 손수 작성하는 것은 중복이며, 번거롭고, 실수할 여지가 많은 작업이기에 Action Creator를 통해서 생성하는 것이 권장됩니다.

```js
const addTodo = (todo) => {
  return {
    type: 'TODO/ADD_TODO',
    payload: todo,
  };
};
```

### Reducer

Reducer는 이전 state 값과, action 객체를 인자로 받아서 새로운 state를 리턴하는 순수 함수입니다. Reducer를 단순화 해서 표현하자면 `(state, action) => newSTate`의 형태로 표현할 수 있습니다.

Reducer는 아래의 원칙을 따라야 합니다.

- 새로운 state는 오로지 기존의 state와 action 객체를 통해서만 계산되어야 한다. 그 외의 요소들에 영향을 받아서는 안된다.
- Reducer는 기존의 state를 수정해서는 안된다, 기존의 state를 복사하고, 복사한 state에 변화를 발생시킨 후 return 하는 식으로 동작해야 한다.
- Reducer 내부에서는 비동기 통신, 랜덤 값을 사용하는 것 등의 그 어떤 사이드 이펙트도 수행되어서는 안된다.

Reducer 함수는 일반적으로 아래의 과정을 수행합니다.

1. Reducer가 현재 전달받은 Action을 처리할 수 있는지 판단한다.
   - 처리할 수 있을 경우 state와 action을 통해서 새로운 state 객체를 만든 뒤 리턴한다.
2. 처리할 수 없는 Action인 경우에는 기존의 state를 그대로 리턴한다.

```js
const INITIAL_STATE = { value: 0 };

function counterReducer(state = INITIAL_STATE, action) {
  // Reducer가 현재 전달받은 Action을 처리할 수 있는지 판단한다.
  if (action.type === 'counter/increment') {
    // 처리할 수 있을 경우 state와 action을 통해서 새로운 state 객체를 만든 뒤 리턴한다.
    return {
      ...state,
      value: state.value + 1,
    };
  }

  // 처리할 수 없는 Action인 경우에는 기존의 state를 그대로 리턴한다.
  return state;
}
```

### Store

Store는 Redux의 모든 state를 관리하는 객체입니다. Redux에서 store는 createStore 함수에 reducer를 인자로 넣으면서 호출해서 만들 수 있습니다.

store는 `getState`란 메소드를 가지고 있으며 이를 통해 현재의 state값을 가져올 수 있습니다.

```js
import { countReducer } from 'redux';
// root reducer(visibilityFilter, todos)를 통합해서 rootReducer 생성
const reducer = combineReducers({ countReducer });

// root reducer를 통해서 store 생성
const store = createStore(reducer);

// getState 메서드를 통해 현재 state 획득
store.getState(); // {value: 0}
```

### Dispatch

Dispatch는 Store객체에 포함되어있는 메소드입니다. 이 메소드를 통해서 Action 객체를 Store에 전달할 수 있습니다. Dispatch를 통해서 Action이 전달되면 Store는 Reducer를 통해서 새로운 state를 만들어냅니다.

```js
store.dispatch({ type: 'counter/increment' });

store.getState(); // {value: 1}

store.dispatch({ type: 'counter/increment' });

store.getState(); // {value: 2}
```

### Selectors

Selector는 store에서 특정한 state만 가져오기 위한 함수입니다. 단일 store에 모든 state를 담아두기에 애플리케이션이 커질수록 store는 비대해지고, View에서는 이중에서 필요한 state만 가져오는 동작을 계속해서 수행하게 됩니다. 이 동작을 매번 손수 반복하지 않기 위해서 selector 함수를 이용합니다.

```js
const selectCounterValue = (state) => state.value;

const currentValue = selectCounterValue(store.getState());

console.log(currentValue); // 2
```

![](https://github.com/jsdmas/wanted-pre-onboarding-11th-4week-searchBar/assets/105098581/385815cf-0c74-4899-9ea4-b0ca63ccc6ee)

# React-Redux

앞서, Redux는 “자바스크립트 앱을 위한 예측 가능한 상태 컨테이너"라고 했습니다. 이 말은 곧 Redux는 React 전용이 아닌 모든 자바스크립트 앱에 활용할 수 있다는 의미입니다.

React 또한 자바스크립트이기에 Redux를 사용할 수 있습니다. React에서 Redux를 사용하게 되면 **1.전역 상태 관리, 2. Props drilling** 문제를 해결할 수 있게 됩니다.

Redux는 어떤 자바스크립트 앱이든 사용할 수 있도록 범용성을 갖추고 있습니다. 하지만 이를 반대로 생각해보면 React에 최적화되지 않았다는 것을 의미합니다. 이러한 이유로 인해 Redux를 React와 통합하기 위해서는 Redux의 store에 저장된 state를 React 컴포넌트에서 가져올 수 있게 해주고, redux state가 변경될 경우 react state와 마찬가지로 리렌더링 해주는 등의 복잡한 과정을 수행해줘야 합니다.

React-Redux는 이처럼 Redux와 React를 통합하기 위해 필요한 복잡한 과정을 수행해주는 라이브러리입니다. React-Redux는 React에 Redux를 통합하기 위한 여러 함수와 컴포넌트들을 제공해줍니다. 따라서, 통상 React에서 Redux를 사용하기 위해서는 Redux와 React-Redux를 함께 사용하는 것이 일반적입니다.

- Redux: 자바스크립트 앱을 위한 상태 컨테이너
- React-Redux: Redux를 React와 통합하기 위한 라이브러리

## React-Redux에서 제공하는 기능

React-Redux에서 제공하는 대표적인 기능들은 아래와 같습니다.

### Provider

React 컴포넌트들에게 Redux Store에 접근할 수 있는 기능을 제공해주는 컴포넌트입니다. 내부적으로 Context API를 활용하고 있습니다.

```jsx
// 일부 코드 생략

import { Provider } from 'react-redux';
import store from './store/index';
import App from './App';

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

### useSelector

컴포넌트에서 Redux Store의 값을 가져올 수 있는 hook입니다. Redux의 Selector를 React Hook으로 표현한 형태입니다.

```jsx
import { useSelector } from 'react-redux';

const Counter = () => {
  const count = useSelector((state) => state.value);

  return <h1>{count}</h1>;
};
```

### useDispatch

컴포넌트에서 action을 store에 보내기 위한 dispatch 함수를 가져올 수 있는 hook입니다.

```jsx
import { useSelector, useDispatch } from 'react-redux';

const Count = () => {
  const count = useSelector((state) => state.counter);
  const dispatch = useDispatch();

  const increase = () => {
    dispatch({ type: 'counter/increment' });
  };

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={increase}>increment</button>
    </div>
  );
};

export default Count;
```
