---
title:  "JS padStart & padEnd"
excerpt: "JS padStart & padEnd 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-04
---

[MDN-padStart](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)  
[MDN-padEnd](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd)
  
- padStart() 메서드는 현재 문자열의 시작을 다른 문자열로 채워, 주어진 길이를 만족하는 새로운 문자열을 반환합니다. 채워넣기는 대상 문자열의 시작(좌측)부터 적용됩니다.

- padEnd() 메서드는 현재 문자열에 다른 문자열을 채워, 주어진 길이를 만족하는 새로운 문자열을 반환합니다. 채워넣기는 대상 문자열의 끝(우측)부터 적용됩니다.

기본적으로 문자열의 맨 앞이나 맨 끝에 padding을 넣는것이다.  
주의할 점으로는 padStart나 padEnd는 결과를 반환한다. 즉 값을 변화시키지 않는다는 뜻이다.  
사용 예시로는 예전에 만들어둔 [JS로 시계만들기](https://jsdmas.github.io/js/JS_Clock/) 를 참조하면 된다.  

