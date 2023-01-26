---
title: "CSS [FLEXBOX]"
execrpt: "FLEXBOX"
toc: true
toc_sticky: true
categories:
  - CSS
tags:
  - CSS
last_modified_at: 2022-08-17
---
## FLEXBOX

- [flexbox_froggy](https://flexboxfroggy.com/#ko)
- flexbox에서는 children과 얘기하지 않는다.
- flexbox에서 뭔가를 움직이고 싶을 때는 `flexbox container`를 만들어야 한다.
- 부모(father)가 자식(children)의 위치를 움직일 수 있다.
- row(행) : row는 가로를 의미한다.
- flex container의 `flex-direction` 기본값은 row이다.
- `justify-content`는 수평축에 있는 flex children의 위치를 변경한다.
  
- `justify-conten`t`는 main axis 방향으로 item을 옮긴다.
- `align-item`은 cross axis 방향으로 item을 움직인다.

  
<div class="notice--primary" markdown="1">

`기본값`

```
flex-direction(방향) : row  (가로,기본값)
justify-content : main axis  (가로축)
align-items : cross axis  (세로축)
```
`column설정`

```
flex-direction : column (세로)
justify-content : main axis  (세로축)
align-items : cross axis  (가로축)
위의 justify-content, align-items 가 반대로 된다.
```

직접 만들면서 하는편이 더 이해가 빠르다..
</div>


### align-self & order

- `align-self`는 align-item과 비슷한 일을 하는데 이 말은 cross axis다. 
- 한 box에만 해당한다. 
- children에게 그렇게 줄 수 있다.
- align-item은 cross axis 방향에 있는 item의 위치를 바꾼다.


다른방법으로, 오직 child에게 줄 수 있는 또 다른 property는 `order`이다.
- box에게 순서를 변경하라고 할 수 있다.
- order의 기본값은 0이다.

*ex*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="zYWyweM" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/zYWyweM">
  align-self</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


### wrap, nowrap, reverse, align-content

- flexbox는 오직 같은 줄에 있도록 만드는데만 신경쓴다.(width의 크기를 깨트려서라도)
- `flex-wrap : nowrap` 무슨 짓을 하더라도 이 element들은 같은 줄에 있어야 한다고 flexbox에 얘기하는 것이다.(기본값)
- `flex-wrap : wrap` flexbox에게 child의 크기를 유지하라고 이야기 한다.

*reverse 예제*

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="YzadQZW" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/YzadQZW">
  reverse</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

- `align-content` 는 line을 변경한다. 그리고 line은 cross axis에 있다.
- 쉽게 말하면 다음줄로 넘어가는 박스의 간격들을 조절한다.

*align-content 예제*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="ZExVyKw" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/ZExVyKw">
  align-content</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


### flex-grow, flex-shrink

- flex-grow, flex-shrink는 child에게 줄 수 있는 property다.
- flex-shrink는 기본적으로 element의 행돌을 정의한다.(flexbox가 찌그러질때 어떤 box가 찌그러질지 정의할 수 있다.)
- flex-shrink는 기본값이 1이다. 
  
- flex-grow는 flex-shrink와 같지만 반대로 작용한다.
- flex-grow는 box주변의 공간을 가진다.
- flex-grow의 기본값은 0 이다.

*flex-shrink예제*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="yLKGXxw" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/yLKGXxw">
  flex-grow&amp;flex-shrink</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

*flex-grow예제*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="JjLwJQR" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/JjLwJQR">
  flex-grow</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### flex-basis

- flex-basis는 child에서 적용되는 property이다.
- flex-basis는 element에게 처음 크기를 준다.(찌그러지거나 늘어나기 전)
- flex-basis는 main axis에서 작용하니까 width라고 생각해도 무방하다.
- 만약 높이를 조절하려면 flex-direction 값을 column으로 주면 된다.