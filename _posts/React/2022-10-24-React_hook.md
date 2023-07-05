---
title: 'React 렌더링 최적화를 위한 Hook'
execrpt: 'React 렌더링 최적화를 위한 Advanced Hook'
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-07-05
---

# React.memo?

리액트는 state가 변할 경우 해당 컴포넌트와 하위의 컴포넌트들을 모두 리렌더링 합니다.

그런데 state가 변한 컴포넌트의 경우 당연히 UI의 변화가 있을 것이기에 리렌더링을 해야 하지만, 하위 컴포넌트의 경우에는 props가 변화하지 않았다면 해당 컴포넌트가 UI가 변화하지 않았을 수도 있을 것입니다.

이런 경우 굳이 새롭게 컴포넌트 함수를 호출할 필요 없이 이전에 저장되어 있던 결과를 그대로 사용하는 것이 효율적입니다.

하지만 UI가 실질적으로 변화되었는지 안되었는지를 매번 리액트가 렌더링 과정에서 일일이 모든 컴포넌트 트리를 순회하면서 검사하는 것은 비효율적입니다.

따라서 리액트에서는 개발자에게 이 컴포넌트가 리렌더링이 되어야 할지 아닐지에 대한 여부를 표현할 수 있는 `React.memo`함수를 제공하고 이를 통해 `기존의 컴포넌트의 UI를 재사용할 지 판단하는 방법`을 채택했습니다.

## React.memo

```jsx
const MyComponent = React.memo(function MyComponent(props) {
  // render using props
});
```

React.memo는 HOC(Higher Order Component)입니다.  
HOC란 컴포넌트를 인자로 받아서, 컴포넌트를 리턴하는 컴포넌트입니다.

```js
function HOC(Component) {
  // do something
  return <Component />;
}
```

React.memo로 감싸진 컴포넌트의 경우에는 상위 컴포넌트가 리렌더링 될 경우 무조건 리렌더링 되는 것이 아니라 컴포넌트 이전의 Props와 다음 렌더링 때 사용될 Props를 비교해서 차이가 있을 경우에만 리렌더링을 수행합니다. 만약 차이가 없다면 리렌더링을 수행하지 않고 기존의 렌더링 결과를 재사용합니다. 이를 통해 컴포넌트에서 불필요하게 리렌더링이 되는 경우를 막을 수 있습니다.

이 때 중요하게 생각해야 할 것은 props를 비교하는 방식입니다.  
React.memo는 기본적으로 props의 변화를 이전 prop와 새로운 prop를 각각 `shallow compare`해서 판단합니다. 만약 이 기본적인 비교 로직을 사용하지 않고 비교를 판단하는 로직을 직접 작성하고 싶을 경우를 대비해서 React.memo는 변화를 판단하는 함수를 인자로 받을 수 있도록 설정해뒀습니다.

```jsx
function MyComponent(props) {
  // render using props
}
function areEqual(prevProps, nextProps) {
  // true를 return할 경우 이전 결과를 재사용
  // false를 return할 경우 리렌더링을 수행
}
export default React.memo(MyComponent, areEqual);
```

React.memo의 두번째 인자로 함수를 전달할 경우 해당 함수의 인자로는 이전의 props와 새로운 props가 순서대로 인자로 전달되며, 이 함수의 return 값이 true일 경우 이전 결과를 재사용하고, false를 return할 경우 리렌더리를 수행합니다.

[참고 Repository](https://github.com/jsdmas/React-practice)

## javaScript 데이터 타입

React.memo는 기본적으로 props 객체간을 비교하는 방식을 통해서 동작합니다.  
이를 잘 인식하고 활용하려면 자바스크립트의 데이터 타입, 그중에서도 기본형 타입과 참조형 타입의 차이에 대해서 명확히 알고 있어야 합니다.

자바스크립트의 데이터 타입은 `string, number, boolean, null, undefined, object` 등 다양하게 있습니다. 그리고 이를 크게 두가지로 나누면 기본형 타입과 참조형 타입으로 구분할 수 있습니다.

기본형 타입은 원시형 타입이라는 용어로도 표현합니다. 단어 자체에서도 알 수 있듯이 javaScript에서 지원하는 원시적이고 기본적인 형태의 데이터 타입이며 다른 데이터 없이 해당 데이터 스스로 온전히 존재할 수 있는 형태입니다.

원시형 타입의 예시에는 `string, number, boolean, null, undefined, bigint, symbol`이 있습니다.

참조형 타입은 달리말해 객체형 타입이라고도 불립니다. 즉 원시형 타입을 제외한 `Object`가 참조형 타입이라고 할 수 있습니다.(자바스크립트에서는 Array도 Function도 Object의 한 종류입니다.)참조형 타입의 가장 큰 특징은 다른 데이터들을 모아서 만들어진 타입이라는 것입니다.

`const yeonuk = {name: “yeonuk”, gender:”male”}`

위의 객체를 보면 이 객체는 `"yeonuk", "male"`이란 두가지 string 타입을 모아서 만들어진 것을 확인할 수 있습니다.

참조형 타입과 기본형 타입을 생각할 때 고려해야 할 가장 큰 특징은 `불변성`입니다.

## 불변성

불변성이란 값이 변하지 않는 것을 의미합니다. 기본적으로 원시형 타입은 모두 불변합니다.

```js
let dog = 'tori';
dog = 'mozzi';
```

위 코드에서는 dog에 할당된 'tori'란 string을 'mozzi'란 string으로 변경하는 방식으로 이루어지는 것이 아니라 'mozzi'라는 새로운 string을 만들고, dog에 할당된 값 자체를 `교체`하는 식으로 동작합니다.

이처럼 javaScript에서는 이미 만들어진 원시형 타입을 변경할 수 있는 방법은 없습니다.

하지만 참조형 타입은 가변합니다.

```js
const yeonuk = { name: 'yeonuk', gender: 'male' };

yeonuk.name = 'charlie';
```

객체(참조형 타입)는 여러 타입들을 모아서 만들어진 형태입니다. 따라서 객체 안의 내용물들은 언제든지, 어떤 형태로든 변경할 수 있습니다. 이를 객체가 가변하다고 합니다.

가변성은 메모리를 절약하면서 객체를 유연하게 사용할 수 있게 해주지만, 때때로 결과를 예상하기 힘들다는 단점과 객체간의 비교가 어렵다는 단점을 가지고 있습니다.

javaScript는 기본적으로 비교연산자를 수행할 때 해당 데이터의 `메모리 주소`를 통해서 일치 여부를 판단합니다. `원시형 타입의 경우에는 변경할 시 새로운 데이터가 만들어지고 교체되는 방식이기에 메모리 주소가 달라져서 비교연산자를 활용하기 용이합니다.`

하지만 객체의 경우에는 안의 내용물이 어떻게 바뀌었는지에 상관없이 해당 객체를 가리키는 메모리 주소는 동일하기에 실질적으로 내용이 변했는지를 판단하기는 어렵습니다.

또한 내용물이 완벽히 일치하는 두 객체를 비교하더라도 각 객체를 가리키는 메모리 주소가 다르기에 두 객체는 동일하지 않다는 결과가 나오게 됩니다.

```js
'dog' === 'cat'; // false

const prev = { name: 'prev' };
const next = prev;
next.name = 'next';
prev === next; // true

const one = { hello: 'world' };
const two = { hello: 'world' };

one === two; // false

const nestingObject1 = {
  name: 'hello, world',
  property: {
    id: '1',
  },
};

const nestingObject2 = {
  name: 'hello, world',
  property: {
    id: '2',
  },
};
```

이런 동작으로 인해 실제 객체의 내용물이 같은지 판단하기 위해서는 두 객체 안의 모든 property들을 순회하면서 일일이 비교를 해주어야 합니다.  
만약 property 중에 객체가 존재한다면 또다시 해당 객체를 순회해야 하기에 이 연산을 수행하기 위한 복잡도는 기하급수적으로 늘어납니다.

객체를 가변하게 사용하면 이처럼 객체간의 비교를 하기 힘들어집니다. 하지만, 한번 선언하고 메모리에 저장해 둔 객체를 계속해서 조금씩만 변경하면서 활용할 수 있기 때문에 메모리 용량 측면에서는 효율적입니다.

하지만 최근에는 과거에 비해 객체를 선언하고 저장하는데 사용할 수 있는 메모리 용량이 늘어났기에 메모리의 효율을 추구하기보다는 객체 비교의 편리함을 취하기 위해 객체를 불변하게 활용하는 방식이 최근에는 많이 사용되고 있습니다.

객체를 불변한다는 것은 말 그대로 `한번 만들어진 객체를 수정하지 않는다`는 뜻입니다. 따라서 객체의 내용이 변해야 할 경우에는 원시형 타입과 마찬가지로 기존의 객체를 수정하지 않고 무조건 새로운 객체를 만든 후 교체하는 방식을 적용하게 됩니다.

```js
const prev = { name: 'prev', hello: 'world' };
const next = { ...prev, name: 'next' };

prev === next; // false
```

## memo의 잘못된 활용

React.memo는 기본적으로 props의 변화를 이전 props와 새로운 props를 `shallow compare`해서 판단한다고 했습니다. props를 shallow compare한다는 의미는 아래와 같습니다.

props는 객체 형태로 표현됩니다. 그리고 props 객체는 매 렌더링마다 새롭게 생성됩니다. 따라서 props 객체 자체를 비교하는 것은 의미가 없습니다.

그렇다면 비교해야 하는 것은 props객체 안의 각 property들입니다. 따라서 리액트는 props 객체 안의 각 property들을 `Object.is(===)`연산자를 통해서 비교합니다.

이 중 하나라도 false가 나올 경우 props가 변경되었다고 판단하고 리렌더링을 수행합니다.

```tsx
<Component name="foo" hello="world" object={{first:1, second:2}} array={[1,2,3]}  />

<Component name="bar" hello="world" object={{first:1, second:2}} array={[1,2,3]}  />

const areEqual = (prevProps, nextProps) => {
	if(prevProps.name !== nextProps.name) return false;
	if(prevProps.hello !== nextProps.hello) return false;
	if(prevProps.object !== nextProps.object) return false;
	if(prevProps.array !== nextProps.array) return false;

	return true;
}
```

이러한 동작과 데이터 타입에 대해서 제대로 이해하지 않으면 memo를 잘 못 활용하는 상황이 발생합니다.

# Memoization

Memoization은 특정한 값을 저장해뒀다가, 이후에 해당 값이 필요할 때 새롭게 계산해서 사용하는게 아니라 저장해둔 값을 활용하는 테크닉을 의미합니다.

함수 컴포넌트는 근본적으로 함수입니다. 그리고 리액트는 매 랜더링마다 함수 컴포넌트를 다시 호출합니다. 함수는 기본적으로 이전 호출과 새로운 호출간에 값을 공유할 수 없습니다.

만약 특정한 함수 호출 내에서 만들어진 변수를 다음 함수 호출에도 사용하고 싶다면 그 값을 함수 외부의 특정한 공간에 저장해뒀다가 다음 호출 때 명시적으로 다시 꺼내와야 합니다.

이것을 직접 구현하는 것은 꽤나 번거로운 일이고, 특히 함수 컴포넌트에서 이를 구현하고 관리하는 것은 많은 노력이 드는 행위입니다.

이를 해결하기위해 아래와 같은 hook을 사용합니다.

## useMemo

> useMemo는 리액트에서 값을 memoization 할 수 있도록 해주는 함수입니다.

```js
// useMemo(callbackFunction, deps]

const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

useMemo는 두가지 인자를 받습니다. 첫번쨰 인자는 콜백함수이며 `이 함수에서 리턴하는 값이 메모됩니다.` 두번쨰 인자는 의존성 배열입니다. 메모이제이션을 할 때 주의해야 할 점은 만약 새로운 값을 만들어서 사용해야 하는 상황임에도 불구하고 이전의 결과를 그대로 활용해버리면 버그가 발생할 수 있다는 점입니다.

위의 예시에서 `a,b`라느 두가지 변수를 이용해 메모이제이션 하기 위한 값을 계산하고 있습니다.  
그런데 만약 `a,b`라는 값이 변경되었는데 이전의 값을 그대로 활용해버리면 의도한 결과와 다른 결과가 나오게 될 것입니다.

이런 상황을 방지하기 위해서 useMemo에서는 의존성 배열을 인자로 받아, 의존성 배열에 있는 값 중 하나라도 이전 렌더링과 비교했을 때 변경되었다면 메모된 값을 활용하는 것이 아니라 새로운 값을 다시 계산합니다.

## useCallback

useCallback은 useMemo를 조금 더 편리하게 사용할 수 있도록 만든 버전입니다.  
일반적인 값들은 useMemo를 통해서 메모하기 편리합니다. 하지만 함수의 경우에는 useMemo를 사용해서 메모하게 되면 콜백함수에서 또다른 함수를 리턴하는 형태가 되게 됩니다. 이는 동작상에는 아무런 이상이 없지만 코드 스타일에 따라 문법적으로 다소 보기가 불편해지는 단점이 있습니다. 따라서 이러한 동작을 간소화한 useCallback이란 함수를 만들어서 제공해주고 있습니다.

```tsx
const memorizedFunction = useMemo(() => () => console.log('Hello World'), []);

const memorizedFunction = useCallback(() => console.log('Hello World'), []);
```

# 언제 memoization을 해야하는가?

개념만 보았을 떄는 굉장히 효율적이고 사용하기만 하면 최적화가 이루어질 것 같은 느낌이 들기도 합니다. 하지만 명확한 목적없이 무작정 메모이제이션을 사용하는 것은 오히려 비효율적입니다.  
리액트에서 메모이제이션이 필요하다고 판단할 수 있는 요인은 아래 두가지 입니다.

1. 새로운 값을 만드는 연산이 복잡하다.
2. 함수 컴포넌트의 이전 호출과, 다음 호출 간 사용하는 값의 동일성을 보장하고 싶다.

1번의 경우에는 만약 10000개의 요소를 가진 배열이 있다고 생각하면 이 배열을 매번 생성하는 것 보다는 메모해서 활용하는 것이 효율적일 것입니다.

2번의 경우 함수 컴포넌트의 호출 간 값들의 동일성을 보장하기 위해서입니다.  
그렇다면 왜 동일성을 보장해야 할까요? 그 이유는 바로 `React.memo`와 연동해서 사용하기 위해서입니다.

앞서 memo의 잘못된 활용 예시에서 props로 전달되는 객체의 동일성이 보장되지 않아 실제 객체의 내용은 똑같아도 shallow compare를 통해서 다른 객체라고 판단되어서 매번 리렌더링이 실행되는 상황을 확인했습니다. 이런 상황에서 전달되는 객체의 동일성을 보장하기 위해서 메모이제이션을 활용할 수 있습니다.

메모이제이션 된 객체는 새롭게 만들어진 것이 아니라 이전의 객체를 그대로 활용하는 것이기에 shallow compare에서 동일함을 보장받을 수 있습니다.

[참고 Repository](https://github.com/jsdmas/React-practice)
