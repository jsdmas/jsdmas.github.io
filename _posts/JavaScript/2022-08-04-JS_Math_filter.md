---
title:  "JS 숫자, 수학 method (Number, Math)"
excerpt: "Number ,Math method 설명"
toc : true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-03-05
---
# 10진수 -> 2진수/16진수 toString
toString() 은 문자열을 반환하는 object의 대표적인 방법입니다 [MDN-toString](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)   
```js
let num = 10;
num.toString(); // "10";
num.toString(2); // "1010" 10을 2진수로 나타냄(string); 

let num2 = 255;
num2.toString(16); // "ff" 255를 16진수로 변환
```

# Math.PI
원주율을 구해줍니다
```js
Math.PI // 3.141592653589793
```

# Math.ceil() - 올림
```js
let num1 = 5.1;
let num2 = 5.7;

Math.ceil(num1) // 6
Math.ceil(num2) // 6
```
# Math.floor() - 내림
```js
let num1 = 5.1;
let num2 = 5.7;

Math.floor(num1) // 5
Math.floor(num2) // 5
```
# Math.round() - 반올림
```js
let num1 = 5.1;
let num2 = 5.7;

Math.round(num1) // 5
Math.round(num2) // 6
```

# toFixed() - 소수점 자리수
숫자를 인수로받아 그 숫자만큼 소수점을 표현한다.(**반환값 string**)  
  
**toFixed 사용X**  
예) 소수점 `둘째자리` 까지 표현 (셋쩨 자리에서 반올림)
```js
let userRate = 30.1234;
Math.round(userRate * 100) / 100 // 30.12
```
  
**toFixed 사용O**
```js
let userRate = 30.1234;
userRate.toFixed(2); // "30.12" (string)
ueerRate.toFixed(0); // "30"
ueerRate.toFixed(6); // "30.123400"
```

# IsNaN
`isNaN()`함수는 어떤 값이 NaN인지 판별합니다.  
[MDN-IsNaN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/isNaN)  

```js
let x = Number("x"); // NaN
x == NaN // false
x === NaN // false
NaN == NaN // false
isNaN(x) // true
isNaN(3) // false
```

# parseInt
문자열을 숫자로 바꿔줍니다. Number와 다른점은 문자가 혼용되어있어도 작동을 한다는 점입니다.    
두번째 인수에 숫자를 전달해서 16진수, 2진수 등으로 바꿀 수 있습니다.  

```js
let margin = "10px";
parseInt(margin); // 10
Number(margin); // NaN

// paseInt는 읽을수있는 부분까지는 읽고 문자를 만나면 숫자를 반환합니다.
// 아래의 예시는 문자가 먼저있어서 뒤의 숫자를 못읽고 NaN을 반환합니다.
let redColor = "f3";
parseInt(redColor); // NaN
```
**16진수 변환**  
```js
let redColor = "f3";
parseInt(redColor, 16); // 243
```

**문자열을 숫자로 변환 후 2진수 변환**
```js
parseInt("11",2) // 3
```

# parseFloat
parseInt와 동일하게 동작하지만 부동소숫점을 반환합니다.  
```js
let padding = "18.5%";
parseInt(padding); // 18
parseFloat(padding); // 18.5
```

# Math.random()
0 ~ 1 사이 무작위 숫자 생성  
  
**1 ~ 100 사이 임의의 숫자를 뽑고 싶다면?**  
```js
Math.floor(Math.random() * 100) + 1;
```
1. 랜덤숫자 * 뽑고싶은 숫자범위 -> 만약 5까지의 숫자를 뽑고 싶다면 (Math.random() * 5)
2. floor로 소수점이하 버리기.
3. 마지막으로 1을 더하는 이유는 랜덤 숫자로 0.0~ 이 나올 수 있어서 floor시 0이 나올 수 도있기 때문에 더해줍니다.
4. 만약 0에서 100까지 구하고 싶다면 +1을 더해주지 않아도 됩니다.

# Math.max(), Math.min()
괄호안의 인수들 중 최댓값, 최소값을 구합니다.
```js
Math.max(1, 4, -1, 10, 2, 9, 5, 5.54); // 10
Math.min(1, 4, -1, 10, 2, 9, 5, 5.54); // -1
```

# Math.abs 
절대값을 구해줍니다.
```js
Math.abs(-1) // 1
```
* abs 는 absolute의 약자입니다.

# Math.pow(n,m)
제곱 값을 구해줍니다. (n의 m승 값)  
```js
Math.pow(2,10); // 1024
```
* pow 는 power의 약자입니다.

# Math.sqrt()
제곱근을 구해줍니다.
```js
Math.sqrt(16) // 4
```

* sqrt는 Square Root의 약자입니다.





 





