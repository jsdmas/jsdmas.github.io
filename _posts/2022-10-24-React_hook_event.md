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

![](https://user-images.githubusercontent.com/105098581/197465044-fbc58170-6bb7-43ef-a263-d52da281f8df.png)
