---
title: "React_hook기본 설명"
execrpt: "hook, useEffect ,useRef, useReducer, useMemo, useCallback 예시&설명"
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2022-11-04
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
**Hook 사용시 주의사항**  
1. 반복문, 조건문, 중첩된 함수 내에서 Hook을 실행할 수 없다.
2. React Component 내에서만 호출할 수 있다.

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

    /**
     * 상태값이 변경될때마다 컴포넌트 함수는 매번 재실행된다
     * 그러므로 컴포넌트 영역은 상태값의 변경에 따라 반복적으로 다시 랜더링 된다.
     * --> 결국 아래의 출력문은 상태값이 변경될 때마다 반복 출력된다.
     * 상태값은 화면에 출력될 변수에만 사용해야 한다. 
     */
    console.log(new Date());

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
- useEffect는 두 개의 argument를 가지는 function이다.
- 첫 번째 argument는 사용자가 딱 한번만 실행하고 싶은 코드가 될 것이다.
- 두 번쨰 argument는 사용자가 원하는 값(state,props...)이 변경될 때만 호출하도록 설정 가능하다.
- 두 번쨰 argument에는 값을 여러개 넣는것이 가능하다.
> 클래스형 컴포넌트의 componentDidMount와 componentDidUpdate를 합친 형태
  
<div markdown="1" class="notice--primary">

`랜더링 될 때마다 실행되는 함수 정의`  
최초 등장하거나 state값이 변경될 때 모두 실행된다.
```jsx
useEffect(()=>{
    // ... 처리할 코드 ...
});
```
`업데이트시에는 생략되는 함수 정의`  
컴포넌트가 마운트 될 때 최초 1회만 실행된다. (state값이 변경될 때는 실행되지 않는다.)  
빈 배열은 감시할 것이 없기때문에 React내에서 한번만 실행하게 되는 것.
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

### useRef
함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 처리해 준다.
Vanilla Script에서 `document.getElementById(...)`나 `document.querySelector(...)`로 DOM 객체를 취득하는 과정을 React 스타일로 포현한 것으로 이해할 수 있다.  
예시  
`components/MyBox`  
부모로부터 전달받은 ref 참조변수를 받기 위해 "React.forwardRef" hook에 대한 콜백으로 컴포넌트를 구현한다.  
이렇게 구현된 컴포넌트는 props와 부모로부터 전달받은 ref 참조변수를 파라미터로 주입받는다.
```jsx
const MyBox = React.forwardRef((props, ref)=>{
    const containerStyle = {
        border: `1px solid black`,
        height: `100px`,
        width: `100px`,
    };
    // 부모로부터 전달받은 ref 참조변수를 <div>에 연결한다.
    // 이 참조변수를 통해 부모 컴포넌트가 ref 참조변수가 연결된 <div>의 DOM에 접근할 수 있다.
    return(
        <div style={containerStyle} ref={ref}>
        </div>
    );
});
```
`MyRef.js`
```jsx
import MyBox from 'MyBox';

/**
 * React에서 document.getElementById(...)에 해당하는 기능을 사용하는 방법
 */ 
const MyRef = () => {
    // HTML 태그를 react 안에서 참조할 수 있는 변수를 생성
    const myDname = React.useRef();
    const myLoc = React.useRef();
    const myResult = React.useRef();

    // 컴포넌트에 설정하기 위한 ref
    const myBoxRef = React.useRef();

    // 화면에 출력되지 않은 상태변수를 생성할 수 있다.
    // useRef()함수에 전달하는 파라미터가 상태변수의 기본값이 된다.
    const myValue = React.useRef(0);

    // 이 컴포넌트가 다시 랜더링되었음을 확인하기 위한 시간 출력
    console.log(new Date());

    return(
        <div>
            <h2>MyRef</h2>
            <h3>ref 기본 사용 방법</h3>
            // 미리 준비한 컴포넌트 참조변수와 HTML 태그를 연결
            <div>
                <label htmlFor="dname">학과명 : </label>
                <input type="text" ref={myDname} id="dname" />
            </div>
            <div>
                <label htmlFor="loc">학과위치</label>
                <input type="text" ref={myLoc} id="loc" />
            </div>
            <p>
                입력값 확인: <span ref={myResult}><span>
            </p>
            
            <button onClick={e=>{
                // 컴포넌트 참조변수를 사용하여 다른 HTML 태그에 접근 가능
                // --> "참조변수.current" 해당 HTML을 의미하는 Javascript DOM 객체
                // --> myDname.current와 document.querySelector(...), document.getElementById(...)등으로 생성한 객체가 동일한 DOM객체이다.
                const dname = myDname.current.value;
                const loc = myLoc.current.value;
                myResult.current.innerHTML = `${dname}, ${loc}`;
            }}>클릭</button>

            <button onClick={e => {
                // 이 변수는 갱신되더라도 컴포넌트 함수를 다시 실행시키지 않는다.
                myValue.current++;
                console.log(`myValue=${myValue.current}`);
            }}>Ref 상태변수 갱신</button>

            <hr />
            <h3>컴포넌트에 ref 적용하기</h3>
            {/* ref 참조변수를 컴포넌트에 전달한다. */}
            <MyBox ref={myBoxRef} />

            <button type='button' onClick={() => {
                // <MyBox>를 통해 myBoxRef를 주입받은 DOM에 접근하여 제어함.
                myBoxRef.current.style.backgroundColor = "#f00";
            }}>Red</button>

            <button type='button' onClick={() => {
                // <MyBox>를 통해 myBoxRef를 주입받은 DOM에 접근하여 제어함.
                myBoxRef.current.style.backgroundColor = "#00f";
            }}>Blue</button>
        </div>
    )
}
```

### useReducer
useState 보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트 하고자 하는 경우 사용.  
useState의 대체 함수로 이해할 수 있다.  
state값이 다수의 하위값을 포함하거나 이를 활용하는 복잡한 로직을 만드는 경우에 useState보다 useReducer를 선호한다.  
`예시`
```jsx
import React from 'react';

/**
 * useReduce에 의해 호출될 사용자 정의 함수.
 * --> action값이 OO일 떄 state값을 ~~해라.
 * --> action값의 DataType은 개발자가 결정할 수 있다. (int, string, boolean, json ...)
 * --> state값의 DataType역시 개발자가 결정할 수 있다. (int, string, boolean, json ...)
 * @param {int} state - 상태값 (useState의 state값과 동일)
 * @param {string} action - 어떤 동작인지에 대한 구분
 */

function setCounterValue(state, action) {
    console.log(`${action}, ${state}`);
    // action값의 상태에 따른 state값의 가공 처리를 분기
    switch (action) {
        case `HELLO`:
            return state + 1;
        case "WORLD":
            return state - 1;
        default:
            return 0;
    }
}

const MyReducer = () => {

    /**
     * 상태값(myCounter)와 상태값 갱신함수 (setMyCounter)를 정의한다.
     * -> setCounterValue: setMyCounter()가 호출됨에 따라 간접적으로 호출될 함수
     * -> 0: myCounter에 저장될 초기값
     * 
     * setMyCounter()함수에게 action값을 전달하면
     * React 내부적으로 setCounterValue()함수가 호출되며,
     * 상태값으로 지정된 myCounter와 "HELLO"|"WORLD"가 파라미터로 전달된다.
     */

    const [myCounter, setMyCounter] = React.useReducer(setCounterValue, 0);
    return (
        <div>
            <h2>MyReducer</h2>
            <p>현재 카운트 값: {myCounter}</p>
            <button type='button' onClick={e => setMyCounter(`HELLO`)}>UP</button>
            <button type='button' onClick={e => setMyCounter(`WORLD`)}>DOWN</button>
            <button type='button' onClick={e => setMyCounter(``)}>RESET</button>
        </div>
    );
};

export default MyReducer;
```

### useMemo
함수형 컴포넌트 내에서의 연산 최적화.  
숫자, 문자열, 객체처럼 일반 값을 재사용하고자 할 경우 사용한다.  
> memoized 된 값을 반환한다. : 컴퓨터 프로그램에 동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술

```jsx
import dayjs from 'dayjs';

const MyMemo = () =>{

    // 파라미터로 전달되는 단어의 길이를 반환하는 함수 --> 처리비용이 매우 큰 함수를 가정함.
    const getLength = w => {
        return w.length;
    }

    // 처리할 단어들
    const words = ['City', 'Eye', 'Apple', 'Apple', 'Orange'];

    // 버튼이 눌러진 횟수
    const [myCount, setMyCount] = React.useState(0);
    // 배열의 탐색 위치
    const [myIndex, setMyIndex] = React.useState(0);
    // 출력할 글자
    const [myWord, setMyWord] = React.useState(words[myIndex]);

    /** A(myWord)라는 상태값이 변경된 경우 B(myLen)라는 상태값도 갱신하는 처리 */
    /**
     * 두 번째 파라미터인 배열에 설정된 state값이 이전 상태와 다를 경우에만 콜백을 실행한다.
     * 콜백의 결과가 저장되는 myLen은 일반 상태값과 동일하게 사용할 수 있다.
     * 즉, myWord가 변경될 때만 콜백이 리턴하는 값을 활용하여 myLen을 갱신한다.
     */

    const myLen = React.useMemo(()=>{
        return getLength(myWord);
    }, [myWord]);

    return(
        <div>
            <h2>MyMemo</h2>
            <p>
                {myIndex}번째 단어 {myWord}의 길이 : {myLen}
            </p>
            <button
                onClick={()=>{
                    const next = (myIndex + 1) % words.length;
                    setMyIndex(next);
                    setMyCount(myCount+1);
                    setMyWord(words[next]);
                }}>버튼 클릭</button>
        </div>
    );

};
```

### useCallback
렌더링 성능 최적화에 사용됨.  
이벤트 핸들러 함수를 필요한 경우에만 생성할 수 있다.
> memoized 된 콜백을 반환한다.

```jsx
const MyCallback = () =>{
    const [myText, setMyText] = React.useState('Hello React');

    /**
     * 컴포넌트가 최초 렌더링될 떄 1회만 이벤트 핸들러 함수를 정의하고 이후 부터는 계속적으로 재사용된다.
     * 만약 두 번쨰 파라미터인 배열에 특정 state값을 지정할 경우 해당 값이 수정될 떄만 이벤트가 정의된다.
     * --> 이벤트 핸들러의 중복 정의를 방지해서 성능 향상을 꾀함.
     */
    const onInputChange = React.useCallback((e) => {
        setMyText(e.currentTarget.value);
    }, []);

    return (
        <div>
            <h2>MyCallback</h2>
            <h3>{myText}</h3>
            <input type="text" placeholder='input...' onChange={onInputChange} />
        </div>
    );
}
```
