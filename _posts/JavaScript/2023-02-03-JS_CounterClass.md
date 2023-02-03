---
title:  "JS Class로 Counter 증감 구현"
excerpt: "JS class 연습"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-03
---

주의사항으로는 class에 일반함수를 사용할시 this가 button을 가리켜서 증감이 되지않는다.  
아래의 예제에서는 arrow function으로 this가 class를 가리키도록 만들었다.  

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="PoBXLOE" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/PoBXLOE">
  Untitled</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>