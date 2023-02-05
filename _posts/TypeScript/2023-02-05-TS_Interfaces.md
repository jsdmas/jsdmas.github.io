---
title: "TS Interfaces"
execrpt: "TypeScript Interfaces 예제"
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-02-05
---

# 타입 alias 예시

```ts
type Team = "red" | "blue" | "yellow"
type Health = 1 | 5 | 10
type Player = {
    nickName : string,
    team: Team,
    health : Health
}
const jinho :Player = {
    nickName : "jinho",
    team : "yellow",
    health : 5
}
```

# Interfaces
타입스크립트에게 오브젝트의 모양을 알려주는 방법엔 두가지가 있다.  
하나는 type 을 쓰고, 오브젝트의 모양을 써주는 방법이 있고 다른 하나는 interface이다.

```ts
type Player = {
    nickName : string,
    team: Team,
    health : Health
}

interface Person {
    nickName : string,
    team: Team,
    health : Health
}
```

인터페이스는 class 처럼 다룰 수 있고 객체 지향 프로그래밍의 개념을 활용해서 디자인되었다.
```ts
interface User {
    name: string
}
interface Player extends User{

}
const hello : Player ={
    name: "jinho"
}
```

위를 type으로 표현하면 아래와 같다.
```ts
type User {
    name: string
}
type Player = User & {

}
const hello : Player ={
    name: "jinho"
}
```

인터페이스의 또 다른 특징으로는 property 들을 축적시킬 수 있다.

```ts
interface User {
    name:string
}
interface User {
    lastName: string
}
interface User {
    health:number
}
const jinho : User = {
    name: "jinho",
    lastName: "haha",
    health: 12
}
```

# class interfaces

기존의 추상화 class와 다르게 interface는 javascript로 컴파일 되지않아 더 가볍다.  

## 기존 abstarct를 활용한 class

```ts
abstract class User {
    constructor(
        protected firstName : string,
        protected lastName : string
    ){}
    abstract sayHi(name:string) : string
    abstract fullName():string
}

class Player extends User {
    fullName(){
        return `${this.firstName} ${this.lastName}`;
    }
    sayHi(name:string){
        return `Hello ${name}. name is ${this.fullName()}`;
    }
}
```

## interface를 활용한 class

```ts
interface User {
    firstName : string,
    lastName : string,
    sayHi(name:string) : string
    fullName():string
}

// implements : 구현하겠다 ~ 뜻
class Player implements User {
    constructor(
        public firstName:string,
        public lastName:string,
    ){}
    fullName(){
        return `${this.firstName} ${this.lastName}`;
    }
    sayHi(name:string){
        return `Hello ${name}. name is ${this.fullName()}`;
    }
}
```
인터페이스를 상속할 때는 property를 private, protected 로 만들지 못한다.  
property는 public만 가능하다.  

interface는 여러가지를 상속할 수 있다.
```ts
interface User {
    firstName : string,
    lastName : string,
    sayHi(name:string) : string
    fullName():string
}

interface Human {
    age: number
}

// implements : 구현하겠다 ~ 뜻
class Player implements User, Human {
    constructor(
        public firstName:string,
        public lastName:string,
        public age:number
    ){}
    fullName(){
        return `${this.firstName} ${this.lastName}`;
    }
    sayHi(name:string){
        return `Hello ${name}. name is ${this.fullName()}`;
    }
}
```
어댑터 패턴과 같은 디자인 패턴을 사용하여 팀과 함께 일할때 인터페이스를 만들어두고 팀원이 원하는 각자의 방식으로 클래스를 상속하도록 하는건 매우 좋은 방법이다.  

