---
title: "GET, POST"
execrpt: "GET, POST 간단설명"
toc: true
toc_sticky: true
categories:
  - express
tags:
  - pug
  - nodejs
  - express
last_modified_at: 2022-08-20
---

## absoute & relative url

- absoute

```
a(href="/edit") Edit Video &rarr;
```
`서버주소/edit`으로 이동
*"&rarr"; 는 오른쪽화살표 표시를 나타내는 기호다.

- relative url
```
a(href="edit") Edit Video &rarr;
```
`서버주소/중간경로/edit`으로 이동

## form 활용
- method라는 게 바로 form과 back end 사이의 정보 전송에 관한 방식이다.
- `get`
get 방식을 쓰는 from은 구글이나 네이버에서 뭔가를 검색을 할 떄 그 검색어가 주소창에 포함되어 있을 것이다. (검색, 데이터 받기)

- `post`
post 방식은 파일을 보내거나, database에 있는 값을 바꾸는 뭔가를 보낼 떄 사용한다. (수정, 추가, 삭제)

```
    form(method="POST")
        input(placeholder="Video Title", value=video.title, required )
        input(value="save", type="submit")
```
만약 별개의 url을 만들고 싶다면 form 의 action 값에 주소를 적어주면 된다.
  
ex)
```
    form(method="POST" action="/save-changes")
        input(placeholder="Video Title", value=video.title, required )
        input(value="save", type="submit")
```

### router 짧게쓰는법

`videoRouter.js`
```js
videoRouter.route("/:id(\\d+)/edit").get(getEdit).post(postEdit);
// 아래 명령어들을 합친것
videoRouter.get("/:id(\\d+)/edit", getEdit);
videoRouter.post("/:id(\\d+)/edit", postEdit)
```

### redirect()

- 브라우저가 redirect(자동으로 이동)하도록 하는 것이다.

ex)
```
export const postEdit = (req, res) => {
    const { id } = req.params;
    res.redirect(`/videos/${id}`);
}
```

## express application - form 활용
- application에게 form을 처리하고 싶다고 말하려면 `express.urlencoded`를 활용하면된다.
- form의 데이터를 자바스크립트 object 형식으로 준다.
- [urlencoded](https://expressjs.com/ko/api.html#express.urlencoded)

<div class="notice--primary" markdown="1">

`server.js`
```
app.use(express.urlencoded( { extended:true }) );
```
- express application이 form의 value들을 이해할 수 있도록 하고, 내가 쓸 수 있는 자바스크립트 형식으로 변형시켜 준다.
</div>


## 서버 연습

<div class="notice--primary" markdown="1">

`server.js`
```js
import express from "express";
import morgan from "morgan";
import globalRouter from "./routers/globalRouter";
import videoRouter from "./routers/videoRouter";
import userRouter from "./routers/userRouter";


const PORT = 4000;

const app = express();
const logger = morgan("dev");

app.set("view engine", "pug");
// view engine을 pug를 사용한다 선언
// express가 view 디렉토리에서 pug 파일을 찾도록 설정되어 있어서 따로 설정하지 않아도 된다.
app.set("views", process.cwd() + "/src/views")
// package.json파일을 기준으로 실행경로를 잡아서 위의 코드로 src안에있는 pug파일의 경로를 잡아준다.
// default값은 "현재 작업 디렉토리 + /views" 이다.
app.use(express.urlencoded({ extended: true }));
//HTML form을 이해하고 그 form을 우리가 사용할 수 있는 JS object 형식으로 통역해준다.
app.use("/", globalRouter);
app.use("/videos", videoRouter);
app.use("/users", userRouter);

const handleListening = () =>
    console.log(`Server listenting on port http://localhost:${PORT}`);

app.listen(PORT, handleListening); 

```
`videoRouter.js`
```js
import express from "express";
import { watch, getEdit, postEdit, getUpload, postUpload } from "../controllers/videoController";

const videoRouter = express.Router();

videoRouter.get("/:id(\\d+)", watch);
videoRouter.route("/:id(\\d+)/edit").get(getEdit).post(postEdit);
videoRouter.route("/upload").get(getUpload).post(postUpload);
export default videoRouter;
```
`videoController.js`
```js
const videos = [
    {
        title: "First Video",
        rating: 5,
        comments: 2,
        createdAt: "2 minutes ago",
        views: 1,
        id: 1
    },
    {
        title: "Second Video",
        rating: 5,
        comments: 2,
        createdAt: "3 minutes ago",
        views: 59,
        id: 2
    },
    {
        title: "Third Video",
        rating: 5,
        comments: 2,
        createdAt: "4 minutes ago",
        views: 59,
        id: 3
    }
]
export const trending = (req, res) => res.render("home", { pageTitle: "Home", videos });
export const watch = (req, res) => {
    const { id } = req.params;
    // params : 매개변수  , const id = req.params.id ; 
    // params 는 videoRouter의 /:id(\\d+) 부분을 말한다.
    const video = videos[id - 1];
    return res.render("watch", { pageTitle: `Watching ${video.title}`, video });
    // watch 라는 template render해주기
    // pageTitel에 보내는 변수명이 videos.title 이 아니라 video.title인 이유는 
    // trending 함수가 home으로 videos배열을 render해준뒤 데이터를 mixins를 통해 받기 때문에 video라 적은것.
}

export const getEdit = (req, res) => {
    const { id } = req.params;
    const video = videos[id - 1];
    return res.render("edit", { pageTitle: `Editing : ${video.title}`, video });
}
export const postEdit = (req, res) => {
    const { id } = req.params;
    const { title } = req.body;
    // form에서 오는 body에서 title 얻기
    // req.body = form에 있는 value의 javascript representation이다.(JS식으로 표현한 것)
    // input에 name이 없다면 req.body에서 데이터를 볼 수 없다.
    videos[id - 1].title = title;
    res.redirect(`/videos/${id}`);
}
export const getUpload = (req, res) => {
    return res.render("upload", { pageTitle: "Upload Video" })
}

export const postUpload = (req, res) => {
    const { title } = req.body;
    const newVideo = {
        title,
        rating: 0,
        comments: 0,
        createdAt: "just now",
        views: 0,
        id: videos.length + 1,
    };
    videos.push(newVideo);
    return res.redirect("/");
}
```
`upload.pug`
```
extends base.pug

block content 
    form(method = "POST")
        input(type="text", requried, placeholder="Title", name="title")
        input(type="submit", value="Upload Video")
```
</div>

