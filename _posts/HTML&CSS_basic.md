---
title:  "HTML&CSS basic"
excerpt: "HTML,CSS 기본정보"

categories:
  - HTML&CSS
tags:
  - HTML
  - CSS
last_modified_at: 2022-06-30
---
##기본 정보

웹사이트를 만드는 데 사용할 수 있는 언어는 HTML, CSS, JS 로 구성된다.

HTML을 사용하는 이유
↳ To describe the content of our website to the browser

CSS을 쓰는 이유
↳ To tell the browser how the content of our website looks like

JS를 쓰는 이유
↳ To make our website interactive

브라우저는 CSS를 위에서 아래로 읽기 때문에 입력순서가 중요하다.

HTML은  Markup Language

CSS는 Design languages

JS는 Programming language 

##html 기본 골격

```html
<!DOCTYPE html>
문서타입이 html 이란것을 알려준다

<html lang="kr">
구글,네이버,bing 같은 검색엔진들에 도움을 주기 위해 쓴다
우리 사이트에서 사용되는 언어가 무엇인지 말해준다
주된 언어가 한국어인지 영어인지 검색엔진에게 알려준다
<head>
    <meta charset="UTF-8">
    브라우저에게 text를 어떻게 그려달라는지 말해준다  
    한글이나 다른 특수문자가 있는 언어를 압력 할 떄 브라우저가 이해 못할떄가 있는데 이 태그가 없으면 글자가 꺠져보인다.

    <title>Home</title>
    홈페이지 이름 
    <meta name="description" content="This is my website." />
    description[설명]태그는 구글이검색할떄 찾는 태그이다.
    [title도 구글이 검색에 중요한 비중을 가지고 있다.]
    meta : 부가적인 정보를 뜻하고 2가지 attribute(속성)을 갖고있다. 
    -[content, name]
    meta tag들은 self-closig tag이다.
    <meta property="og:title" content="Nomad Coders">
    og코드는 카카오톡 대화방에 링크를보내면 웹사이트 정보가 보여지게되도록 만든다
    불러올떄 순서는 og:title -> description -> og:image
    사이트의 부가적인 정보들은 head 태그에 작성해준다
</head>

ex) h1 { color:blue } 에서
  selector(선택자): h1
  property(속성): color
  value(값): blue
```
더많은 html 사용법들은
<https://developer.mozilla.org/ko/docs/Web/HTML/Element>