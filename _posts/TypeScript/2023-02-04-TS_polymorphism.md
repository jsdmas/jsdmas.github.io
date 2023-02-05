---
title: "TS polymorephism"
execrpt: "TypeScript polymorephism 설명"
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-02-05
---

# polymorephism(다형성)
poly는 그리스어로 many, several, much, multi 라는 뜻인데 polygon(다각형) 같은 단어를 생각하면 된다.  
poly는 많은, 다수의 란 뜻이고 gon은 각도란 뜻이다.  
morphos 혹은 morephic 은 form(형태), structure(구조)란 뜻을 가지고 있다.

generic 이란 타입의 placeholder 같은 것이다.  
concrete type을 사용하는 것 대신 쓸 수 있다.  
call signature를 작성할 떄 들어올 확실한 타입을 모를 떄 generic을 사용한다.  

```ts
type ArrPrint = {
    <T>(arr:T[]):T
}
const arrPrint : ArrPrint = (arr) => arr[0];

// arrPrint: <number>(arr: number[]) => number
const a = arrPrint([1,2,3,4]); 
// arrPrint: <boolean>(arr: boolean[]) => boolean
const b = arrPrint([true,false,true]);
// arrPrint: <string>(arr: string[]) => string
const c = arrPrint(["a", "b", "c"]);
// arrPrint: <string | number | boolean>(arr: (string | number | boolean)[]) => string | number | boolean 
const d = arrPrint([1,2,true,false,"hello"]);
```
위의 arrPrint 함수는 많은 형태를 가지고 있다.  
위에서 한것은 typescript에게 타입을 유추하도록 알려준것이다.  
그리고 그 타입의 배열이 될 것이라는 것을 인지하고 그 타입 중 하나를 리턴하도록 만들었다.  

generic 을 2개 사용한 예제

```ts
type ArrPrint = {
    <T, M>(arr:T[], b:M):T
}
const arrPrint : ArrPrint = (arr) => arr[0];

// arrPrint: <number, string>(arr: number[], b: string) => number
const a = arrPrint([1,2,3,4], ""); 
const b = arrPrint([true,false,true], 1);
const c = arrPrint(["a", "b", "c"], true);
const d = arrPrint([1,2,true,false,"hello"], "hi");
```

타입스크립트는 제네릭이 처음 사용되는 지점을 기반으로 이 타입이 무엇인지 알게된다.  
제네릭을 처음 인식했을 떄와 제네릭의 순서를 기반으로 제네릭의 타입을 알게 된다.  

# Conclusions

실사용 예시
```ts
type Player<E> = {
    name: string,
    info: E
}

type PlayerObject = {
    favFood : string
}

const jinho : Player<PlayerObject> = {
    name: "jinho",
    info: {
        favFood : "apple"
    }
};
```