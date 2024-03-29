---
title:  "JS handling "
excerpt: "JavaScript 기본정보"
toc : true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2022-07-31
---
- 자바스립트는 기본적으로 모든 브라우저에 내장되어 있으므로 따로설치를 안해도 된다.  
- 일반적으로 JS는 주로 끝에서 가져와준다.  
- JS는 프로그래밍언어라서 숫자와 문자를 사용할 수 있다.  
- JS는 위부터 아래로 실행된다.  
  
  
```html
JS를 HTML에 가져오는 방법
<script src="JSfileName.js"></script>
```

## Variables
const : constant(상수)라는 걸 의미하며 바뀌지 않는 값이라는 뜻이다.(계속 유지되는 것) 기본적으로 많이 사용한다.  

```js
일반적인 작성방법
const varialbeName ;  //값
const a = 5; //number
const b = "2"; //text

// * variable은 공백이 존재하면 안된다.
const my Name --> X
const myName  --> O

//문자사이 공백이 필요한경우 JS에서는 대문자로 구분해준다.
//위와 같은방법을 camelCase 라고 부른다.
```

- let : 위의 const 와는 다르게 값을 바꾸는 게 필요하거나 variable 값을 업데이트 해야할떄 사용한다.  
- var : 원하면 어디서든 업데이트 할 수 있으며 언어를 통한 보호를 받지 못하기 떄문에 현재는 쓰지 않는다.  

## Booleans
true(참) or false(거짓) 값을 정한다.   
```js
const uo = true;
const io = flase;
String 이 아니다.
const up = "true"; -> 이건 String 이다.
```
- JS의 데이터 타입 중에는 '존재하지 않음', '정의되지 않음', '아무것도 없음' 을 뜻하는 것들이 있다.  
- null : 아무것도 없음 을 뜻한다.(null은 그 변수에 아무것도 없다는 걸 뜻한다.)
- null 은 자연적으로 발생하지 않으며 variable 안에 어떤 것이 없다는 것을 확실히 하기 위해 쓰는것이다.(의도적으로 비어있다는 것을 표현하는것이다.)
- undefined : variable을 만들고 있지만, 값을 주고 있지 않을때 보여준다.   
```js
let something;
console.log(something);
콘솔창에서 확인해보면 보여진다.
```
## Arrays
배열을 만드는 방법은 아래와같다.   
```js
const mon = "mon";
const tue = "tue";
const wed = "wed";
const thu = "thu";
const fri = "fri";
const daysOfWeek = [mon, tue, wed, thu, fri]; 
const nonsense = [1, 2, "hello", false, null, true, undefined];
배열은 0부터 시작해서 첫번째 값을 얻으려면 아래처럼 사용해주면 된다.
console.log(daysOfWeek[0]);
순서대로 0,1,2,3,4
배열안에 값을 추가해주려면 아래처럼 써주면된다.
daysOfWeek.push("sat"); ->5번에 위치하게 되며 배열항목은 6개가된다.
```
## Objects
- JavaScript에서 객체는 속성과 타입을 가진 독립적인 개체다. 현실의 컵과 비교해본다면 색, 디자인, 무게, 소재 등의 속성을 가진 객체라고 할 수 있다.
- 객체는 자신과 연관된 속성들을 가진다. 객체의 속성은 객체에 붙은 변수라고 설명할 수 있다. 객체의 속성은 일반적인 JavaScript 변수와 똑같은데, 다만 객체에 붙어있다는 점만 다르다. 속성에 접근할 땐 간단한 마침표 표기법을 사용한다.   
```
objectName.propertyName
```
- [Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)  
- [객체로 작업하기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects)  
- [JS 객체 기본](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Basics)  
  
간단한 예제를 만들어 보았다.   
```js
const player = {
    name: "Jinho";
    points: 10;
    fat:true;
};
console.log(player.name); -> 실행시 Jinho 값이 나온다.
console.log() 또한 console은 object라는 뜻이고,
그 안의 어딘가에 log라는 것이 있다는 뜻이다.
console.log(player.name); 
player.name = "hoJin";
console.log(player.name);
실행시
첫번째는 Jinho, 두번째는 hoJin 이라는 수정한 값이 나오게된다.
const 값은 수정할수 없다고 했는데 위에서는 object 안의 무언가를 수정하기 때문에 
오류가 발생하지 않는다. 다만 player 라는 const object 를 하나의 값으로서 업데이트
하려고 할때 오류가 발생한다.
```
## Functions
- functions은 계속 반복해서 사용할 수 있는 코드 조각이라 생각하면 편하다.  
- function은 어떤 코드를 캡슐화해서, 실행을 여러 번 할 수 있게해준다.  
```js
// 아래 함수는 sayHello로 보내진 첫번쨰 데이터가 nameOfPerson
// 두번쨰 데이터가 age 라는 variable로 가게 된다는 것을 알게된다.
function sayHello (nameOfPerson, age){
    console.log(nameOfPerson, age);
    //nameOfPerson, age는 이 블럭 안에서만 존재한다.
}
sayHello("jin",10);
sayHello("ho",20);
```
- object와 function 같이 쓰는 법   

```js
const player ={
    name: "jinho",
    // object안에서 만드는 함수는 위의 방법과 조금 다르다.
    sayHello : function(otherPersonName){
        console.log("hello "+ otherPersonName);
    }
};
console.log(player.name);
// jinho 출력
player.sayHello("hoho");
// hello hoho 출력

```

## Returns
보통 값을 얻기위해 사용하며 아래 예제와 함께 살펴보자.  
아래의 코드는 retrun을 사용하지 않은 코드이다.  


```javascript
const calculator = {
    plus : function(a,b){
        alert(a + b);
    }
}
console.log(calculator.plus(2,3));
//실행 결과는 alert(경고창)에서 값은 5라고 뜨지만
//콘솔에 나온 결과는 undefined 가 나오게 된다.
//위의 function은 실제 결과 값을 얻지 못한다.
```

return을 이용하여 값을 얻는 함수를 작성해 보았다.  


```js
const age = 96

fuction calculateKrAge(ageOfForeigner){
    return ageOfForeigner + 2;
}
const krAge = calculateKrAge(age);
console.log(krAge);
//콘솔에 나타나는 값은 98이 된다.
```

위와다르게 return 값을 String으로 정해주는 코드  


```js
const age = 96

fuction calculateKrAge(ageOfForeigner){
    ageOfForeigner + 2;
    return "hello";
}
const krAge = calculateKrAge(age);
console.log(krAge);
//콘솔에 나타나는 값은 hello라는 Sting이 된다.
//function의 반환 값과 같은 variable을 consoe.log하고 있다.
```

## Conditionals 

- parseInt() : 값이 숫자인지 판별하는 코드. 숫자가 아닐경우 NaN 출력.  
- OR , AND 사용법
- [조건문 사용법](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Building_blocks/conditionals)

   

```js
OR 을 쓸떄
두조건중 하나만 참이여도 true
true || true = true
true || false = true
false || false = false

AND 를 쓸떄
두조건중 둘다 맞아야 true
true && true = true 
true && false = false
false && false = false
```

- if, else 사용법 예시  


```js
const age = parseInt(prompt("How old are you?"));
// How old are you 라는 경고창을 띄우면서 값을 받아온다.
// 값을 받아오면 밖의 parseInt가 숫자인지 판별한다.

if(isNaN(age) || age<0 ){
    console.log("please write a real positive number");
    //숫자인지 아닌지 음수가 아닌지 판별
}else if(age < 18){
    console.log("young");
}else if(age >= 18 && age <= 50){
    console.log("drink");
}

//if else 를 쓸떄 조건문을 추가하려면 else if 로 추가가능.
```

## Document Object

- JS를 사용하는 이유는, HTML 과 상호작용 하기 위해서이다.  
- 그 말인 즉슨, HTML의 Element들을 JacaScript를 통해 변경하고, 읽을 수 있다는거다.  
- console 창에서 document를 치면보여준다.(정의되어 있음)  
- document는 브라우저에 이미 존재하는 object이다.  
- console.dir(documnet) 를 쳐보면 object라는 것을 알수있다.  

![document_console](https://user-images.githubusercontent.com/105098581/182154854-5b43147d-a2b1-4231-8e2f-b2fbbd61fe65.jpg){: .align-center}

  
- console 창에 document.title 입력시 title을 가져올 수 있다.
- 위에서 보듯, JS는 HTML에 접근하고 읽을 수 있게 설정이 되어있다.  
- JS는 HTML을 읽어올 뿐만 아니라, HTML을 변경할 수도 있다.  

## HTML in Javascript 


```js

const title  = document.getElementById("title");
//document 함수중 getElementById 를 이용해 HTML에서 id를 통해 
//element를 찾아준다.
title.innerText = "hi";
//title 의 text를 바꿔줄수도 있다.
console.log(title.id);
//title의 id 를 가져올수 있다.
console.log(title.className);
//title의 className 를 가져올수 있다.
```

## Searching For Elements

  

```js
const hellos = document.getElementsByClassName("hello");
//class element를 가져온다.
const title = document.querySelector(".hello h1");
// element를 CSS 방식으로 검색할 수 있다.
// hello란 class 내부에 있는 h1을 가지고 올 수 있다는 것을 의미한다.
// 만약 class가 여러개 일경우 첫번째 element만 가져온다.
const title = document.querySelectorAll(".hello h1");
//위와 다르게 hello class 안에 있는 h1 을 array로 모두 가져온다.
const title = document.querySelector(".hello h1:first-child");
// css 방식으로 검색하기때문에 위와같이 사용할 수 도있다.
const title = document.querySelector("#hello");
// id 를 찾는방법.
```

- getElementById, getElementsByClassName 은 css 방식이 아니라서 위처럼 검색할 수 없다.  

## Events  

- console.dir를 사용할시 기본적으로 object로 표시한 element를 보여준다.   
  

```js
const title = document.querySelector("div.hello:first-child h1");

console.dir(title);
//title 내부 object목록들을 표시해준다.
//목록에는 style 도 있는데 JS에서 style을 변경할 수 있다.
title.style.color = "blue";
//title의 색상을 blue로 바꿔준다.

```

- HTML element를 JS로 가지고 오는 방법을 알았으니 event를 listen 하는 방법도 알 수 있다.  
- eventListner는 말그대로 event를 listen 하는걸 뜻한다.  


```js
const title = document.querySelector("div.hello:first-child h1");

fuction handleTitleClick(){
    console.log("title was click!");
}

title.addEventListener("click", handleTitleClick);
//click event에 대해서 listen하고 싶은 것이다.
// HTML_element.addEventListener("listen하고싶은 event",함수);
//clcik할떄 위의함수 실행. 주의점은 '함수()' 이렇게 쓰면 안된다.
//클릭시에 함수가 실행 되어야 함.
```
  
[HTMLHeadingElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHeadingElement)
   
[HTMLElement_properties,event 목록](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement)
  
활용 예시    


```js
const title = document.querySelector("div.hello:first-child h1");
//title element가져오기
fuction handleTitleClick(){
    title.style.color = "skyblue";
}

fuction handleMouseEnter(){
    title.innerText = "Mouse is here!";
}
fuction handleMouseLeave(){
    title.innerText = "Mouse is gone";
}

title.addEventListener("click", handleTitleClick);
//title.onclick = handleTitleClick 위와 똑같이 작동
title.addEventListener("mouseenter", handleMouseEnter);
//title.onmouseenter = handleMouseEnter 
title.addEventListener("mouseleave", handleMouseLeave);
// addEventListener를 사용하는 이유는 removeEventListener를 통해서
// event listener를 제거할 수 있기 때문.

//  window 활용

fuction handleWindowResize(){
    document.body.style.backgroundcolor = "skyblue";
    //document body는 위처럼 가져올 수 있지만(body, head, title)
    //div, span 등은 위처럼 가져올수 없고 따로 지정해서 가져와야한다.

}

window.addEventListener("resize", handleWindowResize)
//window element의 resize event를 listen하여 함수실행
```

## CSS in JavaScript  
JS에서 h1 active class 에게 전달방법
```css
.active{
    color : tomato;
}
```
- div.hello:first-child h1 위치  

```html
<body>
<div class="hello">
<h1>Click me!</h1>
</div>
</body>
```
    

```js
const h1 = document.querySelector("div.hello:first-child h1");
function handleTitleClick(){
    h1.className = "active";
    // css파일에 있는 active class 지정
}

h1.addEventLitsener("clcik", handleTitleClick);
```
- JS는 HTML을 변경, CSS는 HTML을 바라 보고있다.  

- else if 활용 글자색 변경함수
  
```js
const h1 = document.querySelector("div.hello:first-child h1");

function handleTitleClick(){
    if(h1.className === "active"){
        h1.className = "";
    }else{
        h1.className = "active";
    }
}

h1.addEventLitsener("clcik", handleTitleClick);

```

- 위에서 active가 두번 쓰이기 떄문에 error 발생을 최소화 하기위해 아래와 같이 작성을 권장한다.  

```js
const h1 = document.querySelector("div.hello:first-child h1");
const activeEvent = "active";
function handleTitleClick(){
    if(h1.className === activeEvent){
        h1.className = "";
    }else{
        h1.className = activeEvent;
    }
}

h1.addEventLitsener("clcik", handleTitleClick);

```

- 만약 class가 두개 이상일경우 classList를 활용해서 위코드를 수정할 수 있다.  
- [classList](https://developer.mozilla.org/ko/docs/Web/API/Element/classList)
  
```js
const h1 = document.querySelector("div.hello:first-child h1");
const activeEvent = "active";
function handleTitleClick(){
    if(h1.classList.contains(activeEvent)){
        //위 코드는(classList)HTML element가 가지고 있는 하나의 요소를 사용한다.
        //'element의 class 내용물을 조작하는 것을 허용한다.' 라고 설명됨.
        //contains : 이 function은 우리가 명시한 class가
        //HTML element의 class에 포함되어 있는지 말해준다.
        h1.classList.remove(acriveEvent);
        //클래스 이름 제거
    }else{
        h1.classList.add(acriveEvent);
        //클래스 이름 추가
    }
}

h1.addEventLitsener("clcik", handleTitleClick);
```

- 위의 검증과정을 하나의 코드로 축약한것이 toogle이라는 것이다.  
  
```javascript
const h1 = document.querySelector("div.hello:first-child h1");

function handleTitleClick(){
    h1.classList.toggle("active");
    //toggle은 h1의 classList에 active class가 이미 있는지 확인해서
    //만약 있다면, toggle이 active를 제거해준다.
    //만약 classList에 active가 추가되어 있지 않다면 toggle은 active를 
    //classList에 추가해준다.
}
h1.addEventListener("clcik",handleTitleClick);
```

