---
title: "TS Call Signatures"
execrpt: "TypeScript Call Signatures 설명"
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-02-04
---
typescript에서 type들을 정해준 것처럼 함수도 아래처럼 타입을 지정해주면 된다.
```ts
type Add = (a:number, b:number) => number;
const add :Add = (a,b) => a+b;
```
만약 아래처럼 return타입을 실수로 적을경우 typescript에서 error가 발생했다고 알려준다.
```ts
type Add = (a:number, b:number) => number;
const add :Add = (a,b) => a+b+"";
// Type 'string' is not assignable to type 'number'.
```
