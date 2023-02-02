---
title:  "JS_HTML Fragments"
excerpt: "HTML 을 JS에서 결합하여 나타내기"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-02
---
``백틱을 사용하면 이전에 js에서 html을 생성할때 쓰던 코드들을 축약하여 쓸 수 있다.  
예제
```js
const wrapper = document.querSelector("#wrapper");

const peoples = ["name1", "name2", "name3", "name4"];

const list = `
    <h1>Hello Peoples!</h1>
    <ul>
        ${people.map(people=>`<li>${people}</li>`).join()};
    </ul>
`
wrapper.innerHTML = list;
```


