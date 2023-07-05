---
title: 'React 의존성 배열, useEffect hook'
execrpt: '의존성 배열, useEffect 설명'
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2023-07-05
---

# 의존성 배열

의존성 배열이란 무엇일까요? 얕게 이해하자면 아래와 같습니다.

- useEffect에 두번째 인자로 넘기는 배열입니다.
- 두번째 인자를 넘기지 않으면 Effect는 매번 실행되고, 빈 배열을 넘긴다면 컴포넌트의 첫번째 렌더링 이후에만 실행됩니다.

useEffect의 시그니쳐는 아래와 같습니다.

```
useEffect(effect, 의존성)
```

여기서 effect는 함수의 형태로 표현되고, 의존성은 여러 의존성들을 한번에 전달하기 위해서 배열의 형태로 표현됩니다.

의존성이란 말을 해석해보면

- A라는 요소가 온전히 동작하기 위해서 B,C,D 등 다른 요소들을 필요로 할 때 A는 B,C,D에 의존하고 있다고 표현합니다.
- 그리고 `B,C,D는 A의 의존성이다`라고도 표현할 수 있습니다.

그렇다면 useEffect에서 의존성 배열이란 `무언가가 의존하고 있는 요소들의 모음`이라고 할 수 있습니다. 그리고 여기서 말하는 무언가는 바로 effect 함수입니다. 즉, useEffect의 의존성 배열은 `effect 함수가 의존하고 있는 요소들의 모음`이라고 할 수 있습니다.

단순히 설명하자면 그냥 effect 함수가 사용하고 있는 외부의 값들이 의존성입니다.

```jsx
function Component(){
	const [count, setCount] = useState(0);

	const effect = () => {
		document.title = `you clikced ${count} times`
	};

	useEffect(effect, [count]];
}
```

위의 예시에서 effect 함수는 `count` 라는 외부의 값을 내부에서 사용하고 있습니다. 따라서 effect 함수의 의존성은 `count` 이며 count를 의존성 배열에 넣어줘야하는 것입니다.

이렇게 되면 useEffect는 리렌더링이 된 후 의존성 배열을 검사해서 의존성 배열에 있는 값들이 변경되었을 경우에 다시 새로운 의존성을 가지고 effect를 실행시켜 줍니다.

# useEffect 의존성 배열의 잘못된 활용

이처럼 useEffect에서 의존성 배열은 핵심이 되는 부분입니다. 그런데 이런 의존성 배열을 잘 못 설정하고 활용하는 경우가 있습니다.  
대부분 필요 없는 요소들을 굳이 의존성 배열에 넣는 실수는 잘 하지 않습니다.  
하지만, 필요한 의존성을 제대로 의존성 배열에 넣어주지 않는 실수를 많이 합니다.

```jsx
function Component(){
	const [count, setCount] = useState(0);

	useEffect(()=>{
		document.title = `you clikced ${count} times`
	}, []];
}
```

이 코드는 count가 변경되었을 때 document.title이 맞춰서 변경되지 않습니다. 왜냐하면 의존성 배열에 의존성을 제대로 넣어주지 않았기 때문입니다.

# useEffect 의존성 배열을 잘 설정하는 법

useEffect에서 버그가 발생하지 않게 의존성 배열을 잘 설정하는 방법은 아래의 원칙만 지켜주면 됩니다.

- **“모든 의존성을 빼먹지 말고 의존성 배열에 명시해라”**

여기에 덧붙여서 아래의 원칙을 추가해주면 좋습니다.

- **“가능하다면 의존성을 적게 만들어라”**

```jsx
// bad
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalID = setInterval(() => {
      setCount(count + 1);
    }, 1000);

    return () => clearInterval(intervalID);
  }, [count, setCount]);

  return (
    <div>
      <h1>count:{count}</h1>
    </div>
  );
}
```

```jsx
// good
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalID = setInterval(() => {
      setCount((prevCount) => prevCount + 1);
    }, 1000);

    return () => clearInterval(intervalID);
  }, [setCount]); // count를 쓰지않아 의존성 배열을 줄일수있다.

  return (
    <div>
      <h1>count:{count}</h1>
    </div>
  );
}
```

## useEffect 무한루프

간단한 일반 변수, state, props의 경우에는 의존성을 빼먹지 않고 의존성 배열에 명시하기가 쉽습니다.

하지만, 함수 컴포넌트의 내부에서 선언한 Object, Function의 경우에는 함수 컴포넌트의 매 호출마다 새로운 객체, 함수가 선언되고 참조형 데이터 타입의 특징으로 인해 객체 내부의 요소들이 동일하더라도 새롭게 생성된 객체와 이전 객체를 비교하면 서로 다른 객체라고 판단되게 됩니다.

그래서 아래의 코드는 무한 루프를 반복하게 됩니다.

```jsx
function Component(){
	const [count, setCount] = useState(0);

	const increaseCount = () => {
		setCount(prev => prev + 1);
	}

	useEffect(increaseCount, [increaseCount]];
}
```

위의 문제를 해결하기 위해서 여러가지 방안을 시도해볼 수 있습니다.

### 의존성을 제거하기 => 함수를 effect 안에 선언하기

```jsx
function Component(){
	const [count, setCount] = useState(0);

	useEffect(() => {
		const increaseCount = () => {
			setCount(prev => prev + 1);
		};

		increaseCount();
	}, []];
}
```

### 함수를 컴포넌트 바깥으로 이동시키기

```jsx
// bad
function Component() {
	const getUserAuth = () => {
		localStorage.getItem("ACCESS_TOKEN");
	};

	useEffect(() => {
		const token = getUserAuth();
		// login....
	}, []];
};
```

```jsx
// good
function Component() {

	useEffect(() => {
		const token = getUserAuth();
		// login....
	}, [getUserAuth]];

};

// 함수가 바깥에 생성되면 항상 같은 참조값을 가지고 있기 떄문에 무한루프는 발생하지 않는다.
const getUserAuth = () => {
	localStorage.getItem("ACCESS_TOKEN");
};
```

### 메모이제이션

비용이 많이 들어가므로 최후의 방법으로 생각합니다.

```jsx
function Component(){
	const [count, setCount] = useState(0);

	const increaseCount = () => {
		setCount(prev => prev + 1);
	}

	useEffect(() => {
		// do something 1
		increaseCount();
	}, []];

	useEffect(() => {
		// do something 2
		increaseCount();
	}, []];
}
```

```jsx
function Component(){
	const [count, setCount] = useState(0);

	const increaseCount = useCallback(() => {
		setCount(prev => prev + 1);
	}, []);

	useEffect(() => {
		// do something 1
		increaseCount();
	}, [increaseCount]];

	useEffect(() => {
		// do something 2
		increaseCount();
	}, [increaseCount]];
}
```

# Ref

Ref : [https://overreacted.io/a-complete-guide-to-useeffect/](https://overreacted.io/a-complete-guide-to-useeffect/)
