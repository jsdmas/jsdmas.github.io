---
title:  "box-sizing"
excerpt: "요소의 너비와 높이 계산방법"
header:
    teaser : /assets/images/CSS3_logo_and_wordmark.jpg
categories:
  - HTML&CSS
tags:
  - CSS
last_modified_at: 2022-07-31
---
- CSS에게 200px의 box를 원한다고 하면, CSS는 원한대로 box를 만든다.  
- 근데 그 box에 padding을 추가하게 되면 CSS는 기본적으로 내가 원했던 200px을 유지하려고 할것이다.  
- padding을 50px 줬다고 생각한다면, CSS는 box가 150px이 되었다고 생각할것이다. CSS는 이렇게 둘 수가 없어서 200px를 유지하기 위해 box를 크게 만든다.(상하좌우 padding모두 이런식으로 작동함.)
- CSS에게 box 사이즈가 작아져도 상관없다고 말할 수 있는 방법이 있다. 이 방법이 "box-sizing: border-box"이다.  
- padding을 주고 싶은데, box 사이즈는 그대로 있길 바랄때 사용한다.  
  
[Box-sizing](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing)