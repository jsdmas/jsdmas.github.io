---
title: "JavaScript 내장기능"
execrpt: "자주쓰이는 js 내장기능 설명"
toc: true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2022-09-12
---
## isNaN(value)
파라미터로 전달된 값이 NaN일 경우 true, 그렇지 않을 경우 false를 반환한다.
- 숫자가 아니면 true, 숫자가 맞다면 false
- 숫자로 변환 가능한 형식일 경우 false
  
JavaScript의 다른 모든 값과 달리, NaN은 같음 연산(==, ===)을 사용해 판별할 수 없다.  
그래서 NaN 여부를 판별하는 함수가 필요하다.  
`숫자로 반환할 수 없다고 판단하는 경우`  
```js
isNaN(NaN) // 참
isNaN(undefined) // 참
isNaN({a: 10, b: 20}) // 참
isNaN([10,20,30]) // 참
isNaN(`balbal`) // 참
isNaN(`123ABC`) // 참
```
`숫자로 변환할 수 있다고 판단하는 경우`
```js
isNaN(true); // 거짓 -> 1
isNaN(false); // 거짓 -> 0
isNaN(37); // 거짓
isNaN(`37`) // 거짓 : "37"은 숫자 37로 변환.
isNaN(`37.37`) // 거짓 : "37.37"은 숫자 37.37로 변환.
isNaN(``)// 거짓 : 빈 문자열은 0으로 변환
isNaN(`  `) // 거짓 : 공백만으로 구성된 문자열은 0으로 변환.
```
## parseFloat
주어진 값에서 변환한 부동소수점 수(실수)를 리턴. 변환할 수 없으면 NaN을 리턴.  
```js
//정상의 경우
parseFloat(3.14);   // 3.14
parseFloat(`3.14`); // 3.14
parseFloat(`314e-2`);   // 3.14
parseFloat(`0.0314E+2`);    // 3.14

//NaN을 반환하는 경우
parseFloat(`FF2`);  // NaN
```
## parseInt(value, int)
- 첫번쨰 파라미터를 10진 정수값으로 변환한다.
- 변환할 수 없다면 NaN을 반환.
- 두 번쨰 파라미터는 value가 어떤 진법인지를 알려주는 값.(기본값 = 10)
- 문자열의 선행 공백은 무시한다.
- 숫자 + 글자 형태의 문자열은 숫자 부분만 취한다.
- 글자 + 숫자 형태의 문자열은 변환 불가 - `NaN`
- 소수점을 포함하고 있을 경우 정수부분만 취한다.

```
10진법 : 0 1 2 3 4 5 6 7 8  9 10 11 12 13 14 15 16 17 18 19 20 21 -> ex) 12
16진법 : 0 1 2 3 4 5 6 7 8  9  A  B  C  D  E  F 10 11 12 13 14 15 -> ex) 0x12
8진법  : 0 1 2 3 4 5 6 7 10 11 12 13 14 15 16 17 20 21 22 23 24 25-> ex) 0o12
```
  
```js
// 15로 변환
parseInt(`0xF`, 16)
parseInt(`F`, 16)
parseInt(`17`, 8)
parseInt(`015`, 10) // 따옴표를 제거하고 015는 15와 동일
parseInt(15.99, 10) // 소수점 이하는 버린다.
parseInt(`15,123`, 10)  // 콤마(,)는 단순 문자열이므로 콤마 이후는 버려진다.
parseInt(`FXX123`, 16)  // 16진수 기준 정상숫자인 F는 인식되지만 문자열 X 이후로는 버려진다.
parseInt(`1111`, 2)
parseInt(`15*3`, 10)    // 문자열에서 `*`는 곱하기가 아니라 단순 글자이므로 `*`는 버려진다.
parseInt(`15e2`, 10)    // 문자열 `e`이후는 버려진다.
parseInt(`15px`, 10)    //문자열 `px`는 버려진다.

// -15로 변환
parseInt(`-F`, 16)
parseInt(`-0F`, 16)
parseInt(`-0xF`, 16)
parseInt(-15.99, 10) 
parseInt(`-17`, 8) 
parseInt(`-15`, 10) 
parseInt(`-1111`, 2)

// NaN로 변환
parseInt(`Hello`, 8) // 전부 숫자가 아님.
```

## encodeURL
- 주어진 문자열을 URL에 포함시키기에 적절한 형태로 변환(인코딩) 하는 처리
- 인코딩하지 않는 문자 : A-Z a-z 0-9 ; , / ? : @ & = + $ - _ . ! ~ * ' ( ) #
  
`잘못된 경우`  
> <a href="자바스크립트.html">clcik</a>

`올바른 경우`  
> <a href = "%EC%9E%90%EB%B0%94....%BD&8A%B8.html">click</a>


