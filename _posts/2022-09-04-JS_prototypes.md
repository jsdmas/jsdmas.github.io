---
title: "prototypes, Class"
execrpt: "js prototypes, Class 설명"
toc: true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2022-09-04
---

## 생성자

<div class="notice--primary" makrdown="1">

`맴버 변수를 갖는 생성자를 통해서 객체 만들기`

```js
function User(){
    //맴버변수 정의 (일반적으로 맴버변수는 일반 변수와 구분하기위해 (_)로 시작하는 이름을 갖는다.)
    this._id = null; 
    this._email = null;
};
// 생성자를 통한 객체 만들기
const foo = new User();
foo._id = "hello";  // foo객체 this._id 에 "hello" 저장.
foo._email = "hi@naver.com";    //위와동일
```

`파라미터를 맴버변수에 복사하는 생성자`

```js
const User2 = function(id, email){
    this._id = id;
    this._email = email;
};

const foo = new User2("hello", "hi@naver.com");
console.log(foo); // User2 { _id: 'hello', _email: 'hi@naver.com' }
```

</div>

## 매서드

<div class="notice--primary" makrdown="1">

`prototype을 활용한 매서드 정의`

```js
const User3 = function(id, email){
    this._id = id;
    this._email = email;
};
//로그인 수행 매서드
User3.prototype.login = function(){
    // 객체안에 속한 메서드 안에서는 생성자가 정의한 맴버변수를 마음껏 활용할 수 있다.
    console.log(`로그인 되었습니다 -> id:${this._id}, email:${this._email}`);
}
//로그아웃을 수행하는 매서드
User3.prototype.logout = function(){
    // 객체안에 속한 메서드 안에서는 생성자가 정의한 맴버변수를 마음껏 활용할 수 있다.
    console.log(`로그아웃 되었습니다 -> id:${this._id}, email:${this._email}`);
}
const student = new User3(`학생`, `hi@naver.com`);
student.login();    //객체 안에 내장된 메서드 호출
student.logout();
```
</div>

## getter, setter
<div class="notice--primary" makrdown="1">


`get, set활용`


```js
function User4(){
    this._id = null;
    this._email = null;
}
Object.defineProperty(User4.prototype, `id`, {
    get: function(){
        console.log(`id에 대한 getter 호출됨`);
        return this._id;//맴버변수의 값을 반환하는 기능
    },
    set: function(param){
        console.log(`id에 대한 setter 호출됨`);
        this._id = param;//파라미터 값을 맴버변수에 복사하는 기능
    }
});

Object.defineProperty(User4.prototype, `email`, {
    get: function(){
        console.log(`email에 대한 getter 호출됨`);
        return this._email;//맴버변수의 값을 반환하는 기능
    },
    set: function(param){
        console.log(`email에 대한 setter 호출됨`);
        this._email = param;//파라미터 값을 맴버변수에 복사하는 기능
    }
});

const friend = new User4();
friend.id = `친구`;
friend.email = `hi@naver.com`;
console.log(friend.id);
console.log(friend.email);
// id에 대한 setter 호출됨
// email에 대한 setter 호출됨
// id에 대한 getter 호출됨
// 친구
// email에 대한 getter 호출됨
// hi@naver.com
```

</div>

## json 활용

<div class="notice--primary" makrdown="1">

`prototype을 활용한 매서드 정의`

```js
function Member(username, password){
    this._username = username;
    this._password = password;
}
Member.prototype = {
    get username(){
        return this._username;
    },
    set username(param){
        this._username = param;
    },
    get password(){
        return this._password;
    },
    set password(param){
        this._password = param;
    },
    login: function(){
        console.log(`로그인! username=${this.username}, password=${this.password}`);
        //this._username 를 쓰지않고 this.username을 쓰는 이유는 기존 변수를 지키기 위해서이기도 하다.
        //변수값은 get을 통해 전해져서 더 안전하다.
        //Get,set만드는이유는 맴버변수에 접근을막기위해사용한다.(오류발생여부 차단을위해 사용)
    },
    logout: function(){
        this.username = "";
        this.password = "";
        console.log(`로그아웃! username=${this.username}, password=${this.password}`);
        //값이 공백으로 나온다.
    }

};
console.log(Member.prototype);
// {
//   username: [Getter/Setter],
//   password: [Getter/Setter],
//   login: [Function: login],
//   logout: [Function: logout]
// }
const member1 = new Member(`hihi`, `1234`);
console.log(member1.username);// hihi
console.log(member1.password);// 1234
member1.login();// 로그인! username=hihi, password=1234
member1.logout();// 로그아웃! username=, password=
member1.username = `world`; //setter를 통한 맴버변수 변경
```

</div>

## Class

