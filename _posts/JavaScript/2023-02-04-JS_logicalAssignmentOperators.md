---
title: "JS logical assignment operators"
execrpt: "js logical assignment operators 예제 & 설명"
toc: true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---

# Logical OR Assignment
[MDN-Logical OR Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR_assignment)  
logical or operator는, 변수가 falsy일 떄 변수에 value를 넣을 수 있게 한다.  
falsy라는건 아래의 상태를 말한다.  
- 비어있는 텍스트
- undefined
- null
- false
- 0
- NaN
- -0
- 등등

```js
let name = prompt("what is your name?");
if(!name){
    name = "default";
}
console.log(`Hello ${name}`);
```
위의 코드는 아래처럼 쓸 수 있다.
```js
let name = prompt("what is your name?");
name ||= "default";
console.log(`Hello ${name}`);
```

# Logical AND Assignment
[MDN-Logical AND Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND_assignment)  
Logical AND Assignment는 Logical OR Assignment와 반대이다.  
만약 변수가 truthy라면 (변수가 정의되어있고 value가 있다면) 변수를 수정할 거라는 뜻이다.  

```js
const user = {
    name: "hello",
    password : 1234
}
user.password &&= "[secret]";
user.email ||= "default@mail.com";

console.log(user);
// {name: 'hello', password: '[secret]', email: 'default@mail.com'}
```

# Logical NULLISH Assignment
[MDN-Logical NULLISH Assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_assignment)  
Logical NULLISH Assignment는 오직 null이거나 undefined인 경우만 확인한다.  

```js
const user = {
    name: "hello",
    password : 1234,
    admin: null
}
user.admin ??= true
console.log(user);
// {name: 'hello', password: 1234, admin: true}
```

