---
title: "XMLHttpRequest_Ajax 개념설명 & 예제"
execrpt: "Ajax 개념"
toc: true
toc_sticky: true
categories:
  - JS
tags:
  - JS
last_modified_at: 2022-10-25
---
## AJAX란?

AJAX란 비동기 자바스크립트와 XML (Asynchronous JavaScript And XML)을 말합니다. 간단히 말하면, 서버와 통신하기 위해 XMLHttpRequest 객체를 사용하는 것을 말합니다.  JSON, XML, HTML 그리고 일반 텍스트 형식 등을 포함한 다양한 포맷을 주고 받을 수 있습니다. AJAX의 강력한 특징은 페이지 전체를 리프레쉬 하지 않고서도 수행 되는 "비동기성"입니다. 이러한 비동기성을 통해 사용자의 Event가 있으면 전체 페이지가 아닌 일부분만을 업데이트 할 수 있게 해줍니다.  

AJAX의 주요 두가지 특징은 아래의 작업을 할 수 있게 해줍니다.  

- 페이지 새로고침 없이 서버에 요청
- 서버로부터 데이터를 받고 작업을 수행
  
[XMLHttpRequest](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest), [Ajax](https://developer.mozilla.org/ko/docs/Web/Guide/AJAX/Getting_Started), [asynchronous JavaScript](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Introducing#%EC%9E%A5%EA%B8%B0_%EC%8B%A4%ED%96%89_%EB%8F%99%EA%B8%B0_%ED%95%A8%EC%88%98%EC%9D%98_%EB%AC%B8%EC%A0%9C%EC%A0%90)  
  
*예시*
```js
// Ajax 기능을 수행하는 통신 객체
const xhr = new XMLHttpRequest();

// 백엔드 페이지에 접속하는 방식 (GET, POST, PUT, DELETE)
const method = 'GET';

// 요청(접속)할 대상 페이지 주소 -> 이 파일의 소스코드를 읽어온다.
const url = 'http://localhost:3000/hello.html';

/**
 * xhr 객체의 상태가 변화할 때 마다 호출되는 이벤트.
 * --> 총 4번 호출된다. (준비완료, 접속시도, 통신중, 통신완료)
 * 각각의 상태는 target.readyState 라는 값을 통해 조회할 수 있다.
*/
xhr.onreadystatechange = (e) =>{
    // 이벤트 정보 안에 포함된 Ajax 처리 결과를 별도의 변수에 복사
    const ajax = e.target;

    // 통신객체의 통신 상태에 따른 분기처리
    switch(ajax.readyState){
        // 준비완료 --> xhr객체의 요청이 초기화 됨 
        case XMLHttpRequest.OPENED:
            break;
        // 접속시도 (일반적인 경우 사용하지 않음)
        case XMLHttpRequest.HEADERS_RECEIVED:
            break;
        // 통신중 (일반적인 경우 사용하지 않음)
        case XMLHttpRequest.LOADING:
           break;
        // 통신완료 -> 백엔드로부터 데이터를 받아왔거나, 에러가 났거나
        case XMLHttpRequest.DONE:
           if(ajax.status == 200){
            // 백엔드와의 통신에 성공한 경우
           }else{
            // 백엔드와의 통신에 실패한 경우
            /**
             * ajax.status의 값에 따라 실패 원인을 판단하여 사용자에게 적절한 메시지를 표시
             * - 404 : Page Not Found (url오타)
             * - 400 : 접근권한 없음. (url을 폴더까지만 지정했으나 해당 폴더에 index.html이 없는 경우)
             * - 403 : 서버의 접근 거부 (ex: 파일명이 지정되지 않고 index.html도 없는 경우)
             * -50x : 백엔드 시스템 에러. 이 경우 Frontend에서 할 수 있는 작업이 없다.
            */
           }
           break;
    }
};
xhr.open(method,url); // 요청을 초기화 --> 통신준비를 마침

// 백엔드와의 통신 시도 (통신과정에서 총 4번의 이벤트가 발생한다. -> 준비완료,접속시도,통신중,통신완료)
// 통신 결과를 리턴받는 것이 아닌 통신 과정중에 발생하는 이벤트를 통해 후속처리를 구현해야 한다.
xhr.send();
```
