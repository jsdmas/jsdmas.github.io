---
title: "React styledComponents설명"
execrpt: "styledComponents"
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2022-11-07
---

[styled-components공식홈페이지](https://styled-components.com/)  

## Adapting and Extending
- 적용, 연장 방법


`예시 코드`    

```jsx
import React from 'react';
import styled from 'styled-components';

const Father = styled.div`
  display: flex;
`;

const Box = styled.div`
  background-color: ${props => props.bgColor};
  width: 100px;
  height: 100px;
`;

// Box 상속받기
const Circle = styled(Box)`
  border-radius: 50%;
`;

const App = () => {
  return (
    <Father>
      // props를 보내서 색상을 받는다.
      <Box bgColor="teal" />
      <Circle bgColor="tomato" />
    </Father>
  );
};

export default App;
```

## 'As' and Attrs

`As 사용 예시`  

```jsx
import React from 'react';
import styled from 'styled-components';

const Father = styled.div`
  display: flex;
`;

const Btn = styled.button`
  color: white;
  border: 0;
  background-color: tomato;
  border-radius: 15px;
`;

const App = () => {
  return (
    // HTMLTag가 div에서 header로 바뀐다
    <Father as="header">
      <Btn>Log in</Btn>
      {/* HTMLTag가 button에서 a 로 바뀐다. */}
      <Btn as="a" href="/" />
    </Father>
  );
};

export default App;
```
- as라는 props를 보내면 Btn을 사용할 건데 다른HTMLTag로 사용하고 싶다고 명시할 수 있게 된다.

`Attrs 사용 예시`   

```jsx
import React from 'react';
import styled from 'styled-components';

const Father = styled.div`
  display: flex;
`;

const Input = styled.input.attrs({ required: true, placeholder: "hello!", minLength: 10 })`
  background-color: skyblue;
  color: white;
`;

const App = () => {
  return (
    <Father>
      <Input /><Input /><Input /><Input /><Input /><Input /><Input /><Input />
      <Input /><Input /><Input /><Input />
    </Father>
  );
};

export default App;
```

- 후에 input으로 전달될 모든 속성을 가진 오브젝트를 담을 수 있다.

## Animations and Pseudo Selectors

`적용 예제`  

```jsx
import React from 'react';
import styled, { keyframes } from 'styled-components';

const rotateAnimation = keyframes`
  0%{
    transform: rotate(0deg);
    border-radius: 0px;
  }
  50%{
    border-radius: 100px;
  }
  100%{
    transform: rotate(360deg);
    border-radius: 0px;
  }
`;

const Box = styled.div`
  animation: ${rotateAnimation} 1.5s linear infinite;
  height: 200px;
  width: 200px;
  background-color: skyblue;


  display: flex;
  align-items: center;
  justify-content: center;
    span{
      font-size: 50px;
      transition: 1s ease-in-out;

      // span:hover 
      &:hover{
        font-size: 100px;
      }
    }

`;

const App = () => {
  return (
    <Box>
      <span>🤣🤣</span>
    </Box>
  );
};

export default App;
```

`더 나은 Selector 방법`   
```jsx
import React from 'react';
import styled, { keyframes } from 'styled-components';

const rotateAnimation = keyframes`
  0%{
    transform: rotate(0deg);
    border-radius: 0px;
  }
  50%{
    border-radius: 100px;
  }
  100%{
    transform: rotate(360deg);
    border-radius: 0px;
  }
`;

const Emoji = styled.span`
      font-size: 50px;
      transition: 1s ease-in-out;
`;

const Box = styled.div`
  animation: ${rotateAnimation} 1.5s linear infinite;
  height: 200px;
  width: 200px;
  background-color: skyblue;

  display: flex;
  align-items: center;
  justify-content: center;
    ${Emoji}:hover{
        font-size: 100px;
    }

`;

const App = () => {
  return (
    <Box>
      <Emoji>🤣🤣</Emoji>
    </Box>
  );
};

export default App;
```

## Themes

- 기본적으로 모든 색상들을 가지고 있는 object이다.
- 모든 색상을 하나의 object 안에 넣어놨기 때문에 매우 유용하다.
- Themes를 사용하는 방법은 App이 ThemeProvider 안에 있기 때문에 원한다면 component들이 색에 접근할 수 있다.


`Thems를 사용하기 위한 index.js 세팅`  
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

import App from './App';
import { ThemeProvider } from 'styled-components';

// 다크모드
const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
}

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke",
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  // ThemPrvider는 theme 이라는 props를 받는다.
  <ThemeProvider theme={darkTheme}>
    <App />
  </ThemeProvider>
);
```

`App.js`

```jsx
import React from 'react';
import styled from 'styled-components';

const Box = styled.div`
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: ${props => props.theme.backgroundColor};
`;

const H1 = styled.h1`
  /* theme 으로부터 부여받은 color 사용 */
  color: ${props => props.theme.textColor};
  
`;

const App = () => {
  return (
    <Box>
      <H1>Hello</H1>
    </Box>
  );
};

export default App;
```

