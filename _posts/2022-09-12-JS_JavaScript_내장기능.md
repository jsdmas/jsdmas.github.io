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
```js
<a href="자바스크립트.html">clcik</a>
```
  
`올바른 경우`  
```js
<a href = "%EC%9E%90%EB%B0%94....%BD%8A%B8.html">click</a>
```
  
```js
const set1 = `;,/?:@&=+$#`; // 예약 문자
const set2 = `-_.!~*'()`; // 비예약 문자
const set3 = `ABC abc 123`; // 알파벳 및 숫자, 공백
const set4 = `자바스크립트`;

// 특수문자(예약문자 및 비예약 문자)를 변환하지 못하기 때문에 UTF-8 환경에서는 사용이 불가.
console.log(encodeURI(set1)); // ;,/?:@&=+$#
console.log(encodeURI(set2)); // -_.!~*'()
console.log(encodeURI(set3)); // ABC%abc%123 (공백은 % 으로 인코딩.)
console.log(encodeURI(set4)); // %EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8

// 인코딩 된 문자열을 원래의 문자열로 역변환 (디코딩)
console.log(decodeURI(encodeURI(set1)));
console.log(decodeURI(encodeURI(set2)));
console.log(decodeURI(encodeURI(set3)));
console.log(decodeURI(encodeURI(set4)));
```

## encodeURIComponent
- 알파벳과 숫자및 비예약 표식을 제외한 모든 글자를 URL에 포함시킬 수 있는 문자열로 인코딩한다.
- URL에 사용해도 문제가 없는 특수문자를 제외하고 모든 글자를 변환  

```js
const set1 = `;,/?:@&=+$#`; // 예약 문자
const set2 = `-_.!~*'()`; // 비예약 문자
const set3 = `ABC abc 123`; // 알파벳 및 숫자, 공백
const set4 = `자바스크립트`;

console.log(encodeURIComponent(set1)); //%3B%2C%2F%3F%3A%40%26%3D%2B%24%23
console.log(encodeURIComponent(set2)); // -_.!~*'()
console.log(encodeURIComponent(set3)); // ABC%20abc%20123
console.log(encodeURIComponent(set4)); // %EC%9E%90%EB%B0%94%EC%8A%A4%ED%81...

//디코딩 수행
console.log(decodeURIComponent(encodeURIComponent(set1)));
console.log(decodeURIComponent(encodeURIComponent(set2)));
console.log(decodeURIComponent(encodeURIComponent(set3)));
console.log(decodeURIComponent(encodeURIComponent(set4)));
```

## setTimeout
- setTimeout(func, int)
- func 는 콜백함수, int 는 1/1000초 단위의 시간값.
- 지정된 함수를 두 번째 인자로 전달된 시간 후에 실행하도록 예약한다. (딜레이 가능)
- setTimeout() 이후의 처리 로직들은 func의 실행 여부와 상관없이 즉시 실행된다.

```js
function foo(){
    for(let i =1; i<10; i++){
        console.log(`2 X ${i} = ${i*2}`);
    }
}
setTimeout(foo, 3000);
console.log(`구구단을 외자`);

// 일반적으로 콜백함수를 별도로 정의하지 않고 즉시 전달한다.
// setTimeout(funtion() {
setTimeout(()=>{
    for(let i =1; i<10; i++){
        console.log(`3 X ${i} = ${i*3}`);
    }
}, 1500);
console.log(`프로그램 종료.`);

```
실행순서 : 구구단을 외자 -> 프로그램 종료 -> 3단 출력 -> 2단 출력

## setInterval
- 지정된 함수를 두 번쨰 인자로 전달된 시간마다 한 번씩 호출한다.(타이머기능)
- setInterval() 이후의 처리 로직들은 func의 실행 여부와 상관없이 즉시 실행된다.
- 타이머를 종료시킬 수 있는 timerid를 반환한다.
- 이 값을 clearInterval() 함수에 전달하면 타이머가 종료된다.
- int는 밀리세컨드(1/1000)초를 의미하는 정수.

```js
let cnt1 = 0;
const timerId1 = setInterval(()=>{
    cnt1++;
    console.log(`[타이머1] ${cnt1} 번째 자동 실행`);
    if(cnt1 == 5){
        console.log(`타이머1 종료`);
        clearInterval(timerId1);
    }
}, 3000);
console.log(`타이머1 시작`);

let cnt2 = 0;
const timerId2 = setInterval(()=>{
    cnt2++;
    console.log(`[타이머2] ${cnt2} 번째 자동 실행`);
    if(cnt2 == 10){
        console.log(`타이머2 종료`);
        clearInterval(timerId2);
    }
}, 1000);
console.log(`타이머2 시작`);
```
실행순서 : 타이머1 시작 출력 -> 타이머2 시작 출력 -> 타이머2 1번쨰 자동실행... -> 타이머1 1 번쨰 자동실행 ... -> 타이머2 종료 -> 타이머1 종료

## String
- 문자열은 그 자체가 객체이다. 객체라는 것은 그 안에 맴버변수(프로퍼티)와 메서드(함수)가 내장되어 있음을 의미.
- 그러므로 일반적으로 생성하는 문자열 변수 안에는 메서드와 프로퍼티가 자동으로 내장된다.

```js
const foo = `Hello world`;
foo.매서드()
```
- 문자열 객체에 내장된 메서드들은 변수가 담고 있는 내용을 가공하거나 특정 내용을 추출하는 기능을 갖는다.
- 이 기능들의 공통점은 `원본 내용은 절대 변하지 않고, 가공 결과를 리턴한다.`

<div markdown="1" class="notice--primary">

`변수 형식으로 문자열 만들기`
```js
const str = `Hello World`;
```
`객체 생성 방식으로 문자열 만들기`
```js
const str = new String(`Hello JavaScript`);
```
`ex`
```js
const msg = `Life is too short, You need Javascript.`;

//문자열의 글자 수를 가져온다. -> 공백과 특수문자 포함.
console.log("문자열 길이 : " + msg.length);

// 파라미터로 설정된 위치의 글자를 리턴한다. --> 위치는 0부터 카운트
console.log("두 번쨰 글자 : " + chatAt(2));

// 모든 문자열은 그 자체로 배열처럼 취급될 수 있다.
console.log(`두 번쨰 글자 : ${msg[2]}`);

// 파라미터로 전달된 내용이 처음 나타나는 위치를 리턴한다.
console.log(`f 가 처음 나타나는 위치 : ${msg.indexOf("f")}`);

// 찾지 못할경우 -1을 반환한다.
console.log(`z 가 처음 나타나는 위치 : ${msg.indexOf("z")}`);

//단어나 문장으로 검색할 경우 일치하는 내용이 시작되는 첫 글자의 위치를 반환.
console.log(`short 가 처음 나타나는 위치 : ${msg.indexOf("short")}`);

//indexOf에 파라미터가 두개인 경우,
// 두 번째 숫자 값은 첫 번째 파라미터의 글자를 찾기 시작하는 위치를 의미한다.
const p = msg.indexOf("i"); // 첫 번째 위치
console.log(`i가 두 번째로 나타나는 위치 : ${msg.indexOf("i", p+1)}`);

// 파라미터로 전달된 글자가 마지막으로 나타나는 위치를 리턴한다.
// 단 이 위치를 문자열의 끝에서 부터 세는 것이 아니라 문자열의 처음부터 센다.
// 찾지 못할 경우 -1을 반환.
console.log(`a 의 마지막 위치 : ${msg.lastIndexOf("a")}`);

//응용
if (msg.indexOf(`Hello`) > -1){
    console.log(`Hello가 포함됨.`);
}else{
    console.log(`Hello가 포함되지 않음.`);
}

// 잘라내기 위한 시작 위치와 끝 위치를 파라미터로 설정한다.
// 지정된 끝 위치의 직전 글자까지만 잘라낸다.
console.log(`문자열 자르기 : ${msg.substring(0, 17)}`);

// 두 번째 파라미터가 없을 경우 지정된 위치부터 끝까지 자른다.
console.log(`문자열 자르기 : ${msg.substring(19)}`);

//모든 글자를 대문자로 변환한다.
console.log(msg.toUpperCase());
//모든 글자를 소문자로 변환한다.
console.log(msg.toLowerCase());

```
`문자열의 앞/뒤 공백 지우기`
```js
console.log(`   Hello World   `.trim());
// Hello World
```
`문자열에 포함된 구분자를 기준으로 문자열을 잘라 배열로 반환한다.`
```js
console.log(`HTML,CSS,JavaScript`.split(","));
// (3) ['HTML', 'CSS', 'JavaScript']
```
`첫 번째 파라미터의 내용을 두 번째 파라미터로 변경한 결과를 반환한다.`
```js
// 첫 번째 파라미터와 일치하는 내용이 둘 이상 존재할 경우 첫 번째 항목만 변경한다.
console.log(`HTML,CSS,JavaScript`.replace(",","$"));
// HTML$CSS,JavaScript

console.log(`Hello JS, World JS`.replaceAll("JS","자바스크립트"));
// Hello 자바스크립트, World 자바스크립트
```
</div>

## Math
-  수학적인 속성과 메서드를 가진 내장 객체
-  모든 기능이 정적 맴버변수와 정적 매서드 형태로 제공된다.
-  즉, 객체 생성을 하지 않고 클래스 이름으로 직접 접근한다.

<div markdown="1" class="notice--primary">

`주어진 값 중에서 최대값(파라미터 수 제한 없음)`
```js
console.log(Math.max(100,123,456,789,1000)); // 1000
```
`주어진 값 중 최소값(파라미터 수 제한 없음)`
```js
console.log(Math.min(100,123,456,789)); // 100
```
`소수점 반올림`
```js
console.log(Math.round(3.7146)); // 4
```
`소수점 올림과 내림`
```js
console.log("올림 : "+ Math.ceil(3.7146));// 4
console.log("내림 : "+ Math.floor(3.7146));// 3
```
`절대값을 반환`
```js
console.log(Math.abs(-123));// 123
```
`0~1 범위의 난수 발생`
```js
console.log(Math.random());// 0.1656573...
```
`두 수 사이의 난수를 리턴하는 함수`
```js
function random(n1, n2){
    return parseInt(Math.random() * (n2 - n1 + 1)) + n1;
}
```
</div>

## Date
- 객체를 생성하는 순간의 시스템 시각이나 생성자 파라미터로 전달된 시각을 플랫폼에 종속되지 않는 형태로 나타낸다.

<div markdown="1" class="notice--primary">

`객체 생성`
```js
const date = new Date();
```
`년,월,일,시간,분,초 리턴받기`
```js
const date = new Date();
console.log(date.getFullYear());
// 월은 0이 1월 , 11이 12월을 의미한다.
console.log(date.getMonth() + 1);
console.log(date.getDate());
// 0=일요일 ~ 6=토요일의 값이 리턴된다.
const i = date.getDay();
const days = [`일`, `월`, `화`, `수`, `목`, `금`, `토`];
console.log(days[i]);

console.log(date.getHours());
console.log(date.getMinutes());
console.log(date.getSeconds());
```
`날짜를 의미하는 문자열 반환받기`
```js
console.log(new Date().toDateString());
// Mon Sep 12 2022
```
`ISO 8601 확장 형식`
```js
console.log(new Date().toISOString());
// 2022-09-12T09:11:39.754Z
```
`형식 문자열을 사용해서 Date를 나타내는 문자열을 생성`
```js
console.log(new Date().toLocaleString());
// 2022. 9. 12. 오후 6:12:46
console.log(new Date().toLocaleString(`de-DE`));
// 12.9.2022, 18:13:19
console.log(new Date().toLocaleString(`ko-KR`));
// 2022. 9. 12. 오후 6:13:19
```
`Date의 날짜 부분을 나타내는 문자열을 시스템에 설정된 현재 지역의 형식으로 반환`
```js
console.log(new Date().toLocaleDateString());
// 2022. 9. 12.
console.log(new Date().toLocaleDateString(`de-DE`));
// 12.9.2022
console.log(new Date().toLocaleDateString(`ko-KR`));
// 2022. 9. 12.
```
`Date의 시간 부분을 나타내는 문자열을 시스템에 설정된 현재 지역의 형식으로 반환`
```js
console.log(new Date().toLocaleTimeString());
// 오후 6:18:05
console.log(new Date().toLocaleTimeString(`de-DE`));
// 18:18:05
console.log(new Date().toLocaleTimeString(`ko-KR`));
// 오후 6:18:05
```

</div>

## Date 활용하여 날짜 계산
- timestamp -> 1970년 1월 1일 자정부터 현재까지 흐른 시간을 초단위로 환산한 값.
- getTime()함수는 timestamp를 밀리세컨드단위로 환산하여 반환한다.

`날짜 계산`
```js
const date = new Date();
const ts1 = date.getTime();
console.log(ts1); // 1662974458464
```
`몇일이 지났는지 계산하기`
```js
const date = new Date();
const ts1 = date.getTime();
const prevDate = new Date(date.getFullYear(),0,1);
const ts2 = prevDate.getTime();
const tmp1 = ts1 - ts2;
console.log(tmp1);

// 계산 결과를 원하는 단위로 환산 --> 24시간 * 60분 * 60초 * 1000
// 과거의 시점으로부터 지나온 시간을 계산할 경우 소수점을 내린다. 
const day1 = Math.floor(tmp1/ (24*60*60*1000));
console.log(`올해는 ${day1} 일 지났습니다.`);
```
`몇일이 남았는지 계산하기`
```js
const ts1 = new Date().getTime();
const nextDay = new Date(new Date().getFullYear(), 11, 31);
const ts3 = nextDay.getTime();
const tmp2 = ts3 - ts1;
//미래의 시점으로 남은 시간을 계산할 경우 소수점을 올린다.
const day2 = Math.ceil(tmp2/ (24*60*60*1000));
console.log(`올해는 ${day2} 일 남았습니다.`);
```

`지금으로부터 30일 후`
```js
//단위가 수용할 수 있는 값이 초과될 경우 자동으로 올림 처리한다.
const date = new Date();
const a = date.getDate() + 30;
date.setDate(a);
console.log(date.toLocaleString(`ko-KR`));
// 주의 할 점으로 새로운 date객체를 생성하며 진행할 경우 계산 결과가 맞지 않는다.
```
`30일이 지난 후에서 다시 100일 전을 계산`
```js
const date = new Date();
const a = date.getDate() + 30;
date.setDate(a);
const b = date.getDate() - 100;
date.setDate(b);
console.log(date.toLocaleString(`ko-KR`));
```
`응용`
```js
// 오늘 날짜 객체
const today = new Date();
// 이번달의 1일로 이동
today.setDate(1);
// 이번달 1일에 대한 요일 인덱스
const startDay = today.getDay();
console.log(startDay);

//이번달의 마지막날은 몇일인지 구함. -> 다음달의 0번째 날짜를 구함.
const m = today.getMonth();
today.setMonth(m+1);
today.setDate(0);
const lastDate = today.getDate();
console.log(lastDate);

//6행 7열의 빈 배열 만들기
var data = new Array(6);
for(let i =0; i< data.length; i++){
    data[i] = new Array(7);
}
// 1씩 증가할 값
let cnt = 1;
for (let i = 0; i<data.length; i++){
    for(let j = 0; j<data[i].length; j++){
        if(i == 0 && j < startDay || cnt > lastDate){
            data[i][j] = 0;
        }else{
            data[i][j] = cnt++;
        }
    }
}
for(let i =0; i< data.length; i++){
    let str = ``;
    for (let j=0; j<data[i].length; j++){
        str += data[i][j] == 0 ? "\t" : (data[i][j] + "\t");
    }
    console.log(str);
}
```
## Regex(정규표현식)
- 문자열의 형식을 의미하는 수식
- 문자열이 특정 조건을 충족하는지 검사하거나 특정 패턴의 문자열을 검색, 치환 하기 위해 사용함.
- const 변수명 = /정규표현식/
- 변수명.test(검사할 문자열) -> 문자열이 정규표현식에 부합할 경우 true를 반환함.

```js
// 회원가입시 입력받은 값을 가정한 변수
// 사용자가 입력한 모든 내용은 문자열 변수이다.
const username = `자바스크립트`;
const age = `20`;
const userid = `js1234`;

//이름의 한글 입력 검사
const pattern1 = /^[ㄱ-ㅎ가-힣]*$/;
//username이 pattern1 정규식에 부합하지 않는다면?
if(!pattern1.test(username)){
    console.log(`이름은 한글만 입력 가능합니다.`);
}

// 나이의 숫자 입력 검사
const pattern2 = /^[0-9]*$/;
if(!pattern2.test(age)){
    console.log(`나이는 숫자만 입력 가능합니다.`);
}

// 아이디의 영문+숫자 검사
const pattern3 = /^[a-zA-Z0-9]*$/;
if(!pattern3.test(userid)){
    console.log(`아이디는 영어+숫자 조합으로만 입력 가능합니다.`);
}

// 아이디의 최대 글자 수 검사
if(userid.length > 20){
    console.log(`아이디는 최대 20자 까지만 입력 가능합니다.`);
}

console.log(`검사완료.`);
```
## Array
<div markdown="1" class="notice--primary">

`배열 원소 추가, 삭제, 변경 방법`
```js
const data = [10,20,30,40,50];
data.push(60,70); // 배열 맨 끝에 요소 추가 (파라미터 제한 없음)
console.log(data); // (7) [10, 20, 30, 40, 50, 60, 70]

const k = data.pop(); // 마지막 요소를 리턴하고 제거
console.log(k);  // 70
console.log(data);  // (6) [10, 20, 30, 40, 50, 60]

const x = data.shift(); // 맨 앞 요소를 리턴하고 제거
console.log(x);     // 10
console.log(data);  // (5) [20, 30, 40, 50, 60]

data.unshift(0,10); // 맨 앞에 요소 추가 (파라미터 수 제한 없음)
console.log(data);  // (7) [0, 10, 20, 30, 40, 50, 60]

const z = data.splice(2,3); // 2번째 위치부터 3개를 잘라서 반환하고 원본에서는 제거
console.log(z); // (3) [20, 30, 40]
console.log(data); // (4) [0, 10, 50, 60]

// 0번째 위치부터 2개를 제거하고 그 위치에 다른 요소들을 배치함
// 제거한 수보다 추가되는 원소수가 많을 경우 배열이 확장됨. 기존의 원소들은 뒤로 밀림.
// 제어한 수보다 추가되는 원소수가 적을 경우 배열이 축소됨.
data.splice(0,2, `a`, `b`, `c`);
console.log(data);  // (5) ['a', 'b', 'c', 50, 60]
```
`배열 결합`
```js
const a = [1,2];
const b = [3,4,5];
const c = [6,7,8,9];
// a에 b와 c를 결합한 새로운 배열을 반환.
const d = a.concat(b,c);
console.log(d); // (9) [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
`배열 원소 확인`
```js
// 배열에서 원하는 원소가 있는지 여부 확인하기
let arr = [1, 0, false];

// arr.indexOf(item, from)는 인덱스 from부터 시작해 item(요소)을 찾는다.
// 요소를 발견하면 해당 요소의 인덱스를 반환한다.
// arr.lastIndexOf(item, from)는 위 메서드와 동일한 기능을 하는데, 검색을 끝에서부터 시작한다.
// 두 번째 파라미터 (from)이 없으면 처음부터 탐색한다.
console.log(arr.indexOf(0)); // 1
console.log(arr.indexOf(false)); // 2
//발견하지 못하면 -1 반환
console.log(arr.indexOf(null)); // -1

// arr.includes(item, from)는 인덱스 from부터 시작해 item이 있는지를 검색하는데, 
// 해당하는 요소를 발견하면 true를 반환한다.
console.log(arr.includes(1)); // true
console.log(arr.includes(100)); // false

/**
 * indexOf 메서드는 요소를 찾을 때 완전 항등 연산자 === 을 사용한다는 점에 유의해야 한다.
 * false를 검색하면 정확히 false만을 검색하지, 0을 검색하진 않는다.
 * 요소의 위치를 정확히 알고 싶은게 아니고 요소가 
 * 배열 내 존재하는지 여부만 확인하고 싶다면 arr.includes를 사용하는 게 좋다.
 * includes는 NaN도 제대로 처리한다는 점에서 indexOf/lastIndexOf와 약간의 차이가 있다.
 */
const arr2 = [NaN];
console.log(arr2.indexOf(NaN)); // -1 (완전 항등 비교 === 는 NaN엔 동작하지 않으므로 0이 출력되지 않는다.)
console.log(arr2.includes(NaN)); // true (NaN의 여부를 확인한다.)
```
`forEach`
- forEach 메서드를 활용하여 배열의 모든 원소 탐색.
- 콜백함수에게 배열의 값과 인덱스를 전달한다.
- 콜백함수는 원소의 수 만큼 순차적으로 실행된다.

```js
const arr = [10, 20, 30 ,40 ,50];
arr.forEach((v,i)=>{
    if (i == 3){
        console.log(`---반복중단`);
        // break는 for,while 문에서만 사용 가능하기 때문에 함수 안에서는 사용할 수 없다.
        // forEach의 콜백함수에서 반복을 중단하고자 return을 사용할 경우 현재 동작중인
        // 콜백만 종료될 뿐 전체 반복에는 영향이 없다.
        return;
    }
    console.log(`${i}번째 원소 => ${v}`);
});
/**
 0번째 원소 => 10
 1번째 원소 => 20
 2번째 원소 => 30
 ---반복중단
 4번째 원소 => 50
 */
```

`some`
- 탐색을 중단하는 기능을 제공하는 some함수
- some 함수는 콜백함수가 true를 리턴하는 순간 순환을 중단한다.
```js
const arr = [10,20,30,40,50];
arr.some((v,i)=>{
    if(i == 3){
        console.log(`---반복중단`);
        return true;
    }
    console.log(`${i}번째 원소 => ${v}`);
});
/**
 0번째 원소 => 10
 1번째 원소 => 20
 2번째 원소 => 30
 ---반복중단
 true
*/
```
`map`
- 배열의 원소를 가공하여 새로운 배열 만들기
- map함수에 전달된 콜백이 리턴하는 값들을 하나의 배열로 묶어서 hello에 저장
- hello는 반드시 원본 배열과 같은 길이를 갖는 배열이다.
- 리턴하지 않은 index에 대한 원소는 undefined가 된다.
```js
const arr = [10,20,30,40,50];
const hello = arr.map((v,i)=> v * 10 );
console.log(hello); // (5) [100, 200, 300, 400, 500]
```

`find (배열 검색)`
- 주어진 판별함수를 만족하는 첫번째 값을 반환한다. 못찾으면 undefined를 반환한다.
- 찾고자 하는 항목이 아닌 검색 규칙을 구현한 콜백함수를 전달해야 한다. 

```js
const arr = [5,12,8,131,44];
const found = arr.find((v)=>{
    //파라미터로 전달되는 v는 배열의 모든 원소가 순차적으로 전달된다.
    console.log(v);

    // v를 우리가 원하는 조건에 충족하는지 검사하여 true/false를 리턴
    // true를 리턴하는 순간 배열의 탐색을 중단한다. (break와 동일한 기능)
    if( v%2 == 0){
        // true가 리턴되는 경우 v는 found에 저장된다.
        return true;
    }else{
        // false가 리턴되는 경우 v는 버려진다.
        return false;
    }
});
console.log(found);
```

순서 : 5 대입 -> 12 대입 (return true) -> found값 : 12 -> 종료  
  
`응용`

```js
// forEach를 사용해 배열에서 특정 조건을 충족하는 원소 추출.
const arr = [5,12,8,131,44];
const d = new Array();
arr.forEach((v,i)=>{
    if(v%2 == 0){
        d.push(v);
    }
});
console.log(d); // (3) [12, 8, 44]

// filter 이용
const arr2 = [5,12,8,131,44];
const d2 = arr2.filter(function(v,i,arr) {
    console.log(`v=${v}, i=${i}, arr=${arr}`);
    if(v%2==0){
        //true가 리턴되는 경우 v는 results 배열의 원소로 저장된다.
        //true를 리턴하더라도 배열의 모든 원소를 탐색하기 전까지는 종료되지 않는다.
        return true;
    } else{
        //false가 리턴되는 경우 v는 버려진다.
        return false;
    }
});
console.log(d2); // (3) [12, 8, 44]
```

`배열 정렬`
- 퀵정렬 알고리즘을 사용하여 배열 자체를 정렬한다.
- 배열의 모든 원소를 문자열로 취급하기 때문에 글자 정렬기준이 적용된다.
```js
const arr = [2,1,15];
// sort 함수도 정렬 조건을 콜백함수로 처리한다.
arr.sort((a,b)=>{
    // 정렬을 위해 비교되는 원소값들이 파라미터로 전달된다.
    console.log(`a=${a}, b=${b}`);

    //리턴값이 양수인 경우: a가 b보다 크다.
    //리턴값이 음수인 경우: b가 a보다 크다.
    if(a>b){
        return 1;
    }else{
        return -1;
    }
});
console.log(arr);
/**
 a=1, b=2
 a=15, b=1
 a=15, b=2
 (3) [1, 2, 15]
 */
```

`역순배치`
```js
let arr = [1,2,3,4,5];
arr.reverse();
console.log(arr); // (5) [5, 4, 3, 2, 1]
```
`reduce`
- 배열의 각 요소를 순회하며 callback함수의 실행 값을 누적하여 하나의 결과값을 반환
- accumulator(누산기) : 직전 콜백이 리턴한 값
- index : 이번 회차에 탐색되는 배열 원소의 인덱스 (생략가능)
- array : 배열 원본 (여기서는 arr자체, 생략가능)
- 최초 실행시 accumulator에는 0 번째 원소인 1이 전달되고 currentValue(현재 값)에는 1번째 원소인 2가 전달되며, index는 currentValue에 대한 1이 전달된다.
- 두 번째 실행부터 accumulator에는 이전 회차에서 리턴한 값이 되돌아온다. 그리고 currentValue에는 2번째부터 순서대로 매 실행회차마다 다음 원소가 전달된다.
- 즉, reduce는 배열의 모든 원소를 탐색하면서 누적 결과를 만들고자 할 경우 사용한다.

```js
const arr = [1,2,3,4,5];
const result = arr.reduce((accumulator, currentValue, index, array) =>{
    console.log(`accumulator=${accumulator}, currentValue=${currentValue}, index=${index}`);
    return accumulator + currentValue;
});
console.log(`result = ${result}`);

//불필요한 파라미터 생략, 코드 간단히 표현
const result2 = arr.reduce((acc, cur)=> acc + cur);
console.log(`result2 = ${result2}`);
/**
 accumulator=1, currentValue=2, index=1
 accumulator=3, currentValue=3, index=2
 accumulator=6, currentValue=4, index=3
 accumulator=10, currentValue=5, index=4
 result = 15
 result2 = 15
 */


// accumlator의 초기값 지정하기
// reduce의 콜백함수 다음에 두 번째 인자로 accumlator의 초기값을 설정할 수 있다.
const result3 = arr.reduce((accumulator, currentValue, index, array) =>{
    console.log(`accumulator=${accumulator}, currentValue=${currentValue}, index=${index}`);
    return accumulator + currentValue;
}, 2);
// 더하는건 같은데 걍 더하는 시작값만 다르게 설정할떄 사용.
console.log(`result3 = ${result3}`)
/**
 accumulator=2, currentValue=1, index=0
 accumulator=3, currentValue=2, index=1
 accumulator=5, currentValue=3, index=2
 accumulator=8, currentValue=4, index=3
 accumulator=12, currentValue=5, index=4
 result3 = 17
*/
```

`응용`
```js
const covid19 = [
    {date: `0125`, active: 426},
    {date: `0126`, active: 324},
    {date: `0127`, active: 764},
    {date: `0128`, active: 434},
    {date: `0129`, active: 432},
    {date: `0130`, active: 222},
    {date: `0131`, active: 321},
    {date: `0132`, active: 980}
];
// 전체 확진자 수 구하기
// 객체를 탐색할 때는 accumulator의 초기값을 설정하고 0번째 원소부터 currentValue로 받아야 한다.
const total = covid19.reduce((acc, cur)=> acc + cur.active, 0);
console.log(`전체 확진자 수 : ${total}`); // 전체 확진자 수 : 3903
console.log(`평균 확진자 수 : ${total/covid19.length}`); // 평균 확진자 수 : 487.875
```
</div>