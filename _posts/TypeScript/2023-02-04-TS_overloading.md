---
title: "TS Overloading"
execrpt: "TypeScript Overloading 설명"
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-02-04
---

오버로딩은 함수가 여러개의 call signatures를 가지고 있을 때 발생시킨다.   
그냥 여러 개가 아니라 서로 다른 여러 개의 call signature 를 가졌을 떄 오버로딩이 발생한다.  

```ts
type Add = {
    (a:number, b:number) : number
    (a:number, b:string) : number
}

const add : Add = (a,b) => {
    // (parameter) b: string | number
    // b는 숫자,문자도 될 수 있어서 아래처럼 조건을 나누어 줘야한다.
    if(typeof b === 'string') return a
    return a + b;
}
```

다른 예시
```ts
type Config = {
    path:string,
    state:object
}
type Push = {
    (path:string):void
    (config:Config):void
}

// (parameter) config: string | Config
const push:Push = (config) => {
    // (parameter) config: string
    if(typeof config === 'string') {console.log(config)}
    else{
        // (parameter) config: Config
        console.log(config.path, config.state);
    }
}
```
핵심은 config가 string이나 Config 타입을 가지고 있다면 타입스크립트는 내부에서 그 타입을 체크하도록 해준다.   

파라미터의 개수가 다를떄 예제
```ts
type Add = {
    (a:number, b:number) : number
    (a:number, b:number, c:number) : number
}
const add:Add = (a,b,c?:number) => {
    if(c) return a + b + c
    return a+b
}

add(1,2);
add(1,2,3);
```

