---
title:  "JS Array, JSON 설명"
excerpt: "JS Array, JSON, 기본개념"
toc : true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2022-08-28
---

## Array 정리

### 값이 존재하지 않는 5개의 빈 칸을 갖는 배열 만들기
```js
let myArr = new Array(5);

```
### 배열에서 최대값 찾기
```js
const data = [5,2,4,8,9];
let max = data[0];  //비교를 위해 첫 번째 element 복사.

for(let i =1; i < data.length; i++){
    if(max < data[i]){
        max = data[i];
    }
}
console.log(`최대값 = ${max}`);
```
### 배열 역순으로 배치하기
```js
const data = [1,2,3,4,5];
const p = parseInt(data.length/2); // 반복횟수
for(let i = 0; i < p; i++){
    const k = data.length -i -1; //반대쪽 element 위치
    const tmp = data[i];
    data[i] = data[k];
    data[k] = tmp;
}
console.log(data);
```
### 배열 크기순서대로 정렬
```js
const data = [1,2,3,4,5];
for(let i=0; i<data.length-1;i++){  //마지막 항목을 제외
    for(let j=i+1; j<data.length;j++){  //i번쨰 다음항목과 비교하기위해 i+1
        if(data[i] > data[j]){
            const tmp = data[j];
            data[j] = data[i];
            data[i] = tmp;
        }
    }
}
console.log(data);
```
### 2차배열
```js
const a = [1,2,3];
const b = [4,5,6];
const arr = [a,b];  //[ [1,2,3], [4,5,6] ];
// 행 -> 열 순으로 인덱스 열거
/** arr[0][0] -> 1 , arr[0][1] -> 2 , arr[0][2] -> 3
 *  arr[1][0] -> 4 , arr[1][1] -> 5 , arr[1][2] -> 6
 */
const arr2 = [
    [1,2,3],
    [4,5,6]
];
console.log(arr.length); //값:2 , 배열 자체의 길이는 행을 의미.
```
### for~of문을 활용한 탐색
```js
const arr = [
    [1,2,3],
    [4,5,6]
];
for (const item of arr){
    console.log(item);  //  [1,2,3]
                        //  [4,5,6]
    for(const sub of item){
        console.log(sub);
        // 배열안의 element 들을 살핀다 -> 1,2,3 , 4,5,6
    }
}
```
## JSON
- [JSON_정의](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)

### JSON 파일 예시
```js
const student = {   //key: value 형식으로 나열
    studno: 101010,
    grade: 1,
    name: "진호",
    handsome: false,
    age:null,
    phoneno: "010-1111-1111"
};
/** student.grade -> 1  (객체이름.하위정보이름)
 *  student.["name"] -> 진호 (json의 key이름을 배열 인덱스처럼 활용)
 */
```
### JSON 데이터 탐색

- 2가지 방법이 있다.(key 배열로변환, for~in 사용)

```js
// json의 key를 배열로 반환
const keys = Object.getOwnPropertyNames(student);
// [studno, grade, name, handsome, age, phoneno]
for(const item of keys){
    console.log(student[item]);  //추출한 key이름은 배열이므로 반복문 처리가능.
    // 추출된 key이름값(item)을 활용하여 실 데이터에 동적 접근 가능.
}

// JSON객체의 key를 선언된 변수(k)에 순차적으로 할당하며 모든 key를 탐색한다.
for(const k in student){
    console.log(student[item]);
    //101010, 1, 진호, false, null, 010-1111-1111
}
```

### JSON 확장

```js
const foo = {
    name : "ILoveJS",
    age:5
};
// 존재하지 않는 값 출력시 undefined, 존재하지 않는 값을 활용한 연산시 (undefined+1 = NaN)
foo.email = "hello@naver.com"; // 존재하지 않는 key에 대입
console.log(foo); //{ name: 'ILoveJS', age: 5, email: 'hello@naver.com' }

const Json ={};// 빈 객체 확장
for(let i =0; i<10; i++){
    const key = "value" + i;
    Json[key] = i*100;
}
console.log(Json);
/**{
  value0: 0,
  value1: 100,
  value2: 200,
  value3: 300,
  value4: 400,
  value5: 500,
  value6: 600,
  value7: 700,
  value8: 800,
  value9: 900
}
 */ 
```
## 복사
### 값복사
일반 변수끼리의 복사
```js
let a = 100;
let b = a; // b = 100
// 일반 변수끼리 복사한 경우 원본이 변경되면 복사본에는 영향이 없다.
a++;    // a=101, b=100
```
### 참조복사
얕은복사 라고도 하며 배열, JSON, 객채 끼리의 복사는 참조처리된다.  
`원본을 수정하면 복사본도 함께 수정된다.(반대의 경우도 마찬가지)`

```js
const user = {
    name: "Jin"
};
const member = user;
member.name = "Ho";
console.log(user);  //Ho
console.log(member);//Ho
```

### 깊은복사
`깊은복사_배열`
```js
const a1 = [1,2,3];
const a2 = new Array(a1.length);
// 배열을 온전히 값복사하기 위해서는 원소끼리 개별복사 해야한다.
for(let i =0; i<a1.length; i++){
    a2[i] = a1[i];
}

//배열객체가 갖는 메서드를 할용한 깊은 복사 방법
// --> const 새변수 = 원본배열.slice();

const a3 = a1.slice();
console.log(a1);  //[1,2,3]
console.log(a2);  //[1,2,3]
console.log(a3);  //[1,2,3]

a1[0] += 100;   // a1배열만 첫번째 값이 101이 된다.
```
  
`깊은복사_JSON`
```js
const addr = {
    city: "seoul",
    gu: "서초"
};
const copy = {};
for(let key in addr){
    copy[key] = addr[key]; //copy.city와 copy.gu 동일한 처리
}
console.log(addr); //{ city: 'seoul', gu: '서초' }
console.log(copy); //{ city: 'seoul', gu: '서초' }

addr.gu = "강남";   // addr JSON만 gu가 강남으로 바뀜.
```
`JS가 제공하는 기능 활용`
```js
const copy2 = {};

Object.assign(copy2, addr); 
// addr을 copy2에 깊은 복사 수행하는 JS내장기능으로 복사될 copy2가 비어 있는 
// json일 경우 복사한다. copy2가 비어있지 않으면 누적된다.
console.log(copy2); // addr값이 나온다.
```

## 구조분해
`JSON`
```js
const object = {a:1, b:2, c:3};
//각 element를 변수로 추출 : const x = object.a; -> x=1
const {a,b,c} = object;
//JSON or 객체 에서 필요한 값만 추출하여 새로운 상수로 만들어준다.(명시된 항목과 동일한 key를 갖는 데이터가 존재해야 함.)
// a =1, b =2, c =3

const data = {name:"hello", age:15,}; // 필요한 데이터만 추출
const {name} = data;
console.log(name); // hello 출력

const {name: user, age: old} = data;// data안에 있는 name, age를 분해하며 이름을 변경
console.log(user); // hello
console.log(old); // 15

//구조분해를 수행한 나머지를 별도로 분리하기
// rest_b라는 것은 단순한 변수 이름이므로 어떤 단어를 사용해도 무관
const dummy = {a1: 100, a2:200, a3:300, a4:400};
const {a1,a2, ...rest_b} = dummy;
console.log(a1); //100
console.log(a2); //200
console.log(rest_b); // {a3:300, a4:400}

//구조분해를 활용하여 기존 데이터와 추가적인 값을 병합한 새로운 객체 생성
// ... 뒤에 오는 변수명은 반드시 원본 객체 이름이어야 한다.
const origin = {name: "JS", age:15};
const newdata = {...origin, gender: "M"};
console.log(newdata); // {name: "JS", age:15, gender: "M"}

//구조분해를 통한 새로운 데이터 생성이 원본과 동일한 이름의 속성이 있다면 원본 데이터를 수정.
const newdata2 = {...origin, age:30, gender: "F"};
console.log(newdata2);// { name: 'JS', age: 30, gender: 'F' }
```
  
`배열`
```js
const arr = [100, 200];
const [x,y] = arr; // 순서대로 원소분리, 변수이름은 정할 수 있다. x=100,y=200

```