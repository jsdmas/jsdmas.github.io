---
title:  "JS Class_Static 예제"
excerpt: "Static 예제"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-03
---

참고로 Private field 설정시 constructor 바깥에 생성 해야한다.

```js
class User {
    #name;
    static count = 0;
    constructor(name) {
        this.#name = name;
    }

    get name() {
        return this.#name;
    }
    set name(value) {
        this.#name = value;
    }

    in() {
        User.count++;
    }
    out() {
        User.count--;
    }
    showState() {
        console.log(`name:${this.#name}, cnt:${User.count}`);
    }
}

console.group("손님1");
const c1 = new User("손님1");
c1.in();
c1.showState();
console.groupEnd();

console.group("손님2");
const c2 = new User("손님2");
c2.in();
c1.showState();
c2.showState();
console.log(User.count); // static
console.groupEnd();

console.group("손님3");
const c3 = new User("손님3");
c3.in();
c1.showState();
c2.showState();
c3.showState();
console.groupEnd();

console.group("손님2,3 out");
c3.out();
c2.out();
c1.showState();
c2.showState();
c3.showState();
console.groupEnd();
```

![](https://user-images.githubusercontent.com/105098581/216551908-53b73edb-d440-4500-b65e-b6db0ef96d47.png)