---
title: '데이터 바인딩'
toc: true
toc_sticky: true
categories:
  - theory
tags:
  - theory
last_modified_at: 2023-08-09
---

# 데이터 바인딩이란?

> 데이터 바인딩은 뷰와 모델을 하나로 연결하는 것을 의미한다.

두 데이터 혹은 정보의 소스를 일치시키는 기법으로, 화면에 보이는 데이터와 브라우저 메모리에 있는 데이터(여러개의 자바스크립트 객체)를 일치시키는 것을 말합니다.

예를 들어, MVC 모델에서 model, view를 서로 묶어 model과 view의 `자동 동기화`시키기 라고 이해할 수 있습니다.

## 양방향 데이터 바인딩

`controller`와 `view`양쪽의 데이터 일치가 모두 가능한 것이 양방향 데이터 바인딩 입니다.

- 장점
  - 코드의 사용면에서 코드량을 크게 줄여줍니다.
- 단점
  - 변화에 따라 DOM 객체 전체를 렌더링해주거나 데이터를 바꿔주므로, 서능이 감소되는 경우가 있습니다.

> `controller`에서 `model`이 변경된다 -> `view`가 변경된다.  
> `view`에서 `scope model`이 변경된다 -> `controller`에서 `model`이 변경된다.

- 데이터의 변화를 감지하여 템플릿과 결합해 화면을 갱신, 화면의 입력에 따라 데이터를 갱신하는 것입니다.
  - (HTML -> JS, JS -> HTML 양쪽 모두 가능합니다.)
- 데이터의 변경을 프레임워크에서 감지하고 있다가, 데이터가 변경되는 시점에 DOM 객체에 렌더링을 해주거나 페이지 내에서 모델의 변경을 감지해 JS 실행부에서 변경합니다.
- 입력된 값이나 변경된 값에 따라 내용이 바로 바뀌기 때문에 따로 체크해주지 않아도 됩니다.
- 웹 애플리케이션의 복잡도가 증가할수록 코드 양을 줄여주고 유지보수도 쉽게 해줍니다. (vue.js)

## 단방향 데이터 바인딩

- 장점
  - 데이터 변화에 따른 성능 저하 없이 DOM 객체 갱신이 가능합니다.
  - 데이터 흐름이 단방향(부모 -> 하위 컴포넌트)이라서 코드를 이해하기 쉽고 데이터 추적, 디버깅이 쉽습니다.
- 단점
  - 변화를 감지하고 화면을 업데이트 하는 코드를 매번 작성해야 합니다. (React)

데이터와 템플릿을 결합해 화면을 생성하는 것입니다. (JS -> HTML 만 가능)  
사용자의 입력에 따라 데이터를 갱신하고 화면을 업데이트 해야 하므로 단방향 데이터 바인딩으로 구성하면, 데이터의 변화를 감지하고 화면을 업데이트 하는 코드를 매번 작성해주어야 합니다.

# Reference

- [https://poiemaweb.com/angular-component-data-binding](https://poiemaweb.com/angular-component-data-binding)
- [https://velog.io/@sunaaank/data-binding](https://velog.io/@sunaaank/data-binding)
