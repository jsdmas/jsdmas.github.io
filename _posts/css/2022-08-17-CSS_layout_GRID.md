---
title: "CSS [GRID]"
execrpt: "GRID"
toc: true
toc_sticky: true
categories:
  - CSS
tags:
  - CSS
last_modified_at: 2022-08-18
---

## GRID

* grid : 격자무늬
* GRID가 필요한 이유 : box들을 옆으로 두거나 가운데로 옮기거나 나누는 등은 쉬운데 grid 형태를 만드는 건 어려워서
* grid의 규칙은 flexbox와 비슷하다
* grid design은 주로 father에서 한다.
* flex 는 1차원(오직column만 갖고 있고 row가 없다.) 이고 grid는 2차원 layout이다.
* [GRID_개념](https://developer.mozilla.org/ko/docs/Web/CSS/grid)

*grid 기본예제*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="jOzXGbM" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/jOzXGbM">
  grid_basic</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


### Gird Template Areas

*grid template area 예제*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="poLqWQp" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/poLqWQp">
  Grid_template_Areas</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

* repeat : 반복
* 공백을 써야할 때는 `.`으로 표시한다.
* gird-area 의 이름을 선언해줘야 template이 인식하고 사용한다.(class이름과는 상관없음.)
* 참고로 `grid-template-columns : auto 200px` 사용시 모든 공간을 사용한다는 뜻이다.(화면의 모든 공간)


### Rows and Columns
위의 template area의 이론적인 부분을 예제로 만들어 봤다.  
코드를 잘 살펴보자
*ex1)*

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="rNdoYeP" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/rNdoYeP">
  Rows &amp; Columns</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
  
시작지점과 끝나는 지점의 숫자들을 잘 살펴봐야한다.  

*ex2)*

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="Barvmwm" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/Barvmwm">
  Rows &amp; Columns_2</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Grid Template

- fr의 뜻은fraction(부분)이다. pixel이나 %와 같다.(측정 단위)
- fraction은 사용 가능한 공간을 뜻한다.
- `grid container` 에서 가능한 공간을 사용한다. 
- fr은 기본적으로 가능한 만큼 공간을 차지한다.
- grid-template에서 repeat는 적용되지 않는다.

*예시*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="MWVZQem" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/MWVZQem">
  Untitled</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

  
template옆에 적는 크기는 높이(row : height) / 폭(column : width) 순서이다.

### Place Items

- justify-items의 기본값은 stretch(늘리기) 이다. 이 말은 grid-container는 모든 grid 자식을 갖고 있고 자식들을 늘여서 본인을 채우게 한다.

*예시*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="eYMbqLz" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/eYMbqLz">
  place-items</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Palce Content

- items는 셀중에 하나 (각 하나 하나) 라는 걸 의미한다.
- content는 전체 grid이다.
- `justify-content`는 모든 grid를 수평적으로 움직이고 `align-content`는 모든 grid를 수직적으로 움직인다.
- place-content는 place-itmes 와 비슷하다 `(수직(세로) , 수평(가로))`

*예시*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="wvmRVRb" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/wvmRVRb">
  Place_Content</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Place-self

- `align-self`, `justify-self` : child에만 적용되는 property이다. align & justify - items 해준거는 모든 content에 적용을 해준것이다. 이 경우는 align 그 자체만 적용된다.

*예시*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="VwXgZwG" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/VwXgZwG">
  Auto_Columns &amp; Rows</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### Auto_Columns & Rows

- `grid-auto-rows` : 만약 정해놓은 content보다 더많은 content가 있다면 내가 따로 rows를 지정해주지 않아도 default value를 자동으로 줘서 row를 생성하는 것이다.

*예시*

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="poLGzvb" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/poLGzvb">
  Auto_Columns &amp; Rows</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

- 위와같이 지정해주지 않으면 grid가 끝날 때 마다 css는 사이즈가 없는 여분의 div를 넣는다.(자동적으로, default값)
- `grid-auto-flow : column` 을 설정한다면 row의 수 보다 더 많은 content가 있을 때 마다 더 많은 column을 만든다.(flexbox direction 과 비슷하다.)

*예시*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="jOzdNWq" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/jOzdNWq">
  Auto_Columns &amp; Rows_2</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### minmax

- 얼마나 작게 혹은 크게 element가 될 수 있는지 지정할 수 있도록 해준다.
- 주로 사용할 때는 elemnet가 가능한한 엄청 크길 원하는데 동시에 엄청 작게 되진 않길 원할떄 쓸 수 있다.

*예시*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="poLGzvb" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/poLGzvb">
  Auto_Columns &amp; Rows</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### auto-fit & auto-fill

- 얼마나 element를 가질지 모르니까 사용한다.
- `auto-fill` : column을 만들어주는 일을 한다.(가능한 한 많이 우리가 준 사이즈안에서) 화면 크기를 늘려주면 공간을 준다(해당 row를 채운다 (fill) column이 있는 만큼 가능한한 많이 (column이 비어있더라도)) (정확한 사이즈)
- `auto-fit` : 딱 맞추는 것이다(fit) 현재의 element를 쭉 늘려서 row에 딱 맞게 해준다. (유동적인 사이즈)
- auto-fill은 row에다가 빈 column들로 채우는거고 auto-fit은 현재 div를 가져와서 얘네들을 다 늘린다.
- 반응형 디자인을 할때 매우 중요하다.

*예시*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="bGvzbap" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/bGvzbap">
  auto-fit &amp; auto-fill</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

### min-content & max-content

- `min-content` : 만약 box를 만든다고 하면, content가 작아질 수 있는 만큼 작아진다.
- `max-content` : 박스를 content가 필요한 만큼 크게 만든다.

*ex*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="VwXgZqJ" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/VwXgZqJ">
  min-content &amp; max-content</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

*ex2*
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="rNdPBRK" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/rNdPBRK">
  min-content &amp; max-content_2</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
