---
title: "Router"
execrpt: "ROUTERS 간단설명"
toc: true
toc_sticky: true
categories:
  - JS
tags:
  - JS
  - routers
  - express
last_modified_at: 2022-08-15
---

## Router

- [라우터_express](https://expressjs.com/ko/starter/basic-routing.html)
- 라우터는 컨트롤러와 URL의 관리를 쉽게 해준다.
- 작업중인 주제를 기반으로 URL을 그룹화해준다. 
- 뭔가를 import하기 전에는 export를 먼저 해야한다.
- 폴더에 있는 모든 js파일은 독립되어있다

간단한 예제와 함께 살펴보도록 하자  
<div class="notice--primary" markdown="1">
`Server.js`  

```js
import express from "express";
// express를 express라는 이름으로 import
import globalRouter from "./routers/globalrouter";
// routers폴더의 globalrouter를 globalRouter라는 이름으로 import
app.use("/", globalRouter);
```

`globalRouter.js`

```js
import express from "express";

import { join, login } from "../controllers/userController";
// userController안의 join, login 을 사용하기위해 import
// 위와같이 import하면 변수이름을 정확히 써야한다.

const globalRouter = express.Router();
// globalRouter 생성

globalRouter.get("/join", join);
globalRouter.get("/login", login);
//url은 함수의 이름과 꼭 같을 필요는 없다.

export default globalRouter;
// export : 내보내다
// default로 내보내면 globalRouter의 변수명을 다르게 지정할 수 있다.
```

`userController.js`

```js
export const join = (req, res) => res.send("Join");
export const login = (req, res) => res.send("Login");
```
</div>
 