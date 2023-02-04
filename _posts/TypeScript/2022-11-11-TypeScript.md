---
title: "TypeScript 기초 설명"
execrpt: "TypeScript 설명"
toc: true
toc_sticky: true
categories:
  - TypeScript
tags:
  - TypeScript
  - React
last_modified_at: 2022-11-11
---

## Introduction
- TypeScript는 JavaScript를 기반으로 한 프로그래밍 언어이다. (다른 언어이다.)
- strongly- typed 언어이다 : 프로그래밍 언어가 작동하기 전에 type을 확인한다.
- 코드에 실수가 있어도, 프로그램이 작동하기 전에 TypeScript가 알려준다.
- [TypeScript](https://www.typescriptlang.org/)
- [What is TypeScript?](https://www.samsungsds.com/kr/insights/typescript.html)

## DefinitelyTyped
- [install TypeScript](https://www.typescriptlang.org/download)
- [DefinitelyTyped Github](https://github.com/DefinitelyTyped/DefinitelyTyped)

TypeScript와 reacte를 동시에 설치방법에는 예를들어 아래와 같다.
```
npx create-reacte-app 프로젝트이름 --template typescript

# or

yarn create reacte-app 프로젝트이름 --template typescript
```

이미 생성된 프로젝트에 타입스크립트를 설치할 시에는 아래처럼 하면 된다.

```
npm install --save typescript @types/node @types/react @types/react-dom @types/jest

#or

yarn add typescript @types/node @types/react @types/react-dom @types/jest
```
그후 `npx tsc --init` 명령어로 `tsconfig.json` 파일 생성 후 `"jsx" : "react-jsx"`를 추가하면 된다.  
만약 root 에러가 난다면 index.tsx로 가서  root div 에 `as HTMLElement` 를 붙여주면 된다.  
> const root = ReactDOM.createRoot(document.getElementById("root") as HTMLElement);

[관련 링크](https://github.com/facebook/react/blob/main/CHANGELOG.md#react-dom-client)
  
typescript에서는 `.js` 라는 확장자는 쓰지 않고 `.ts`라는 확장자를 사용한다.  
그리고 TypeScript와 React에서는 `.tsx`를 사용한다.  

만약 설치한 Package에 오류가 뜬다면 `@types/패키지이름(node)` 형식으로 설치하면 대부분은 해결된다.

## Typing the Props

Prop Types 는 브라우저 콘솔에 prop이 거기 있는지 없는지를 확인해 준다. **(코드 실행 후)**  
TypeScript는 코드를 실행 **전** 에 오류를 확인하게 해준다.  
prop들을 TypeScript로 보호하는 방법  

```tsx
// App.tsx

import React from 'react';
import styled from 'styled-components';
import Circle from './Circle';

const App = () => {
  return (
    <div>
      <Circle bgColor="teal" />
    </div>
  );
};

export default App;

// Circle.tsx

import React from "react";
import styled from "styled-components";

const Container = styled.div`

`;

const Circle = ({bgColor}) => {
    return <Container /> 
}

export default Circle;
```
위의 코드를 실행시 bgColor가 뭔지 모르겠다고 오류가 뜰 것이다.  
그러면 **interface** 란 걸 해주면 된다.  
**interface**란 object shape(객체모양)을 TypeScript에게 설명해주는 TypeScript의 개념이다.  
**예시**

```js
const x = (a: number, b: number) => a+b;
// TypeScript에게 a,b의 타입에 대해 설명해주고 있다.
```
위처럼 하는 대신 object shape를 TypeScript에게 설명해 주고 싶다면 아래처럼 하면 된다.  
이렇게 하는걸 interface라고 부른다.  

```tsx
import React from "react";
import styled from "styled-components";

const Container = styled.div`

`;

interface CircleProps {
    // TypeScript에게 object shape를 설명해준다.
    bgColor: string;
}

const Circle = ({ bgColor }: CircleProps) => {
    return <Container bgColor={bgColor} />
}

export default Circle;
```
- bgColor의 타입은 CircleProps의 object이다 라고 말해준다.
- 이제 TypeScript는 CircleProps 안에 bgColor 가 있다는 걸 안다.

하지만 위에서 Typescript가 봤을 때는 Container가 div이다. 그래서 이 component는 어떤 props 도 받고 있지 않고 있다.
```jsx
const Circle = ({ bgColor }: CircleProps) => {
    return <Container bgColor={bgColor} />
}
```
위와 같을때 TypeScript에게 bgColor를 styled-components에게도 보내고 싶다고 할 수 있다.

```tsx
import React from "react";
import styled from "styled-components";

interface ContainerProps {
    bgColor: string;
}

const Container = styled.div<ContainerProps>``;

interface CircleProps {
    // TypeScript에게 object shape를 설명해준다.
    bgColor: string;
}

const Circle = ({ bgColor }: CircleProps) => {
    return <Container bgColor={bgColor} />
}

export default Circle;
```
위처럼 설정하면 오류가 더이상 뜨지않는다.

`최종`    

```tsx
// Circle.tsx
import React from "react";
import styled from "styled-components";

interface ContainerProps {
    bgColor: string;
}

const Container = styled.div<ContainerProps>`
    width: 200px;
    height: 200px;
    background-color: ${props => props.bgColor};
`;

interface CircleProps {
    // TypeScript에게 object shape를 설명해준다.
    bgColor: string;
}

const Circle = ({ bgColor }: CircleProps) => {
    return <Container bgColor={bgColor} />
}

export default Circle;
```
- interface는 위처럼 object를 설명해 주는 것이다.

## Optional Props

위의 코드들은 값을 반드시(Required) 넣어줘야 했다. 하지만 필수가아닌 선택적(optional)이도록 만들고 싶다면 아래처럼 사용하면 된다.

```tsx
// ~~~App.tsx~~~
import React from 'react';
import Circle from './Circle';

const App = () => {
  return (
    <div>
      <Circle borderColor="black" bgColor="teal" />
      <Circle bgColor="tomato" text='hello world' />
    </div>
  );
};

export default App;

// ~~~Circle.tsx~~~
import React from "react";
import styled from "styled-components";

interface ContainerProps {
    bgColor: string;
    borderColor: string;
}

const Container = styled.div<ContainerProps>`
    width: 200px;
    height: 200px;
    background-color: ${props => props.bgColor};
    border-radius: 100px;
    // styled-componet 에선 borderColor가 required 상태이기 때문에 
    // 아래와 같이 색상을 반드시 지정해 줘야 한다.
    border: 5px solid ${props => props.borderColor};
`;

interface CircleProps {
    // TypeScript에게 object shape를 설명해준다.
    bgColor: string;
    // borderColor가 없을수도(optional) 있다고 말해준다.
    // 다른방법 -> borderColor : string | undefined; -> 아래 코드와 같은 의미이다.
    borderColor?: string;
    text?: string;
}

const Circle = ({ bgColor, borderColor, text = "Default Text" }: CircleProps) => {
    return <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>{text}</Container>
    // borderColor={borderColor ?? bgColor} -> string | undefined
    // 만약 borderColor가 있다면 borderColor를 사용하고 만약 없다면 bgColor를 사용하라.
}

export default Circle;
```

## State

React 에서 useState hook을 사용하면 TypeScript는 초기값을 추론해서 데이터타입을 정해준다.  
만약 state값이 string이였다가 number로 바뀌거나 값을 2가지를 가져야 하는 상황이 온다면 아래처럼 사용해 주면된다.  
```tsx
const [value, setValue] = useState<number| string>(0);
setValue(2);
setValue("hello");
setValue(true);//  -->  오류발생
```

## Forms
- any타입은 어떤 것이든 될 수 있다는 것이다.
- any타입을 배제하고자 노력해야한다.
- 아래는 event들에 타입을 추가하는 예시이다.

```tsx
import React, { useState } from 'react';

const App = () => {

  const [value, setValue] = useState("");
  // React.FormEvent<HTMLInputElement>
  // React 에서 FormEvent 발생시 어떤종류의 Element가 이 onChange 이벤트를 발생시킬지를 특정할 수 있다.
  // 타입스크립트는 onChange 함수가 InputElement에 의해서 실행 될 것을 알게된다.
  const onChange = (event: React.FormEvent<HTMLInputElement>) => {
    const {
      // 이벤트 안의 currnetTarget.value 값을 얻어온다.
      currentTarget: { value },
    } = event;
    setValue(value);
  }
  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log(`hello ${value}`);
  }

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input type='text' placeholder='username...' value={value} onChange={onChange} />
        <button type='submit'>Log In</button>
      </form>
    </div>
  );
};

export default App;
```

## Themes

1. declaration(선언)파일 만들기 (d.ts -> declaration.ts)
    - 이 파일은 이전에 설치해 놓은 이 파일을 override(덮어쓰기) 한다.


**styled.d.ts**  

```ts
import "styled-components";

// and extend them!
// styled-components의 테마 정의를 확장한다.
declare module 'styled-components' {
    export interface DefaultTheme {
        textColor: string;
        bgColor: string;
    }
}
```

2. theme 만들기
    - 테마는 위의 정의 파일 속 속성들과 같아야한다.

**theme.ts**   

```ts
import { DefaultTheme } from "styled-components";

export const lightTheme: DefaultTheme = {
    bgColor: "white",
    textColor: "black",
};

export const darkTheme: DefaultTheme = {
    bgColor: "black",
    textColor: "white",
};
```

3. index.tsx에 ThemeProvider import

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

import { ThemeProvider } from 'styled-components';
import { lightTheme } from "./theme";
import { darkTheme } from './theme';

const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(
  <ThemeProvider theme={darkTheme}>
    <App />
  </ThemeProvider>
);
```

4. App.tsx 에서 Theme 사용

```tsx
import React from 'react';
import styled from 'styled-components';

const Container = styled.div`
  background-color: ${props => props.theme.bgColor};
  color: ${props => props.theme.textColor};
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
`;


const App = () => {
  return (
    <Container>
      <h1>Hello World!</h1>
    </Container>
  );
};

export default App;
```

