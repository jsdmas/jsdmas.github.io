---
title:  "localStorage"
excerpt: "loaclstorage를 활용한 로그인창 만들기"
toc : true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2022-08-02
---

## Input Values

한 줄로 document내의 input 검색방법

```js
const loginForm = document.querySelector("#login-form");
// form 끌어오기
const loginInput = loginForm.querySelector("input");
//loginform에서 input 끌어오기

위의 코드를 한줄로 쓰려면 밑의 방법처럼 하면 된다.

const loginInput = documnet.querySelctor("#login-form input");
// login form 이라는 id 내의 input을 찾아준다.
```

## Events

- form을 submit하면 브라우저는 기본적으로 페이지를 새로고침 하도록 되어 있다.
- 브라우저가 기본 동작을 실행하지 못하게 막아주는 코드
- 아무것도 안 하더라도 JS가 어떤 정보를 담은 채로 function을 호출한다.   


```js
const loginForm = document.querySelector("#login-form");

const loginInput = document.querySelector("#login-form input");
function onLoginSubmit(event){
  event.preventDefault();
  //preventDefault는 기본적으로 제공되는 function이다.
  //브라우저의 기본 동작을 막아준다.
  console.log(loginInput.value);
  //콘솔창에 input 에 적은 value 값을 나타내준다.
}
loginForm.addEventListener("submit", onLoginSubmit);

```
코드 예시  

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="KKoQWME" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/KKoQWME">
  JS_values</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


- JS는 함수를 실행시키는 동시에 그 함수에 첫번쨰 인자로 object를 널어준다.  
- 그리고 이 object에는 방금 일어난 event에 대한 여러 정보가 담겨있다.  
- 아래는 브라우저 기본동작을 막는 예시이고, event가 일어날때 console에서 정보를 알수있게 해놧다.   
  
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="bGvLoxp" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/bGvLoxp">
  JS_events2</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

- 가장 중요한건 addEventListener 안에 있는 함수는 직접 실행하지 않는다는 것이다.  
- 브라우저가 실행시켜주며 실행만 시켜주는게 아니라 event에 대한 정보도 담아준다.  
- 우리는 자리만 만들어주면 되고 이 정보 안에는 몇 가지 함수도 같이 들어있다.

##  `백틱을 이용한 내용 작성방법


```js
"Hello" + username ;
hi.hello = 'Hello ${username}';
//위와 아래의 표현법은 같다.
```

## saving username

- user의 이름이나 다른 무언가를 기억하고 싶을떄 기억할 수 있게 하는 기능이 존재하는데 그 API의 이름은 local storage 이다.  
- [localStorage사용](https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage)  
- [localstorage API개념](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)  
- 개발자 도구에서 local srorage의 모습


![]("https://user-images.githubusercontent.com/105098581/182523099-76a90bd6-6634-4f77-b5ef-c2b2e238682b.png")

- setItem을 활용하면 local storage에 정보를 저장할 수 있다.  

```js
localStroage.setItem("cat","tom");
// 값을 저장 ->key, value 순서.
localStorage.getItem("cat");
//실행시 tom 이란 저장값을 불러낸다.
localStorage.removeItem("cat");
//application 이동시 key값이 삭제되면서 vlaue 도 함꼐 삭제된다.
```

- loaclStorage를 활용한 예시

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="ZExrPYr" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/ZExrPYr">
  loacal_storage</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
