---
title:  "JS_styled Components 만들어보기"
excerpt: "JS를 이용해 styled Components clone"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-02
---

```js
const styled = (css) => console.log(css);

styled`
    border-radius:10px;
    color:blue;
`;
// styled("css코드") 한것과 위의 결과는 같다.
```
![](https://user-images.githubusercontent.com/105098581/216261271-a80ed524-de74-4615-a707-305247604260.png)

위의 함수는 string인 하나의 argument가 나온다.  

위의 특성을 활용하여 아래의 예제를 만들어 보았다.  


<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="BaPvBKL" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/BaPvBKL">
  Untitled</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>