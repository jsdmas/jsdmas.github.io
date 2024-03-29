---
title: "쿠키, 세션"
execrpt: "쿠기, 세션 설명"
toc: true
toc_sticky: true
categories:
  - theory
tags:
  - theory
last_modified_at: 2022-12-14
---

# 쿠키 (Cookie)
변수를 클라이언트(=웹 브라우저)에 텍스트 파일 형식으로 저장해 놓은 것.  
저장 주체가 백엔드일 경우 백엔드가 갖고 있는 변수 값을 프론트엔드에 보관시킨다는 의미.  
백엔드에 접속하는 개인별로 맞춤형 정보를 저장할 수 있기 때문에 개인화 기능 구현에 활용됨.  
텍스트 파일의 특성상 보안에 민감한 정보는 기록해서는 안된다.  
백엔드와 프론트엔드가 변수값을 공유할 수 있는 가장 원시적인 방법.  
저장위치가 브라우저이기 때문에 프론트엔드에서도 쿠키를 저장하거나 읽어올 수 있는 API가 존재함.(같은 값 공유 가능)  
하지만 보안에 최악.  
## 쿠키의 특성
일반 변수는 프론트엔드가 페이지를 이동하면서 사라진다.  
기본적으로 브라우저를 닫기 전까지는 사이트 내의 모든 페이지가 공유하는 전역변수의 역할을 한다.  
설정에 따라 일정 유효기간 동안은 브라우저를 재시작해도 변수값을 유지할 수 있다.  
## 패키지 설치
```
yarn add cookie-parser
```
## 쿠키 저장에 필요한 조건


| 이름     | 설명                                                                                                                                                                       |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| maxAge   | 유효시간(ms단위). 설정하지 않을 경우 브라우저를 닫으면 삭제됨.                                                                                                             |
| domain   | 유효 도메인 지정. 지정하지 않을 경우 현재 도메인<br /> ex) 도메인이 itpaper.co.kr인 경우 ".itpaper.co.kr"로 설정하면 해당 도메인 내의 모든 사이트가 쿠키를 공유할 수 있다. |
| httpOnly | boolean(true/false). false인 경우 https에서도 식별가능.                                                                                                                    |
| path     | 유효경로(기본값'/') -> 특별한 일이 없다면 지정 안함.                                                                                                                       |
| signed   | 암호화 여부<br /> app.use(cookieParser("암호화키")); 형식으로 암호화 키 지정 필요함.                                                                                       |


# 세션 (Session)
쿠키와 마찬가지로 사이트 내의 모든 페이지가 공유하는 전역변수.  
단, 유효시간은 설정할 수 없기 떄문에 브라우저가 닫히거나 마지막 접속 이후 5분간 재접속이 없다면 자동 폐기된다.(시간은 설정 가능함.)  
특정 클라이언트에게 종속된 개인화 정보를 백엔드가 직접 저장/관리하는 형태.  
많은 데이터를 보관할 수는 없다.  
쿠키보다 보안에 유리하므로 로그인 정보 관리 등에 사용된다.  
```
yarn add express-session
```

