---
title:  "JS 브라우저 관련 기능"
excerpt: "브라우저 관련 기능 JS"
toc : true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2022-09-30
---
## Window 내장 객체
  
브라우저의 새창, 팝업 열기/닫기 기능 제공  
a.html을 새 창(새 탭)으로 열기
```js
window.open(`a.html`);
```
a.html을 팝업으로 열기
```js
// window.open(`URL`, `창이름`, `옵션`)
window.open(`a.html`, `mywin`, `width=500`, `height=300`, `scrollbars=no`, `toolbar=no`, `menubar=no`, `status=no`, `location=no`);
```
- 창이름 : 생략시 매번 새 팝업창 생성, 포함하면 한번 사용한 팝업창을 재사용함.
- 옵션 : 창 크기 관련 : width, height (창의 가로, 세로 크기를 정수로 지정), 창 모양 관련 : scrollbars,toolbar,menubar,status,location (yes / no 로 값을 지정하며 location의 경우 피싱 사이트 방지를 위해서 동작하지 않음.)
  
현재 창 닫기
```js
window.close();
// 혹은
self.close();
```

## navigator 내장객체
웹 브라우저의 정보 조회 기능.

| 기능                  | 설명                                         |
| --------------------- | -------------------------------------------- |
| navigator.appName     | 브라우저 이름                                |
| navigator.appCodeName | 브라우저 코드명                              |
| navigator.platform    | 플랫폼 정보                                  |
| navigator.appVersion  | 브라우저 버전                                |
| navigator.userAgent   | 사용자 정보(가장 포괄적인 정보를 담고 있다.) |

웹 브라우저의 이름, 버전정보, 운영체제 정보 등이 포함된 문자열 값
```js
var agent = navigator.userAgent;
```
이 값에 특정 단어가 포함되어 있는지 여부를 판단하여 브라우저나 운영체제 종류, 모바일/PC 여부 등을 확인할 수 있다.
```js
var agent = navigator.userAgent;

if(agent.indexOf("검사할단어") > -1){
    // 처리내용....
}
```

예시
<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="JjvvjmR" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/JjvvjmR">
  browser_check</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## geolocation
- 최신 노트북은 위치 정보 취득에 제한이 없기 때문에 기능 사용상의 문제가 없지만, Desktop PC의 경우 위치정보 취득을 위한 하드웨어가 없는 경우가 대부분. 이 경우 IP주소 기반으로 위치를 취득하는데 대부분 서울시청 근처로 조회될 경우가 많음.
- 구글 크롬브라우저는 상용화 된 웹 페이지에서 이 기능을 사용할 경우 https://로 시작되는 주소가 아니면 보안상의 이유로 위치정보를 반환하지 않는다.
- Javascript의 geolocation은 위도, 경도만을 반환한다. 이 정보를 네이버, 카카오, 구글등이 제공하는 지도 서비스를 통해 시각화 하면 지도 구현이 가능함. 단, 네이버 맵, 구글맵은 유료 서비스임.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="jOxxOQJ" data-user="jsdmas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jsdmas/pen/jOxxOQJ">
  geolocation</a> by jsdmas (<a href="https://codepen.io/jsdmas">@jsdmas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## location
[location_mdn](https://developer.mozilla.org/ko/docs/Web/API/Location)
location 인터페이스는 객체가 연결된 장소(URL)를 표현한다.  
>예시 주소 : http://localhost:5500/location.html?name=helloworld&age=20#info
- 문서의 URL 주소 : `location.href` -> http://localhost:5500/location.html?name=helloworld&age=20#info
- 호스트 이름과 포트 : `loacation.host` -> localhost:5500
- 호스트 컴퓨터 이름 : `location.hostname` -> localhost
- Hash값 : `loacation.hash` -> #info
- 디렉토리 이하 경로 : `location.pathname` -> /location.html
- 포트번호 부분 : `location.port` -> 5500
- 프로토콜 종류 : `location.protocol` -> http:
- URL 조회부분 : `location.search` -> ?name=helloworld&age=20를 인코딩.
  
location.search는 인코딩 된걸 다시 디코딩(URLSearchParams)하여 데이터를 검색하는데 사용할 수 있다.

## History 객체
[history_mdn](https://developer.mozilla.org/ko/docs/Web/API/History)
- history.back() : 세션 기록의 바로 뒤 페이지로 이동하는 비동기 메서드 (브라우저 뒤로가기)
- history.forward() : 세션 기록의 바로 앞 페이지로 이동하는 비동기 메서드 (브라우저 앞으로 가기)
- history.pushState(state, title, url) : 웹 브라우저의 페이지 이동 히스토리에 가상의 주소를 추가한다. state -> 페이지 정보
