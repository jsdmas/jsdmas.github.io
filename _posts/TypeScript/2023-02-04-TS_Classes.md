---
title: "TS Classes"
execrpt: "TypeScript Classe 설명"
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-02-05
---

```ts
abstract class User {
    constructor(
        protected firstName : string,
        protected lastName : string,
        protected nickName : string
    ){}
    abstract getNickName():void
    getFullName(){
        return `${this.firstName} ${this.lastName}`;
    }
}

class Player extends User {
    // Non-abstract class 'Player' does not implement inherited abstract member 'getNickName' from class 'User'.
    // getNickName 을 만들어 주지 않으면 오류가 나타난다.
    getNickName(){
        console.log(this.nickName);
    }
}

const jinho = new Player("jin", "ho", "jino");
jinho.getFullName();
```
- abstract : 추상  
- private : 비밀
- protected : 보호

📌접근 가능한 위치

| 구분      | 선언한 클래스 내 | 상속받은 클래스 내 | 인스턴스 |
| --------- | ---------------- | ------------------ | -------- |
| private   | ⭕                | ❌                  | ❌        |
| protected | ⭕                | ⭕                  | ❌        |
| public    | ⭕                | ⭕                  | ⭕        |
