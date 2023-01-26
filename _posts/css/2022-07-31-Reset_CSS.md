---
title:  "Reset_CSS"
excerpt: "CSS초기화 방법"
header:
    teaser : /assets/images/CSS3_logo_and_wordmark.jpg
categories:
  - HTML&CSS
tags:
  - CSS
last_modified_at: 2022-07-31
---
웹 브라우저 마다 default 값으로 설정되어 있는 스타일을 없애주고 완전 초기상태에서 디자인 할 수 있도록 도와주는 코드이다.  
stylesheet css에 import 하여 사용해주면 된다.  
```
/* http://meyerweb.com/eric/tools/css/reset/ 
   v2.0 | 20110126
   License: none (public domain)
*/

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
```
  
출처 : [Reset_CSS](https://meyerweb.com/eric/tools/css/reset/)