---
title: 'Redux Middleware'
toc: true
toc_sticky: true
categories:
  - Redux
tags:
  - Redux
last_modified_at: 2023-07-21
---

# Middleware

미들웨어란 무엇일까요? 미들웨어는 **“프레임워크의 요청과 응답 사이에 추가할 수 있는 코드”**라고 생각할 수 있습니다.

일반적으로 express, koa와 같은 서버 프레임워크단에서 미들웨어란 개념을 많이 사용하는데 서버 프레임워크에서는 흔히 미들웨어에서 CORS 관련 설정, 로깅 등의 목적으로 활용합니다.

미들웨어의 가장 큰 특징은 “연결" 할 수 있다는 점입니다. 각각의 미들웨어는 서로 독립적이며, 프레임워크 안에 여러개의 미들웨어를 추가해서 연결할 수 있습니다. 이로 인해 개발자는 미들웨어를 기반으로 일련의 흐름을 작성하듯이 프로그램을 설계할 수 있게 됩니다.

이런 특징으로 인해 JavaScript를 이용한 서버 프레임워크 중 가장 유명한 express 같은 경우에는 기본적인 핵심 기능만 포함하고 있으며 그 외의 코드들은 모두 express에 middleware를 추가하는 방식으로 설계를 하게됩니다.

리덕스의 미들웨어도 마찬가지입니다. 서버단의 미들웨어와 다른점은 서버의 미들웨어는 요청과 응답 사이에서 동작하지만 **리덕스의 미들웨어는 액션이 디스패칭되어서 reducer에 전달되는 과정 사이에서 동작**한다는 점입니다.

# In Depth of Middleware

미들웨어는 리덕스를 이용하는데 필수적인 요소는 아닙니다. 다만 미들웨어를 통해서 리덕스를 좀 더 편리하게 사용할 수 있습니다. 기술의 필요성을 직관적으로 이해하려면 해당 기술이 없었을 때를 상상하거나, 실제 구현해보면 좋습니다.

실제 미들웨어가 없이 아래의 동작을 구현해보겠습니다.

1. Dispatch된 Action을 로깅한다.
2. Action이 Reducer로 전달되어서 처리된 후, state를 로깅한다.

### Solution 1. Logging Manually

```js
const increaseAction = increaseCounter();

console.log('dispatching', increaseAction);

store.dispatch(increaseAction);

console.log('next state', store.getState());
```

첫번쨰로 생각할 수 있는 가장 단순한 방법은, 매번 액션을 Dispatch 하기 전후로 직접 log를 출력하는 것입니다. 하지만 매번 액션을 Dispatch 하는 코드마다 손수 위와 같은 코드를 작성하는 것은 그닥 바람직한 방법은 아닙니다.

### Solution 2. Wrapping Dispatch

두번쨰 방법은 `store.dispatch` 메서드를 감싸는 함수를 만들고, 그 함수 안에서 로깅 동작을 추가하는 것입니다.

```js
function dispatchAndLog(store, action) {
  console.log('dispatching', action);
  store.dispatch(action);
  console.log('next state', store.getState());
}
```

이 방법은 Solution 1보다는 발전했지만, `store.dispatch`를 사용하는것이 아니라, 매번 dispatchAndLog라는 함수를 따로 import 해서 사용해야 한다는 단점이 있습니다. (실제 나중에 React-Redux등의 useDispatch와 같은 형태로 사용하려면 더 까다로워집니다.)

### Solution 3. Monkeypatching Dispatch

세번째 방법은 `store.dispatch` 메서드를 몽키패칭 하는 것입니다. 몽키패칭이란, 라이브러리나, 프레임워크단의 코드 동작을 직접 수정해서 사용하는 것을 의미합니다. 이 경우에는 `store.dispatch`함수를 수정해서 활용할 수 있습니다.

```js
const originDispatch = store.dispatch;
store.dispatch = function dispatchAndLog(action) {
  console.log('dispatching', action);

  const result = originDispatch(action);

  console.log('next state', store.getState());
  return result;
};
```

이 방법은, 매번 로깅하는 코드를 작성하지 않아도 되며, 디스패치를 사용하는 입장에서 원래의 디스패치 메서드가 아닌 Wrapping한 디스패치 함수를 import해서 사용하는데에 주의를 기울여도 되지 않는다는 장점이 있습니다. 따라서 위의 3가지 솔루션 중 가장 좋은 방법이라고 생각할 수 있습니다.

### Q. 여러개의 기능이 필요하다면?

‼️ 단, 1가지 기능만 추가할경우에만 가장 좋은 방법입니다.

만약 디스패치 메서드에 로깅 외에, try, catch 구문을 이용해 예외가 발생했을 시 error 로그를 출력하는 동작을 추가하려면 어떻게 해야할까요?

```js
function patchStoreToAddLogging(store) {
  const next = store.dispatch;

  store.dispatch = function dispatchAndLog(action) {
    console.log('dispatching', action);
    let result = next(action);
    console.log('next state', store.getState());
    return result;
  };
}

function patchStoreToAddCrashReporting(store) {
  const next = store.dispatch;

  store.dispatch = function dispatchAndReportErrors(action) {
    try {
      return next(action);
    } catch (err) {
      console.error('에러 발생', err);
      throw err;
    }
  };
}

// enhance
patchStoreToAddLogging(store);
patchStoreToAddCrashReporting(store);
```

위와 같은 방식으로 각각의 기능에 맞게 patching하는 함수를 작성한 후, 직접적으로 개별 함수를 실행해줘야 합니다.  
이러한 방식은

1. 함수 내에서 `store.dispatch`메서드를 직접 수정하고 있기에 사이드 이펙트를 발생시키고 있습니다. 이는 추후 프로그램의 동작을 예측하기 어렵게 만들기에 유지보수에 좋지 않습니다.
2. patching하는 함수들을 특정 위치에서 직접 개별적으로 모두 실행해줘야 합니다.

### Solution 4. Hiding Monkeypatching

위의 문제를 해결하기 위한 방법으로 몽키패칭을 적용하는 부분을 별도 함수로 분리하는 방법이 있습니다.  
여러개의 패칭 함수를 연결해서 수행하기 위해서, dispatch를 바로 함수 안에서 수정하는 게 아니라

1. 패칭함수에서는 wrapping된 dispatch 함수를 return 한다.
2. 몽키패칭을 적용시켜주는 함수에서는 return된 dispatch함수를 `store.dispatch`에 패칭한다.

```js
function logger(store) {
  const next = store.dispatch;

  return function dispatchAndLog(action) {
    console.log('dispatching', action);
    const result = next(action);
    console.log('next state', store.getState());
    return result;
  };
}

function crashReporter(store) {
  const next = store.dispatch;

  return function dispatchAndReportErrors(action) {
    try {
      return next(action);
    } catch (err) {
      console.error('에러 발생', err);
      throw err;
    }
  };
}
```

```js
function applyMiddlewareByMonkeypatching(store, middlewares) {
  copiesOfMiddlewares = [...middlewares];

  /*
  middleware는 순차적으로 실행되며,
  마지막 middleware는 원래의 store.dispatch를 호출해줘야 한다.
  dispatch 함수를 patching하는 과정은 가장 끝 미들웨어부터 이루어져야 한다.
  따라서, 역순으로 정렬시킨 후 patching 한다.
  */
  copiesOfMiddlewares.reverse();

  // store.dispatch 메서드를 middleware 함수의 return 값으로 변경한다.
  copiesOfMiddlewares.forEach((middleware) => (store.dispatch = middleware(store)));
}

applyMiddlewareByMonkeypatching(store, [logger, crashReporter]);
```

### Solution 5. Remove Monkypatching

몽키패칭을 수행해서 `store.dispatch`를 직접 변경시키는 것은, 결국 라이브러리단의 코드의 동작을 직접 수정해버린다는 위험을 가지고 있으며, `store.dispatch`가 지속적으로 변하기 때문에, 그로 인한 버그가 발생하기 쉽다는 취약점을 가지고 있습니다. 그리고 기존의 `store.dispatch`를 덮어씌워버리기 떄문에, 추후 만약 기존의 `dispatch`메서드가 필요한 순간이 발생할 경우 이에 대응하기 어렵다는 문제를 가지고 있습니다.

근본적으로 우리가 몽키패칭을 수행하는 방법을 선택한 이유는 무엇일까요? 우리는 여러개의 함수들이 순차적으로 `store.dispatch`에 특정한 동작을 추가하기 위해서 몽키패칭을 수행했습니다.  
하지만 조금만 더 생각해보면, 여러개의 함수들이 순차적으로 동작을 추가하기 위해서는 몽키패칭 외에 다른 방법을 선택할 수 있습니다.

바로, 미들웨어에서 patching된 함수를 리턴하고, 이를 다음 미들웨어의 인자로 전달하는 방식입니다.

- 기존방식
  - 미들웨어 함수에 `store`만 인자로 전달한다.
  - 미들웨어에서는 원하는 동작을 수행한 뒤 `store.dispatch`를 호출해준다.
- 새로운 방식
  - 미들웨어 함수에 `store`와, `next`를 인자로 전달한다.
  - 미들웨어에서는 원하는 동작을 수행한 뒤 next 함수를 호출해준다.
  - next는 미들웨어가 동작을 수행한 뒤 호출할 함수를 의미한다.

**currying, 함수형 프로그래밍 기법, 인자를 부분적으로만 넣을 수 있다.**

```js
function logger(store) {
  return function wrapDispatchToAddLogging(next) {
    return function dispatchAndLog(action) {
      console.log('dispatching', action);
      const result = next(action);
      console.log('next state', store.getState());
      return result;
    };
  };
}

function crashReporter(store) {
  return function wrapDispatchToCrashReporting(next) {
    return function dispatchAndReportErrors(action) {
      try {
        return next(action);
      } catch (err) {
        console.error('에러 발생', err);
        throw err;
      }
    };
  };
}
```

**arrow function style**

```js
const logger = (store) => (next) => (action) => {
  console.log('dispatching', action);
  let result = next(action);
  console.log('next state', store.getState());
  return result;
};
```

**apply middleware**

```js
function applyMiddleware(store, middlewares) {
  copiesOfMiddlewares = [...middlewares];
  copiesOfMiddlewares.reverse();
  // dispatch를 변수로 선언하고, 초기값으로는 store.dispatch를 할당한다.
  let dispatch = store.dispatch;

  // dispatch 변수의 값을 middleware 함수의 리턴값으로 재할당한다.
  copiesOfMiddlewares.forEach((middleware) => {
    dispatchEnhancer = middleware(store);
    dispatch = dispatchEnhancer(dispatch);
  });

  return { ...store, dispatch };
}

applyMiddleware(store, [logger, crashReporter]);
```

# Middleware를 사용하는 이유

위와 같은 과정을 거쳐서 미들웨어는 탄생되었고, 이 과정을 이해하면 이제 개발자들은 각자의 미들웨어를 구현할 수 있습니다. 그런데 근본적으로 왜 미들웨어를 사용할까요?  
리덕스를 사용하면서 미들웨어를 사용하는 이유는 크게 두가지로 나눠볼 수 있습니다.

1. 횡단 관심사를 처리하기 위해서
2. 관심사의 분리를 실현하기 위해서

## 횡단 관심사를 처리하기 위해서

횡단 관심사란 애플리케이션의 여러 서비스에 걸쳐서 공통적으로 실행되어야 하는 코드를 의미합니다.  
위의 예시에서 본 로깅, 크래쉬 리포트 등이 횡단 관심사라고 할 수 있습니다. 이러한 코드들을 매번 필요한 곳에 작성하면서 중복을 발생시키는 것이 아니라, 미들웨어를 통해서 리덕스의 중간 과정에서 모두 처리하게 된다면 훨씬 깔끔하고 좋은 설계를 가진 애플리케이션으로 만들 수 있습니다.

## 관심사의 분리를 실현하기 위해서

프론트엔드 애플리케이션을 개발하면서 API 호출 등의 사이드 이펙트를 완전히 배제할 순 없습니다.  
특정 순간에는 반드시 이러한 사이드 이펙트를 수행해야하는 순간이 발생합니다. API 호출 등의 비동기 처리를 동반한 사이드 이펙트 뿐만 아니라 동기적인 코드일지라도 상태값을 수정하기 위해서 복잡한 로직이 수행되어야 하는 경우도 존재합니다. 리덕스를 사용하면서 이런 동작들은 어디서 수행해줘야 할까요?

관심사의 분리와 응집도 측면에서 보자면, 결국 상태값을 수정하는 것에 연관된 동작이기에 상태를 가지고 있는 리덕스 내에서 처리하는 것이 올바른 접근법으로 보입니다.  
하지만 리덕스의 모든 과정은 순수하게 이루어집니다. 순수하게 이루어진다는 의미는 사이드 이펙트를 수행할 공간이 없다는 것입니다.

리덕스는 순수한 객체인 액션을 디스패치를 통해서 전달하고 순수함수인 리듀서에서 이를 처리해서 새로운 상태값을 만드는 흐름을 따릅니다.  
이 과정 중간에 사이드 이펙트나, 복잡한 로직을 처리하기에 적절한 공간은 존재하지 않습니다.

이러한 동작을 수행하기 위한 가장 단순한 방법은 리액트 컴포넌트 안에서 사이드 이펙트를 모두 다 수행한 뒤, 그 결과를 통해서 액션을 만들고 디스패치 하는 것입니다.  
위와 같은 방법으로 코드를 작성해도 해도 실제 동작상에는 아무런 차이가 없습니다. 하지만, 리덕스 스토어의 상태를 다루기 위한 관심사가 컴포넌트와 리덕스 두가지 공간으로 분리되서 작성된다는 점, 컴포넌트에서 다루는 관심사가 데이터를 UI로 표출하는 것뿐만 아니라 상태값을 변경하는 것도 다루게 된다는 점 등으로 인해 관심사의 분리가 제대로 되지 않는 방식이라는 한계가 있습니다.

이러한 문제를 해결하기 위해서, 리덕스의 미들웨어를 활용할 수 있습니다.

# Implement Middleware

미들웨어를 만들기 위해서는 아래의 동작 과정을 이행하는 함수를 만들면 됩니다.

1. `store`를 인자로 받아서 `next`를 인자로 받는 또다른 함수를 리턴한다.
2. `next`를 인자로 받아서, `action`을 인자로 받는 또다른 함수를 리턴한다.
3. `action`을 인자로 받아서, 원하는 동작을 수행한 후 `next`에 `action`을 인자로 호출하여 다음 미들웨어로 전달하는 함수를 리턴한다.

이 과정을 거쳐 로그를 기록하는 미들웨어를 만들면 아래와 같습니다.

```js
const logger = (store) => (next) => (action) => {
  console.group(action.type);
  console.log('dispatching', action);
  next(action);
  console.log('next state', store.getState());
  console.groupEnd();
};
```

이제, 사이드 이펙트를 미들웨어단에서 수행하기 위한 방법을 고민해볼 차례입니다. 먼저 기존에 알고 이해해야 하는 정보는 아래와 같습니다.

- 리덕스에서 리듀서에 전달되는 액션(origin dispatch를 총해서 전달된)은 반드시 단순한 형태의 객체여야 합니다.
- 하지만, 미들웨어가 받는 액션은 반드시 객체일 필요는 없습니다.
- 미들웨어에서 `next`를 호출하지 않으면 해당 액션은 다음 미들웨어로 전달되지 않습니다. 이말은 곧 특정 미들웨어에서 액션이 리듀서로 전달되지 않게 취소해버릴수도 있다는 의미입니다.
- 미들웨어에는 store객체를 인자로 받았기에 이에 접근할 수 있습니다. 그말은 즉, `store.dispatch`를 직접 호출할수도 있다는 의미입니다. 리덕스 내부에서는 미들웨어에서 `store.dispatch`를 호출했을 경우 해당 액션을 가장 처음 미들웨어부터 다시 실행해주는 동작을 수행합니다.
  - (이 페이지에서 구현한 applyMiddelware의 경우에는 이 동작을 수행해주지 않지만, 실제 리덕스에서 제공해주는 applyMiddleware 함수에서는 위와 같은 동작을 수행해줍니다.)

위의 정보들을 종합하면, action으로 우리가 원하는 동작을 수행하는 함수를 전달한 뒤, 미들웨어에서 해당 함수를 호출하고 그 결과값을 다시 `store.dispatch`를 통해서 전달하는 방법을 사용할 수 있습니다.

이러한 동작을 수행하는 미들웨어가 바로 redux-thunk입니다. thunk의 경우 npm에서 인스톨해서 사용할수도 있지만, 간단하기에 직접 만들수도 있습니다.

```js
const thunk = (store) => (next) => (action) => {
  if (typeof action === 'function') {
    action(store.dispatch, store.getState);
  } else {
    next(action);
  }
};

// use thunk
const fetchSomeData = (dispatch, getState) => {
  client.get('todos').then((todos) => {
    dispatch({ type: 'todos/loadTodos', payload: todos });
  });
};

function Component() {
  useEffect(() => dispatch(fetchSomeData), [fetchSomeData]);
}
```

# Redux DevTools

- Redux의 설계는 예측가능성을 가장 큰 중점으로 두고 설계했으며 그로 인해 애플리케이션의 동작을 분석하고, 예측하는 디버깅을 수행하기 용이합니다.
- 이런 특성을 인해 디버깅 기능을 잘 활용할 수 있는 Redux DevTools란 개발자 도구가 존재합니다.
- 브라우저의 익스텐션을 통해 리덕스의 동작을 분석하고, 추가적인 동작을 수행할 수 있도록 할 수 있습니다.
- 이를 사용하기 위해서는 브라우저에서 익스텐션을 설치하고, 코드에서 DevTools 사용 설정을 해주어야 합니다.
- 코드에서 편리하게 사용 설정을 하기 위해서는 `@redux-devtools/extension` 라이브러리를 활용할 수 있습니다.

```js
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from '@redux-devtools/extension';

const store = createStore(
  reducer,
  composeWithDevTools(
    applyMiddleware(...middleware)
    // other store enhancers if any
  )
);
```

# Ref

[https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)  
[https://github.com/reduxjs/redux-devtools/tree/main/extension#13-use-redux-devtoolsextension-package-from-npm](https://github.com/reduxjs/redux-devtools/tree/main/extension#13-use-redux-devtoolsextension-package-from-npm)
