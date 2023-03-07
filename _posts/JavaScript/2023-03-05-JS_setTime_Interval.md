---
title:  "JS_setTimeout , setInterval"
excerpt: "setTimeout , setInterval 정리"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-03-05
---

# setTimeout
일정 시간이 지난 후 함수를 실행합니다.  
```js
function showName(name){
    console.log(name);
}
setTimeout(showName, 3000, "JinHo");
// 함수, 시간, 인수
```
delay를 0으로 설정하면 현재 실행중인 스크립트가 종료된 이후 스케줄링 함수를 실행하기 떄문에 0이라고 적어도 바로 실행되는것이 아닙니다.  
```js
setTimeout(()=>{
    console.log(2);
},0);
console.log(1);
// 출력결과
// 1
// 2
```
그리고 브라우저는 기본적으로 4ms정도의 대기시간이 있습니다.  


# clearTimeout();
setTimeout은 timeId를 반환합니다. 이것을 이용하여 스케줄링을 취소할 수 있습니다.
```js
const tId = setTimeout(()=>{
    console.log("hello~");
}, 3000);
clearTimeout(tId);
 // 3초가 지나기전 clearTimeout이 실행되기 때문에 아무일도 일어나지 않는다.
```

# setInterval
일정 시간 간격으로 함수를 **반복** 실행합니다.    
setTimeout과 사용법이 동일하며 계속 반복 수행합니다.
```js
function showName(name){
    console.log(name);
}
const tId = serInterval(showName, 3000, "jinho");
// 3초마다 jinho 콘솔에 출력
```

# clearInterval
clearTimeout과 사용법이 같습니다.  

