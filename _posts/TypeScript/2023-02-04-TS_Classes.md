---
title: "TS Class"
execrpt: "TypeScript Class 예제"
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-02-05
---

[TS_Class](https://poiemaweb.com/typescript-class)
[TS_Documnet_Class](https://www.typescriptlang.org/docs/handbook/2/classes.html)

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
    // 추상 메소드가 있는 경우, 추상 클래스를 상속받는 클래스에서 추상 메소드를 구현해주어야 한다.
    getNickName(){
        console.log(this.nickName);
    }
}

const jinho = new Player("jin", "ho", "jino");
jinho.getFullName();
```
- abstract : 추상  
  - 추상 클래스는 직접적으로 인스턴스를 만들 수 없다.
  - 추상 메소드는 구현 되어 있지 않은 (코드가 없는) 메소드다.
  - 추상 클래스는 다른 클래스가 가져야 할 property 랑 메소드를 명시할 수 있도록 도와준다.
- private : 비밀
- protected : 보호

📌접근 가능한 위치

| 구분      | 선언한 클래스 내 | 상속받은 클래스 내 | 인스턴스 |
| --------- | ---------------- | ------------------ | -------- |
| private   | ⭕                | ❌                  | ❌        |
| protected | ⭕                | ⭕                  | ❌        |
| public    | ⭕                | ⭕                  | ⭕        |


# Class를 Type으로 받는 예제

단어사전 예제
```ts
// 단어타입
type Words = {
    [key:string] : string
}

// 참조 단어 클래스
class Word {
    constructor(
        // 클래스 내부에서 구조체를 통해 받아서 지정해줘야함.
        public readonly term : string,
        public readonly def : string
    ){}
}

// 사전
class Dict {
    private words : Words
    constructor(){
        // words는 구조체에서 받아오지않는다.
        // 따라서 {} 안에 정의만 해줘야함.
        this.words = {}
    }

    // Word class type 참조
    add(word:Word){
        if(this.words[word.term] === undefined){
            this.words[word.term] = word.def
        }
    }

    def(term:string){
        return this.words[term];
    }
}

const potato = new Word("potato", "감자"); // 단어생성
const dict = new Dict(); // 사전생성
dict.add(potato); //단어추가
dict.def("potato") // 감자
```

# Polymorephism Class
다형성, 제네릭, 클래스, 인터페이스를 활용해 클래스를 만들어봤다.  
다형성 : 다른 모양의 코드를 가질 수 있게 해 주는것. 다형성을 이룰 수 있는 방법은, 제네릭을 사용하는 것이다.  

localStorageAPI 타입 연습
```ts
interface SStorage<T>{
    [key:string] : T
}

class LocalStorage<T>{
    private storage : SStorage<T>= {}
    set(key:string, value:T){
        this.storage[key] = value
    }
    remove(key:string){
        delete this.storage[key]
    }
    get(key:string){
        return this.storage[key]
    }
    clear(){
        this.storage = {}
    }
}

// constructor LocalStorage<string>(): LocalStorage<string>
const stringStorage = new LocalStorage<string>();
// (method) LocalStorage<string>.set(key: string, value: string): void
stringStorage.set("jinho","handsome");

// constructor LocalStorage<boolean>(): LocalStorage<boolean>
const booleanStorage = new LocalStorage<boolean>();
// (method) LocalStorage<boolean>.set(key: string, value: boolean): void
booleanStorage.set("hello", true);

```