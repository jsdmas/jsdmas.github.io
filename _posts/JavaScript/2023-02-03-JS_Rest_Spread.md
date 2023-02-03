---
title:  "JS Rest & Spread 설명"
excerpt: "JS Rest & Spread 예제,설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-03
---

# spread (전개 구문)
기본적으로 변수를 가져와서 풀어 해치고 전개하는 것이다.  
[MDN-spread](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
```js
const numbers = [1,2,3,4,5];

console.log(numbers); // [1,2,3,4,5] : array
console.log(...numbers); // 1 2 3 4 5 : array속의 값들  
```

```js
const numbers = [1,2,3,4,5];
const friends = ["a", "b", "c"];
console.log( [...numbers, ...friends] );
// [1,2,3,4,5,"a","b","c"]
```

```js
const hello = {
    name : "jinho",
    age: 9999,
};
const hi = {
    handsome: true,
    hi: "hello",
};
console.log({...hello, ...hi});
// {name : "jinho", age: 9999, handsome: true, hi: "hello",}
```

# Rest Parameters

parameters(매개변수) 라는건 우리가 함수에 전달하는 인자들을 이야기 한다.  
[MDN-restParameters](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/rest_parameters)  
rest는 모든 값을 하나의 변수로 축소 (contract) 시켜주는것이다.
rest Parameters의 예제는 아래와 같다.  
```js
const hello = (...potato) => console.log(potato);

hello({
"1", "2", true, 1, 23232, [121231,123,321,321]
});

// 입력한 인자들이 모두 보인다.
```

```js
const first = (firstOne, ...rest) => console.log(`first Params : ${firstOne}`);

first("Mark", "naver", "Google", "hello");
// first Params : Mark
```

  
  
**rest & spread 활용**  
```js
const user = {
    NAME: "jinho",
    age:25,
    password: 12345
};
const rename = ({location="KR", NAME:name = "who?",...user}) => {
    return {location, name, ...user};
}
console.log(rename(user));
// {location: 'KR', name: 'jinho', age: 25, password: 12345}
```