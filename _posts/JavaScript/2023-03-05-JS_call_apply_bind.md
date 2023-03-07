---
title:  "JS call, apply, bind 함수 설명"
excerpt: "call, apply, bind 함수 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-03-05
---
기본적으로 위의 3개의 함수는 함수 호출 방식과 관계없이 `this`를 지정할 수 있습니다.  

# call
call 메서드는 모든 함수에서 사용할 수 있으며, this를 특정값으로 사용할 수 있습니다.  

# apply

apply는 함수 매개변수를 처리하는 방법을 제외하면 call과 완전히 같습니다.  
call은 일반적인 함수와 마찬가지로 매개변수를 직접 받지만, apply는 매개변수를 배열로 받습니다.  

# bind
함수의 this 값을 영구히 바꿀 수 있습니다.
