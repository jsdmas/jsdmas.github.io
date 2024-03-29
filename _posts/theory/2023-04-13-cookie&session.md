---
title:  "cookie, session 설명"
excerpt: "cookie, session 설명"
categories:
  - theory
tags:
  - theory
last_modified_at: 2023-04-13
---

# cookie

[cookie_mdn](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)   
HTTP 쿠키는 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각을 뜻합니다.  
브라우저는 그 데이터 조각들을 저장해 놓았다가, 동일한 서버에 재 요청 시 저장된 데이터를 함께 전송합니다. 쿠키는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지를 판단할 때 주로 사용합니다.  
이를 이용하면 사용자의 로그인 상태를 유지할 수 있습니다. 상태가 없는(stateless) HTTP 프로토콜에서 상태 정보를 기억시켜주기 때문입니다.  
- 세션 관리 (session management)
  - 서버에 저장해야 할 로그인, 장바구니, 게임 스코어 등의 정보 관리
- 개인화 (Personalization)
  - 사용자 선호, 테마 등의 세팅
- 트래킹(Tracking)
  - 사용자 행동을 기록하고 분석하는 용도

## 쿠키 만들기 
HTTP 요청을 수신할 때, 서버는 응답과 함꼐 Set-Cookie 헤더를 전송할 수 있습니다.  
쿠키는 보통 브라우저에 의해 저장되며, 그 후 쿠키는 같은 서버에 의해 만들어진 요청(Request)들의 Cookie HTTP 헤더안에 포함되어 전송됩니다. 만료일 혹은 지속시간(duration)도 명시될 수 있고, 만료된 쿠키는 더이상 보내지지 않습니다.  
추가적으로, 특정 도메인 혹은 경로 제한을 설정할 수 있으며 이는 쿠키가 보내지는 것을 제한할 수 있습니다.  
  
```
Set-Cookie : 쿠키 이름=쿠키 값
```

## option

- Max-Age : 쿠키의 수명시간을 정합니다.
- Secure : http 가 아닌 https 를 쓰는경우만 웹브라우저가 웹서버에 전송하게 설정합니다.
- HttpOnly : 웹브라우저와 웹서버가 통신할때만 쿠키를 발행합니다. (javascript로 쿠키값을  가져올 수 없게 합니다.)
- Path : 지정한 경로와 경로안에서만 path가 활성화 되어서 해당되는 쿠키만 서버에게 전송합니다.
- Domain : 어떤 도메인에서 작동할건지를 제한합니다.

# session

세션(session)이란 웹 사이트의 여러 페이지에 걸쳐 사용되는 사용자 정보를 저장하는 방법을 의미합니다.  
사용자가 브라우저를 닫아 서버와의 연결을 끝내는 시점까지를 세션이라고 합니다.  
쿠키는 클라이언트 측의 컴퓨터에 모든 데이터를 저장합니다.  
하지만 세션은 서비스가 돌아가는 서버 측에 데이터를 저장하고, 세션의 키값만을 클라이언트 측에 남겨둡니다. 브라우저는 필요할 때마다 이 키값을 이용하여 서버에 저장된 데이터를 사용하게 됩니다.  
이러한 세션은 보안에 취약한 쿠키를 보완해주는 역할을 하고 있습니다.  

