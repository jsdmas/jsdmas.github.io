---
title: "React_simple-spa"
execrpt: "Routes, route설명"
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2022-10-20
---

리엑트 프로젝트 생성 방법(프로젝트 이름은 영어 소문자만 사용 가능)  
> yarn create react-app 프로젝트이름

## Single Page Application (=SPA)
하나의 HTML 페이지로 다수의 페이지 효과를 내는 구현 방식.  
js 파일로 웹 페이지 화면을 변경하는 형태로 구현된다.
### Router
분배하는 기능을 수행하는 소프트웨어나 하드웨어  
대표적인 라우터로는 아이피공유기가 있다.  
React의 Router는 URL에 의해 컴포넌트를 분배하는 기능을 한다.  
HTML5 에서 Javascript에 추가된 기능중 history객체를 통해 URL을 변조하는 기능이 있다.
리액트의 Router는 이 기능을 활용하여 현재 페이지의 URL을 다양하게 변조하여 거기에 각각의 컴포넌트를 분배한다.
### SPA 개발의 장점
페이지 이동 없이 JS에 의해 화면이 갱신되므로 실제로 네트워크 통신이 발생하지 않아 실행 속도가 빠르다.
### SPA 개발의 단점
JS코드가 비대해 질 수 있다. 코드 스플리팅 기법으로 해결가능 (코드 분할 작성)  
하나의 HTML이므로 SEO에 취약하다 (서버사이드 렌더링으로 해결 가능, SEO:검색엔진최적화)  
서버사이드 랜더링 = React + Node / React + PHP / React + Java(Spring)

## 예시
`App.js`
```jsx
import React from "react";
const App = () =>{
    return(
        <div>
        <h1>02-simple-spa</h1>
        <hr />
        // 링크 구성 부분
        <Link to="/">[Home]</Link>
        // HTTP GET 파라미터를 포함하는 링크 구성
        <Link to="/department_get?id=101&msg=hello">[컴퓨터공학과]</Link>
        // PATH 파라미터를 포함하는 링크 구성
        <Link to="/department_path/201/hello">[전자공학과]</Link>

        // 페이지 역할을 할 컴포넌트를 명시하기
        <Routes>
        // 첫 페이지로 사용되는 컴포넌트의 경우 exact={true}를 명시해야 한다.
        // 첫 페이지로 사용되는 컴포넌트는 path에 "/"를 권장
        <Route path="/" element={<Home />} exact={true} />
        // 서브라우팅 사용
        // 참고로 서브라우터에 대한 경로 구성은 "./" 없이 상대경로만 가능하다.(절대경로 불가)
        <Route path="/main/*" element={<Main/>}>
        // GET파라미터 사용
        <Route path="/department_get" element={<DepartmentGet/>} />
        // Path 파라미터는 URL 형식에 변수의 위치와 이름을 정해줘야 한다.
        <Route path="/department_path/:id/:msg" element={<DepartmentGet>} />
        </Routes>
        </div>
    );
};
```