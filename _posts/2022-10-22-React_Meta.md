---
title: "Meta Tag"
execrpt: "Meta 설명"
toc: true
toc_sticky: true
categories:
  - React
tags:
  - React
last_modified_at: 2022-10-22
---
## 검색엔진 최적화 (SEO : Search Engine Optimization)
검색엔진이 이해하기 쉽도록 홈페이지의 구조와 페이지를 개발해 검색 결과 상위에 노출될 수 있도록 하는 작업.  
기본적인 작업 방식은 특정 검색어를 웹 페이지에 적절하게 배치하고 다른 웹 페이지에서 링크가 많이 연결되도록 하는 것이다.  
```html
<title>사이트 제목</title>
<!-- 검색엔진용 -->
<meta name="description" content="사이트 설명(최대 160자 추천)" />
<meta name="keywords" content="사이트 키워드" />
<meta name="author" content="사이트 저자" />
<meta name="subject" content="사이트 목적(주제)" />
<meta name="copyright" content="사이트 저작권(소유권)" />
<meta name="content-language" content="ko" />

<!-- SNS용 -->
<meta property="og:url" content="사이트 주소(URL)" />
<meta property="og:title" content="사이트 제목" />
<meta property="og:type" content="website" />
<meta property="og:description" content="사이트 설명(최대 160자 추천)" />
<meta property="og:image" content="사이트 대표 이미지 URL" />

<link rel="icon" href="데스크탑 favicon" type="image/png" />
<link rel="shortcut icon" href="안드로이드용 favicon" type="image/png" />
<link rel="apple-touch-icon" href="아이폰용 favicon" type="image/png" />
```

