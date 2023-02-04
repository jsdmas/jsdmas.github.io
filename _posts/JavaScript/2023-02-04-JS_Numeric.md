---
title: "JS Numeric Separators"
execrpt: "js Numeric Separators 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---

코드상에서 숫자를 보기 힘들떄 보기 편하라고 사용한다.    
```js
const Money = 10_000_000_000_000_000_000;
console.log(Money);
// 10000000000000000000
```
원본값을 변화시키지 않고 언더바('_')를 사용해 그냥 보기편하게만 만들어준다.