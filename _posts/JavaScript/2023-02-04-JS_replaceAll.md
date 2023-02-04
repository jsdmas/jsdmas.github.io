---
title: "JS replaceAll"
execrpt: "js replaceAll 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---

[MDN-replaceAll](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)  

string 안에 있는 모든 표적을 대체할 수 있다.  
**중요한건 replaceAll은 본래 변수를 변화시키지 않는다.**   
replaceAll 은 새로운 string을 리턴한다.  

```js
const name = "jinjo";
const newName = name.replaceAll("j", "o");

console.log(name, newName);
// jinjo oinoo
```

