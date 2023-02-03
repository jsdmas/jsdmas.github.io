---
title: "JS Async Await"
execrpt: "js Async Await 예제 & 설명"
categories:
  - JS
tags:
  - JS
last_modified_at: 2023-02-03
---

# Async Await

[MDN-async function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)  
  
[MDN-await](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/await)  
  
[Async Await](https://ko.javascript.info/async-await)
async와 await라는 특별한 문법을 사용하면 프라미스를 좀 더 편하게 사용할 수 있습니다.   
function 앞에 async를 붙이면 해당 함수는 항상 프라미스를 반환합니다. 프라미스가 아닌 값을 반환하더라도 이행 상태의 프라미스(resolved promise)로 값을 감싸 이행된 프라미스가 반환되도록 합니다.  
자바스크립트는 await 키워드를 만나면 프라미스가 처리될 때까지 기다립니다(await는 '기다리다’라는 뜻을 가진 영단어입니다)  

```js
const getMoviesAsync = async() => {
    const response = await fetch("https://yts.mx/api/v2/list_movies.json");
    const json = await response.json();
    console.log(json);
};

getMoviesAsync();
```

# try catch finally

error를 잡기위해선 아래처럼 try catch문을 활용한다.  

```js
const getMoviesAsync = async() => {
    try{
    const response = await fetch("https://yts.mx/api/v2/list_movies.json");
    const json = await response.json();
    console.log(json);
    } catch (error){
        console.log(error);
    }finally{
        console.log("We are done!!~");
    }
};

getMoviesAsync();
```

# Parallel Async Await

```js
const getMoviesAsync = async() => {

    try{
        const [movieResponse, suggestionsResponse] = await Promise.all([
            fetch("https://yts.mx/api/v2/list_movies.json"),
            fetch("https://yts.mx/api/v2/movie_suggestions.json?movie_id=100")
        ]);
        const [movie, suggestions] = await Promise.all([
            movieResponse.json(),
            suggestionsResponse.json()
        ]);

        console.log(movie, suggestions);
    } catch (error){
        console.log(error);
    }finally{
        console.log("We are done!!~");
    }
};

getMoviesAsync();
```