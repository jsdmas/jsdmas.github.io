---
title:  "JS_Array method"
excerpt: "Array관련 method 정리"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-02
---

# Array.from()

[MDN-Array.from](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)  

Array.from() 메서드는 유사 배열 객체(array-like object)나 반복 가능한 객체(iterable object)를 얕게 복사해 새로운Array 객체를 만듭니다.  

# Array.of()

[MDN-Array.of](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/of)  

Array.of() 메서드는 인자의 수나 유형에 관계없이 가변 인자를 갖는 새 Array 인스턴스를 만듭니다.  
Array.of()와 Array 생성자의 차이는 정수형 인자의 처리 방법에 있습니다. Array.of(7)은 하나의 요소 7을 가진 배열을 생성하지만 Array(7)은 length 속성이 7인 빈 배열을 생성합니다.  

# Array.find()

[MDN-Array.find](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)  

find() 메서드는 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환합니다. 그런 요소가 없다면 undefined를 반환합니다.

# Array.findIndex()

[MDN-Array.findIndex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)  

findIndex()메서드는 제공된 테스트 기능을 충족하는 배열의 첫 번째 요소 인덱스를 반환합니다. 테스트 기능을 만족하는 요소가 없으면 -1이 반환됩니다. 

# Array.fill()

[MDN-Array.fill](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)  

fill() 메서드는 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채웁니다.  

