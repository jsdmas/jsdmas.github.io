---
title:  "this in ArrowFunctions"
excerpt: "화살표함수 사용시 this 키워드 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-02
---

arrow function을 사용하지 않아야할 때가 있다.

```js
const button = document.querySelector("button");

button.addEventListener("click", function(){
    console.log(this);
    // <button></button>
});
```
위의 일반함수는 this가 button을 참조하고 있다.

```js
const button = document.querySelector("button");

button.addEventListener("click", () =>{
    console.log(this);
    // window
});
```
arrow function 안에 있는 this는 window를 참조한다.  
arrow function은 this를 이벤트로 부터 가지고 있지 않는다.  
arrow function은 this를 window Object로 가지고 있다.  
  
**또다른 예시**   
```js
const myName = {
    name: "jinHo",
    age: 99,
    addYear: ()=>{
        this.age++;
    },
};
console.log(myName); // age = 99
myName.addYear();
myName.addYear();
console.log(myName); // age = 99
```
myName Object 출력시 age는 2가 증가되지않고 99 그대로이다.  
arrow function은 this가 window를 가리키기 때문이다.  
만약 this가 객체를 가리키기 원한다면 아래처럼 일반함수로 고쳐주면 된다.

```js
const myName = {
    name: "jinHo",
    age: 99,
    addYear(){
        this.age++;
    },
};
console.log(myName); // age = 99
myName.addYear();
myName.addYear();
console.log(myName); // age = 101
```
