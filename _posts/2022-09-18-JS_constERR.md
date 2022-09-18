---
title: "Assignment to constant variable 해결 방법"
execrpt: "js ERR"
toc: true
toc_sticky: true
categories:
  - err
tags:
  - err
last_modified_at: 2022-09-18
---
## 에러 발생 코드
![](https://user-images.githubusercontent.com/105098581/190885652-5a480a51-1d23-4568-b1e4-74c80e365637.png)

## 에러 원인

- 이미 선언한 const 변수 myarr에 새로운 값을 할당했을 때 발생.
- const 변수는 재할당을 허용하지 않는다.