---
title:  "JS ?? Operator"
excerpt: "?? Operator 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---
[MDN-?? Operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)  
  
or(||)연산자는 변수에 기본값을 줄 때 유용하다.  
or연산자가 거짓으로 판별하는 것은 아래와 같다.
- 빈 문자열 ("")
- 0
- false
- undefined
- null

하지만 빈 문자열이나 0 을 False라고 여기지 않고 싶을 경우 ?? 연산자를 사용한다.  
?? 연산자는 단순히 참, 거짓을 판별하는 게 아니라 null이나 undefined에 대해서 False라고 판별한다.  
?? 연산자는 null이나 undefined일 떄만 작동한다.  

```js
let name = 0;

console.log("hello" + name || "jinho") // hello jinho

console.log("hello" + name ?? "jinho") // hello 0

```


