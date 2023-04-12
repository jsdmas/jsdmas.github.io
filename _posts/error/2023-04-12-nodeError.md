---
title: "Cannot set headers after they are sent to the client"
execrpt: "node Error 처리"
categories:
  - err
tags:
  - err
last_modified_at: 2023-04-12
---

“Cannot set headers after they are sent to the client” 오류는 응답 헤더가 이미 클라이언트로 전송된 후에 다시 설정하려고 할 때 발생합니다.   
이 오류는 일반적으로 두 가지 이유로 발생합니다.  
1. 응답 객체의 send 또는 end 메소드가 두 번 이상 호출되었을 때 발생합니다.  
2. 미들웨어에서 응답이 전송된 후에 다음 미들웨어에서 응답 객체의 send 또는 end 메소드가 호출되었을 때 발생합니다.

이 문제를 해결하려면 코드를 검토하여 응답 객체의 send 또는 end 메소드가 한 번만 호출되도록 수정해야 합니다.  
또한, 미들웨어에서 응답이 전송된 후에 다음 미들웨어로 이동하지 않도록 return 문을 사용하여 함수를 종료해야 합니다.   


`error code`
```js
export const createAcount = async (req, res, next) => {
    const { body: { data: { email, password, nickname } } } = req;
    let result = null;
    try {
        // 유효성 검사
        // 유효성 검사전에 비밀번호를 해싱?
        RegexHelper.value(email);
        RegexHelper.value(password);
        RegexHelper.value(nickname);
        RegexHelper.minLength(password, 1, "비밀번호를 입력해주세요");
        RegexHelper.minLength(nickname, 1, "nickname을 입력해주세요");
        RegexHelper.minLength(email, 1, "이메일을 입력해주세요");
        RegexHelper.maxLength(password, 255, "비밀번호 입력값을 확인해주세요");
        RegexHelper.maxLength(nickname, 20, "nickname 을 확인해주세요");
        RegexHelper.email(email, "이메일을 정확하게 입력해주세요");
    } catch (error) {
        next(error);
    }

    const encryptedPW = await bcrypt.hash(password, 5);

    try {
        result = await userService.createAcount({ email, password: encryptedPW, nickname });
    } catch (error) {
        next(error);
    }

    return res.sendResult({ result });
};

```
백엔드 userService의 createAcount 처리중 에러가 발생시 error 객체를 함수로 전달하여 프론트로 보내는 과정에서 헤더를 2번씩 세팅하고 보내는 일이 생겨났습니다. 이를 해결하기위해 아래 코드처럼 `return next(error)`를 써줘서 간단하게 해결했습니다.  
`바뀐부분`
```js
catch (error) {
     return next(error);
    }
```

이렇게 return 문을쓰면 next를 2번씩 하지 않고 하나의 에러만 통과하게 되어 위와같은 오류가 발생하지 않습니다.👀