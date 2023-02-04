---
title: "JS Error cause"
execrpt: "JS Error cause 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---

[MDN-Error cause](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Error/cause)  

에러에 메세지를 추가할 수 있을 뿐만 아니라 또 다른 정보를 추가하는 기능도 있다.  

```js
try{
    2+2;
    throw new Error("~~@Error@~~", {
        cause:{
            error: "Password is not correct",
            value : 2345,
            message: ["too short", "only number not ok"];
        }
    });
}catch(e){
    console.log(e.cause);
}
```