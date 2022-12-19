---
title: "Uncaught (in promise) TypeError: json.forEach is not a function
    at HTMLInputElement.<anonymous>"
execrpt: "TypeError 처리"
categories:
  - err
tags:
  - err
last_modified_at: 2022-12-18
---


![](https://user-images.githubusercontent.com/105098581/208280160-6f63f859-2af4-42dc-b56a-99d18c1bb2b4.png)

node에서 프론트쪽으로 json을 넘겨주던 중 위와 같은 에러가 발생했다.  
**원인**   
프론트에서 배열을 처리할떄 써야하는 forEach를 백엔드에서 넘겨준 데이터 타입과 맞지 않아서 발생.  
TypeError객체는 일반적으로 값이 기대하던 자료형이 아니라서 연산을 할 수 없을 때 발생하는 오류이다.  
**해결**  
데이터를 가공하여 오류를 해결하였다.  

**추가로 살펴보면 좋은글**  
[TypeError_MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/TypeError)
