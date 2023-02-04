---
title:  "JS Optional Chaining (?.)"
excerpt: "Optional Chaining 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---

[MDN-Optional Chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)  

```js
const jinho ={
    profile:{
        email:{
            provider: "hello~"
        }
    }
};

console.log(jinho.profile && jinho.profile.email && jinho.profile.email.provider);

// 위의 코드를 optional chaining을 사용하면 아래처럼 간략하게 사용할 수 있다.

console.log(jinho?.profile?.email?.provider);
```
jinho가 존재하고 profile이 존재하고 email이 존재하면 provider를 출력해라. 라는 뜻이다.  

위처럼 && 연산자를 여러번 쓰지 않고 표현할 수 있다.
