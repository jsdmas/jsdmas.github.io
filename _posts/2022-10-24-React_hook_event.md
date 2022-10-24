---
title: "React_hook기본 설명"
execrpt: "hook 예제&설명"
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2022-10-24
---
## 리액트 이벤트 시스템
- 용어정리  
  - 이벤트  
    프로그램이 겪는 일련의 사건.  
    사용자가 프로그램에 대해 행하는 어떠한 행위를 포함한다.
  - 이벤트 리스너  
    이벤트가 발생할 때를 대기하고 있는 구현체.  
    웹에서는 HTML 태그에서 명시하는 이벤트 속성이 이에 해당한다.  
    ```html
    <a onclick="...">
    ```
  - 이벤트 핸들러  
    이벤트가 발생한 순간 그에 반응하도록 구현된 코드나 함수 (또는 클래스)

- 리액트에서 이벤트 구현시 주의점
    1. 이벤트 리스너의 이름은 HTML속성 아닌 JSX에 의한 자바스크립트 프로퍼티이므로 카멜 표기법으로 작성.
       - onclick (X)
       - onClick (O)
    2. 이벤트 리스너에 전달할 이벤트 핸들러는 코드 형태가 아니라 반드시 함수 형태로 전달해야 한다.
    3. DOM요소(=HTML태그)에만 이벤트 리스너가 존재한다.
       - 직접 구현한 컴포넌트에 대해서는 설정 불가.

## Hooks
함수형 컴포넌트에서 상태값(state)를 관리하기 위한 기능으로 클래스형 컴포넌트의 LifeCycle에 대응된다.  
### 상태변수의 이해
시스템의 상태공간이라는 것은 어떤 지정된 시간에서 시스템의 상태를 완전하게 서술하는 데 필요한 임의의 최소 변수의 집합이다. 이와 같이 시스템을 정의할 수 있는 임의의 최소 변수들을 상태변수라고 한다.  
![](https://user-images.githubusercontent.com/105098581/197465044-fbc58170-6bb7-43ef-a263-d52da281f8df.png)  
### useState
- 가장 기본적인 Hook 함수
- 함수형 컴포넌트에서 state값을 생성한다.
- 하나의 useState 함수는 하나의 상태 값만 관리할 수 있다.
- 컴포넌트에서 관리해야 할 상태가 여러 개라면 useState를 여러번 사용하면 된다.

useState() 함수를 import하고 사용하는 경우.
```jsx
import React, {useState} from "react";
// ...
const [상태변수, 변수에대한setter함수] = useState(초기값);
```
useState()함수를 import하지 않고 직접 사용하는 경우.
```jsx
const [상태변수, 변수에대setter함수] = React.useState(초기값);
```
`사용 예시`
```jsx
    const MyState = () => {
    /**
     * state(상태)값 정의
     * - 이 페이지 안에서 유효한 전역변수 같은 개념.
     * - const [변수이름, 변수에대한setter함수] = React.useState(변수의기본값);
     * - state값은 직접 변경할 수 없고 반드시 setter를 통해서만 변경 가능하다.
     * - useState() 함수에 전달하는 값은 state값에 대한 초기값이다.
     */ 
    const [myName, setMyName] = React.useState(``);
    const [myPoint, setMyPoint] = React.useState(50);

    /** 이벤트 핸들러로 사용될 함수는 컴포넌트 함수 안에서 정의된다. */
    const onMyNameChange = e => {
        // e.currentTarget은 jQuery의 ${this}에 해당함.
        // 즉, 이벤트가 발생한 자신 (여기서는 input태그)
        setMyName(e.currentTarget.value);
    };
    return(
    <div>
        <h2>MyState</h2>
        // state값을 출력할 때는 단순히 변수값으로서 사용한다.
        <h3>{myName}님의 점수는 {myPoint}점 입니다.</h3>
        <hr />
        <div>
            <label htmlFor="myNameInput">이름 : </label>
            <input id="myNameInput" type="text" value={myName} onChange={onMyNameChange} />
        </div>
        <div>
            <label htmlFor="myPointInput">점수 : </label>
            <input id="myPointInput" type="text" min="0" max="100" value={myPoint} onChange={e=>{
                // 자기 스스로의 입력값을 myName이라는 state값에 반영함.
                setMyPoint(e.currentTarget.value);
            }} />
        </div>
    </div>
    );
};
```

### useEffect
useEffect는 기본적으로 렌더링 직후마다 실행되며,  
두 번쨰 파라미터 배열에 무엇을 넣는지에 따라 실행되는 조건이 달라진다.  
> 클래스형 컴포넌트의 componentDidMount와 componentDidUpdate를 합친 형태
  
<div markdown="1" class="primary-notice">

`랜더링 될 때마다 실행되는 함수 정의`  
최초 등장하거나 state값이 변경될 때 모두 실행된다.
```jsx
useEffect(()=>{
    // ... 처리할 코드 ...
});
```
`업데이트시에는 생략되는 함수 정의`  
컴포넌트가 마운트 될 때 최초 1회만 실행된다. (state값이 변경될 때는 실행되지 않는다.)
```jsx
useEffect(()=>{
    // ... 처리할 코드 ...
}, []);
```
`특정 state, props값이 변경될 때만 호출되도록 설정하기`
```jsx
useEffect(()=>{
    // ...처리할 코드 ...
}, [값이름]);
```
`컴포넌트가 언마운트(화면에서 사라짐)될 때만 실행하도록 설정하기`  
클로저(리턴되는 함수)를 명시한다.
```jsx
useEffect(()=>{
    return function(){
        // ... 처리할 코드 ...
    };
}, [state값이름]);
```
```jsx
useEffect(()=>{
    return()=>{
        // ... 처리할 코드 ...
    };
},[state값이름]);
```
</div>

