---
title: "NodeJS, NPM 간단설명"
execrpt: "nodeJS & npm & express 설명"
toc: true
toc_sticky: true
categories:
  - clone
tags:
  - JS
  - nodejs
  - npm
  - express
last_modified_at: 2022-08-14
---

## NodeJS

- 자바스크립트는 브라우저에서만 사용되어졌다.
- 자바스크립트를 브라우저에서 분리해서 만든것이 NodeJS 이다.
- nodejs와 npm은 같이 사용해야 하기때문에 설치시 같이 설치된다.
[NodeJS_공식홈페이지](https://nodejs.org/ko/)

- Json : 프로그래머가 파일에 정보를 저장하기 위해 만든 방식 중 하나이다. (파일에 정보를 입력하는 방식.)
- nodeJS인 경우 이런 파일의 이름은 package.json 이어야한다. (변경불가, 대문자금지 꼭 소문자로 package.json 이라고 써야한다.)
- nodeJS 프로젝트를 만들 떄 가장 먼저 만들어야 할 파일이 package.json이다. 
## NPM
- Npm init : 새로운 프로젝트나 패키지를 만들때 사용.
- npm은 자바스크립트 언어를 위한 패키지 매니저이다.
- npm은 nodeJS와 상호작용을 할 수 있게 해준다.
- [npm_공식홈페이지](https://www.npmjs.com/)

## setup

### package.json

- package.json은 node.js 관련 정보를 담는 방법이다.
- 그냥 text기 떄문에 뭘 넣어도 상관없다.
  
- package.json에 넣으면, npm이 어떤 행동을 할 수 있게 해주는 것들이 있다.
- 예를 들어 scripts entry를 생성하고 그안에 script를 입력하면, 콘솔에 npm run (script 이름)을 사용할 수 있다.

```
"scripts": {
  "dev": "nodemon --exec babel-node src/server.js"
}
콘솔에 script 전체를 입력하게 하는 대신, dev라는 별명을 준다.

```
- package.json은 말 그대로 package이다. 그냥text일뿐 새로운 것은 없다.
- 하지만 특정 장소에 뭔가를 입력하면 npm이 그걸 사용할 수 있다

### dependencies
- 프로젝트가 돌아가기 위해 필요한 package들이다.

ex)
```
"dependencies": {
    "express": "^4.18.1"
  },
  "devDependencies": {
    "@babel/core": "^7.18.10",
    "@babel/node": "^7.18.10",
    "@babel/preset-env": "^7.18.10",
    "nodemon": "^2.0.19"
  }
```
- "express"와 버전 정보는, express를 설치했을 떄 자동으로 추가된 것이다.
- npm install express를 입력하면, npm이 express를 다운받고, 버전 정보를 확인해서 dependencies에 정보를 추가한다.
- express는 node_module로 들어간다.
- 설치하는 모든 것들은 node_modules라는 폴더에 저장된다.

  
- node_modules 폴더는, git에 업로드할 필요가 없다.(package.json만 주면 npm i 실행시 설치가 다 되기때문.)
- package.json이 있는 상태에서 npm install을 할 경우 npm이 dependencies, devDependencies를 찾아서, 있는 모든 걸 자동으로 설치해 준다.

### devDependencies

- dependencies와 devdependencies 두 경우 다 node_modules로 가게 된다.
- 두 개의 실직적인 차이

```
* dependencies
프로젝트가 작동하기 위해 필요한 것들이다.
ex) express

* devDependencies
개발자가 개발할 떄 필요한 것들이다.
ex) nodemon : 파일을 보고 있다가 변화가 생기면 commend를 재시작 해준다. (일일히 npm run 할필요가 없다.)
```
자동차에 비유하자면 dependencies는 자동차가 움직이는데 필요한 연료, devdependencies는 차에서 음악을 들을 수 있게 하는 것이다.
### babel

- 가끔 최신 node.js가 코드를 지원하지 않을 때가 있기 때문에 설치해야 한다. (ES6)
- ES란, ECMAScript의 약자이며 자바스크립트의 표준, 규격을 나타내는 용어이다. 뒤에 숫자는 버전을 뜻하고 ES5는 2009년 ES6는 2015년에 출시되었다.
- 자바스크립트 컴파일러이다.

```
"scripts": {
  "dev": "nodemon --exec babel-node src/server.js"
}
```
- 위 코드에서 server.js를 node로 돌리는 게 아니라 babel-node로 돌리는 것이다.
- babael-node를 사용하려면 babel.config.json이라는 파일을 만들어야 한다.
- babel.config.json에 추가하고 싶은 plugin을 넣어 사용한다.

ex)
```
{
  "presets":["@babel/preset-env"] 
}
```

- [babael_공식홈페이지](https://babeljs.io/)

### package-lock.json

- 나의 패키지들을 안전하게 관리해준다.
- 패키지가 수정 됐는지 해시값으로 체크해준다.

## Servers

- 서버는 항상 켜져 있고, 인터넷이 연결 돼 있으면서 request를 listening하고 있는 컴퓨터이다.
- request는 우리가 서버에게 요청하는 것들이다.
- 브라우저를 통해 웹사이트에게 하는 모든 상호작용이 request이다.


### 예제

```js
import express from "express";
// node_modules/express에서 express가 import된다.
const app = express();
// app 변수에 express함수를 실행하면 express application을 바로 사용할 수 있게 return해 준다.
// 모든 게 app에 있다.
const PORT = 4000;
//PORT를 쓰는 이유는 서버는 사용자 컴퓨터 전체를 listen할 수 없다. port는 컴퓨터로의 창문이나 문 같은 것으로 프로그램들과 소통하기 위해 사용한다. request를 보낼 떄, 해당 port로 request를 보낸다.
const handleListening = () => console.log(`Server listening on port http://localhost:${PORT}`);

app.listen(PORT, handleListening);
// 여기서 하는 일은 문이나 창문을 여는 것과 같다. 
// handleListening 함수는 listening이 시작되면 호출되는 함수이다.
```
### routes

- 위의 코드를 실행해서 locahhost:4000 page를 가보면 'Cannot GET / ' 이라는 문구가 보일 것이다.
- GET은 가장 기본적인 method이다. 웹사이이트를 접속할떄 웹사이트가 나에게 오게 하는 것이다.
- 내가 요청을 하면, 내 서버가 나의 브라우저로 보내주는 것이다.
- 브라우저가 어디로 가는 게 아니고, 받기만 하는 것이다.
- 서버는 url을 통해 requests를 전달한다.
- / (home), /login , /password-change 이런 페이지들을 routes라고 부른다.
  
### Controllers

브라우저가 누군가 홈페이지를 get 하려고 하면 어떻게 반응할 지 알려줘야 한다.
express js에서 이걸 하는 방법은 app.get을 사용하는 것이다.  
- [express_공식홈페이지](https://expressjs.com/ko/)
  
```js
app.get("/", handleHome)
//route [home (/)], funtion 순서이다. 하나가 될수도 여러 개가 될 수도 있다.
// routes를 만들고 controllers를 만든다.
```
- 모든 controller에는 request와 response가 있다.

```js
const handleHome = (req, res) => {
    return res.send("I love middlewares");
    // 참고로 arrow function은 return이 내포돼 있다.
    // request에 대한 정보를 주고, response는 request에 어떻게 응답하느냐는 것이다.
    // 무엇으로든 응답해 주는 게 필수이다. 파일, redirect, text, status code 뭐가 됐든 응답해 줘야한다.
    //응답해 주지 않으면 브라우저는 계속 로딩만 하고, 웹사이트는 느려진다.
};
// request랑 response는 express로부터 주어진다.
// express가 request object를 제공하고, 그 안에는 누가 웹사이트를 request하는지 cookies, 브라우저 정보, IP 주소 같은, request와 관련된 정보가 있다.


res.end() -> 서버종료
res.send() -> 뭔가를 보낼떄 사용
// 종류가 많으니 더 알아보고 싶다면 위의 링크를통해 확인하자.
```

### middlware

- middlware는 중간(middle)에 있는 software이다.
- 여기서 중간은 request와 response가 움직이는 활동의 중간을 말한다.
- middlewares는 controller와 비슷하다. 실질적으로 controller가 middleware 그 자체이다. 모든 게 middleware이다.
- middleware도 request와 response, 추가로 next도 있다. (home에도 next가 있다.)

```js
const logger = (req, res, next) => next();
const logger2 = (req, res, next) => next();

const home = (req, res) => res.send("hello");
const loginPage = (req, res) => res.send("loginPage");

app.get("/", logger, logger2 ,home);
// next함수를 호출하면 express가 다음 함수를 호출한다. (누군가 응답할 떄까지)
// middleware는 원하는 만큼 쓸 수 있다. 
// 물론 중간에 뭔가를 return 해준다면 home은 호출되지 않는다.
app.use(logger, logger2);
app.get("/login", loginPage);
// 만약 logger, logger2를 생략하고 싶다면 app.use(logger, logger2);를 써주면 알아서 적용이 된다.
// 위의 /login 페이지로 가는 코드는 logger, logger2를 적용시키고있다. (app.use에서 선언을 해줫기때문.)
// 쓰는순서는 중요하다.(app.use가 위의 로그인 아래에 있으면 작동X)
// 연결이 위에서부터 온다고 생각하면 편하다.
```

### Morgan

- morgan은 node.js 용 request logger middleware다.
- [morgan](https://www.npmjs.com/package/morgan) , [morgan_github](https://github.com/expressjs/morgan)
- morgan도 마찬가지로 import 해서 사용할 수 있다.
- morgan은 GET, path, status code 이 모든 정보를 가지고 있다.

  
ex)
```js
import morgan from "morgan";

const logger = morgan("dev");
// morgan 함수는 middleware를 return해준다.
// 위의 dev 말고도 다른 옵션도 있다.
// morgan("dev")를 호출하면 request, response, next를 포함한 함수를 return해 준다.
const handlefunction = (req, res) => res.end();

app.get("/", logger, handlefunction);
// 콘솔창을 살펴보면 정보들이 적혀있다.
```