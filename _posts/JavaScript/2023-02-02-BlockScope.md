---
title:  "JS Block Scope"
excerpt: "Block Scope 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-02
---

# Block Scope 
scope는 variable들이 접근가능한지 아닌지를 감지해준다.

## const, let 사용 예제
```js
if(true){
    const hello = "hi";
}
console.log(hello); // ReferenceError
```
위의 경우 const 그리고 let도 마찬가지로 모두 block scope로 되어 있다.  
이 말의 뜻은 그 block 안에서만 존재한다는 뜻이다.  
- block은 `{ }`괄호로 만들어져 있다.  
- `hello`는 괄호밖에는 존재하지 않는다.  
- 외부에서만 접근 가능하다.
- 그러나 외부에서는 안에 있는 어떤 것들에도 접근할 수 없다.

## var 사용 예제

```js
if(true){
    const hello = "hi";
}
console.log(hello); // "hi"
```

- var는 block scope 같은 건 없다.
- if나 while, for 구문 안에서 var로 변수를 만들 수 있다.
- var는 function scope를 가진다.
- function scope의 뜻은 var가 function 안에서 접근할 수 있다는 뜻이다.

```js
function a (){
    var hello = "hi";
}
console.log(hello); // ReferenceError
```
- function을 만들고 그 안에 var를 넣으면 그 var는 외부에서는 접근하지 못하게 된다.