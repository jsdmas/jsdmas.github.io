---
title: "TS install"
execrpt: "TypeScript install 방법"
toc: true
toc_sticky: true
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-02-05
---
# install & target

nodejs 프로젝트 만들기
```
npm init -y
```
TypeScript 설치
```
npm i -D typescript
```
ts config 파일 만들기
```
touch tsconfig.json
```
위의 tsconfig.json이 있으면 vscode는 타입스크립트로 작업한다는 것을 즉시 알게되고 프로젝트를 컴파일하는 데 필요한 루트 파일과 컴파일러 옵션을 지정한다.  
  
**tsconsfig.json** 설정
```json
{
    // include의 배열에는 자바스크립트로 컴파일하고 싳은 모든 디렉터리를 넣어준다.
    "include":["src"],
    "compilerOptions": {
        // outDir는 자바스크립트 파일이 생성될 디렉토리를 지정한다.
        "outDir": "build"
    }
}
```

**package.json** 설정
```json
{
    "script": {
        "build" : "tsc"
    }
}
```
> npm run build

명령어를 실행시 작업중인 디렉토리에 build/index.js 로 컴파일되어 나타나게 된다.  
위의 컴파일되는 코드를 다른 버전의 자바스크립트로 바꿀수 있다.  
**tsconsfig.json** 설정
```json
{
    // include의 배열에는 자바스크립트로 컴파일하고 싳은 모든 디렉터리를 넣어준다.
    "include":["src"],
    "compilerOptions": {
        // outDir는 자바스크립트 파일이 생성될 디렉토리를 지정한다.
        "outDir": "build",
        // target은 어떤 버전의 자바스크립트로 컴파일 하고싶은지를 나타낸다.
        "target": "ES3"
    }
}
```

# Lib Configuration

lib은 합쳐진 라이브러리의 정의 파일을 특정해주는 역할을 한다.(타겟 런타임 환경이 뭔지를 알려준다.)   
코드가 동작하는 환경에 따라, 타입스크립트는 기본적으로 API의 타입을 알기에, 타입에 대해 알려준다.  
자동완성 기능을 사용하려면 정해줘야한다.    
  
**tsconfig.json**
```json
{
    "include": [
        "src"
    ],
    "compilerOptions": {
        "outDir": "bulid",
        "target": "ES6",
        // DOM 은 브라우저에서 실행된다는것을 나타낸다.
        "lib": ["ES6","DOM"]
    }
}
```

# Declaration Files
타입스크립트가 localStorage, Math, Window 등의 타입을 어떻게 이해하고 인지하는지 설정하는 파일이다.  
정의 파일은(d.ts) 자바스크립트 코드의 모양을 타입스크립트에 설명해주는 파일이다.

# Strict

모든 엄격한 타입 검사 옵션을 활성화한다.  
strict 플래그는 프로그램 정확성을 더 강력하게 보장하는 광범위한 타입 검사 동작을 가능하게 한다.  

# allowJs

JavaScript 파일을 프로그램에서 작동할 수 있게 되도록 허용한다.  
typescript가 js파일 안에 들어와서 함수를 다 불러올 수 있게 한다.

# JSDoc

JSDoc은 쉽게 말해서 코멘트로 이루어진 문법이다.  
기존의 js파일을 ts처럼 보호받게 하고싶다면 `@ts-check`라는 주석을 달면 된다.   
@ts-check는 타입스크립트 파일에게 자바스크립트 파일을 확인하라고 알리는 것이다.  

```js
// @ts-check
/**
 * Initalizes the project
 * @param {object} config
 * @param {boolean} config.debug
 * @param {string} config.url
 * @returns {boolean}
 */
export function init(config){
    return true;
}
```





