---
title:  "JS Destructuring"
excerpt: "JS Destructuring 관련 예제"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-02
---

# Function Destructuring

```js
function saveSetting({notifications, color:{theme}}){
    return console.log();
};

saveSetting({
    notifications:{
        follow:true,
        alert:true,
        mkt:false,
    },
    color:{
        theme:"blue",
    }
});
```

Object 구조분해와 마찬가지로 default Value도 설정해줄수 있고, 이름을 다르게 받아올수도 있다.  

# Swapping and Skipping 

## Swapping
```js
let mon = "Sat";
let sat = "mon";

[sat, mon]= [mon, sat];
```
처음에는 바꾸고싶은 변수들을 가지고 array를 만들고 그 다음 변수들을 오픈해서 각 변수가 담고있는 내용을 업데이트 해준다.  

## Skipping
```js
const days = ["mon", "tuw", "wen", "thu", "fri"];

// 변수를 생략하여 4,5번째값 가져오기.
const [, , , thu, fri] = days;
```


