---
title: "JS Promises"
execrpt: "js promises 예제 & 설명"
toc: true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-03
---

Promise 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냅니다.  
[MDN-promises](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)  

Promise는 다음 중 하나의 상태를 가집니다.  

- 대기(pending): 이행하지도, 거부하지도 않은 초기 상태.
- 이행(fulfilled): 연산이 성공적으로 완료됨.
- 거부(rejected): 연산이 실패함.


# Create Promises

수행한 비동기 작업이 성공한 경우 resolve(...)를 호출하고, 실패한 경우 reject(...)를 호출합니다.  
Promise의 핵심은 아직 모르는 value와 함께작업할 수 있게 해준다는 것입니다.  
```js
const myPromises = new Promise((resolve, rejecte) => {
    setTimeout(resolve, 3000, "hello~~");
});

setInterval(console.log, 1000, myPromises);
```
![](https://user-images.githubusercontent.com/105098581/216502252-ccdf7e1e-c4e2-4f21-8efb-ecf99e2d3aac.png)

# Using Promises

값을 성공적으로 불러올 경우의 예제
```js
const myPromises = new Promise((resolve, rejecte) => {
    resolve("Good!!");
});
const thenFn = value => console.log(value);
myPromises.then(thenFn); // Good!! 출력
```

실패할 경우의 예제
```js
const myPromises = new Promise((resolve, rejecte) => {
    setTimeout(rejecte, 3000, "fail ㅠㅠ");
});
const thenFn = value => console.log(value);
myPromises.then(thenFn); // fail ㅠㅠ 출력
```
![](https://user-images.githubusercontent.com/105098581/216502932-d43058fc-a401-4daf-94b8-bf3e77e855f6.png)

catch를 써서 error를 잡는 promise 예제
```js
const myPromises = new Promise((resolve, rejecte) => {
    setTimeout(rejecte, 3000, "fail ㅠㅠ");
});
const thenFn = value => console.log(value);
myPromises.then(thenFn).catch(value => console.log(value)); // fail ㅠㅠ 출력
```
![](https://user-images.githubusercontent.com/105098581/216503411-1452064e-3bbf-4e09-99cc-1fbb748f4935.png)

- then이 실행되면 catch는 절대 실행되지 않는다.
- 반대로 catch가 실행되면 then은 절대 실행되지 않는다.

# Chaining Promises

promise들을 엮고 싶을 때는 기존의 then에서 **return 값이 있어야** 정상적으로 다음 then에게 값이 전달됩니다.  

```js
const myPromises = new Promise((resolve, rejecte) => {
    resolve(2);
});

myPromises
    .then(number => {
    console.log(number * 2);
    return number * 2;
    })
    .then(otherNumber => {
    console.log(otherNumber * 2);
    });
// 4 -> 8 
```

# promise.all

[MDN-Promise.all](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)  

Promise.all() 메서드는 순회 가능한 객체에 주어진 모든 프로미스가 이행한 후, 혹은 프로미스가 주어지지 않았을 때 이행하는 Promise를 반환합니다. 주어진 프로미스 중 하나가 거부하는 경우, 첫 번째로 거절한 프로미스의 이유를 사용해 자신도 거부합니다.  

```js
const p1 = new Promise(resolve => setTimeout(resolve, 3000, "First"));
const p2 = new Promise(resolve => setTimeout(resolve, 1000, "Second"));
const p3 = new Promise(resolve => setTimeout(resolve, 5000, "Third"));

const motherPromise = Promise.all([p1,p2,p3]);

motherPromise.then(values => console.log(values));
// ["First", "Second", "Third"] 
```

Promise.all 이 다른 promise들이 전부 진행 될 때까지 시간이 얼마나 걸리던 전부 끝났을 떄 값을 순서대로 제공하는 것이다.  
만약 중간에 하나라도 error가 발생하면 reject를 호출하며 moterPromise도 reject 된다. (다른 promise들도 같이 reject 된다.)

# promise.race()
[MDN-Promise.race()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)  
Promise.race() 메소드는 Promise 객체를 반환합니다. 이 프로미스 객체는 iterable 안에 있는 프로미스 중에 가장 먼저 완료된 것의 결과값으로 그대로 이행하거나 거부합니다. (하나라도 resolve되거나 reject 되면 된다.)

```js
const p1 = new Promise(resolve => setTimeout(resolve, 3000, "First"));
const p2 = new Promise(resolve => setTimeout(resolve, 1000, "Second"));
const p3 = new Promise(resolve => setTimeout(resolve, 5000, "Third"));

const motherPromise = Promise.race([p1,p2,p3]);

motherPromise.then(values => console.log(values));
// "Second"
``` 

# finally

[MDN-finally](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)  

finally() 메소드는 Promise 객체를 반환합니다. Promise가 처리되면 충족되거나 거부되는지 여부에 관계없이 지정된 콜백 함수가 실행됩니다. 이것은 Promise가 성공적으로 수행 되었는지 거절되었는지에 관계없이 Promise가 처리 된 후에 코드가 무조건 한 번은 실행되는 것을 제공합니다.  

```js
const p = new Promise((resolve,reject) => {
    setTimeout(resolve, 3000, "Done~");
})
  .then(value => console.log(value))
  .catch(value => console.log(value))
  .finally(() => console.log("I'm finally !!"));
// Done ~
// I'm finally !!
```

# fetch use Promise
fetch는 간단히 말하면 뭔가를 가져오는 것입니다.  
[MDN-fetch](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)  

```js
fetch("https://yts.mx/api/v2/list_movies.json")
    .then(response => {
        console.log(response);
        return response.json();
    })
    .then(json => console.log(json))
    .catch(error => console.log(error));
```

response (응답)
![](https://user-images.githubusercontent.com/105098581/216508233-0d13540e-0d76-4238-94bc-1b56ff5c77ff.png)

response.json() : 응답처리된 data를 json으로 변환해준다.
![](https://user-images.githubusercontent.com/105098581/216508408-850f1d5a-37dd-4d17-a23a-ebe5ab495895.png)







