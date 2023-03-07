---
title:  "JS_String method"
excerpt: "string관련 method 정리"
toc : true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-03-05
---

# includes
[MDN-includes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

includes() 메서드는 배열이 특정 요소를 포함하고 있는지 판별한다.  
```js
const array1 = [1, 2, 3];

console.log(array1.includes(2)); // true

const pets = ['cat', 'dog', 'bat'];

console.log(pets.includes('cat')); // true

console.log(pets.includes('at')); // false
```

# repeat

[MDN-repeat](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)  
string.repeat은 원하는 어떤 글자던지 반복할 수 있다.  

# startsWith

[MDN-startsWith](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith)  
startsWith() 메서드는 어떤 문자열이 특정 문자로 시작하는지 확인하여 결과를 true 혹은 false로 반환한다.

# endsWith

[MDN-endsWith](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)  

endsWith() 메서드를 사용하여 어떤 문자열에서 특정 문자열로 끝나는지를 확인할 수 있으며, 그 결과를 true 혹은 false로 반환한다.  