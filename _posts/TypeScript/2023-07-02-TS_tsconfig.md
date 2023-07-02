---
title: 'TS config'
execrpt: 'TypeScript config 파일 설명'
toc: true
toc_sticky: true
categories:
  - TypeScript
tags:
  - TypeScript
last_modified_at: 2023-07-02
---

- [ref](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-tsconfigjson-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0-%EC%B4%9D%EC%A0%95%EB%A6%AC)

# tsconfig.json

**tsconfig.json**은 typeScript를 javaScript로 변환시키는 **컴파일 설정을 한꺼번에 정의 해놓는 파일**이라고 보면 됩니다.  
프로젝트를 컴파일 하는데 필요한 루트 파일, 컴파일러 옵션 등을 상세히 설정할 수 있습니다.

보통 tsconfig.json파일은 TypeScript 프로젝트의 루트 디렉토리(Root Directoty)에 위치된다.  
그래서 tsconfig.json 파일이 프로젝트에 있다면 vscode는 우리가 타입스크립트로 개발한다는 것을 인식하게 되는 것입니다.

tsconfig에서 option들을 미리 정의해 놓으면, 더이상 컴파일 할 떄 명렁어에 일일히 대상 파일이나 옵션을 지정히지 않아도 됩니다.  
그래서 `tsc`나 `ts-node`명령어를 그냥 실행하게 되면, 현재 폴더에 있는 tsconfig 설정 내용을 기준으로 프로젝트에서 소스들을 변환 작업(컴파일)을 진행하게 됩니다.

만약 현재 폴더에 tsconfig 설정 파일이 없다면 프로젝트 폴더 내에서 상위 폴더의 경로를 검색해 나갑니다.

# tsconfig 생성

아래의 명렁어는 tsconfig.json 파일을 자동으로 만들어줍니다.

```
tsc --init
```

# tsconfig 전역 속성

파일의 최상위에 위치하고 있는 속성들을 일컫는다.

```json
{
  // TypeScript 컴파일러의 옵션들을 지정하는 속성
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "sourceMap": true
    // ... 무수히 많은 속성들
  },

  // 컴파일할 파일들의 개별 목록을 지정하는 속성
  "files": ["src/main.ts", "src/utils.ts"],

  // 컴파일할 파일들을 지정하는 속성 (와일드 카드 패턴으로 묶어 표현)
  "include": ["src/**/*.ts"],

  // 컴파일 대상에서 제외할 파일들을 지정하는 속성
  "exclude": ["node_modules", "**/*.test.ts"],

  // 다른 tsconfig.json 파일을 상속받아서 설정을 재사용할 수 있게 해주는 속성
  "extends": "./configs/base.json",

  // 여러 개의 하위 프로젝트로 구성된 프로젝트의 의존 관계를 지정하는 속성
  "references": [{ "path": "./subproject1" }, { "path": "./subproject2" }],

  // 타입 습득(type acquisition)과 관련된 옵션들을 지정하는 속성
  "typeAcquisition": {
    "enable": true,
    "include": ["jquery"],
    "exclude": ["react"]
  },

  // watch 모드와 관련된 옵션들을 지정하는 속성
  "watchOptions": {
    "watchFile": "useFsEvents",
    "watchDirectory": "useFsEvents",
    "fallbackPolling": "dynamicPriority"
  }
}
```

주로 쓰이는 다섯가지 속성으로는 **compoilerOptions, files, include, exclude, extends**정도가 있습니다.

## files

프로젝트에서 컴파일할 파일들의 목록을 명시적으로 지정하는 속성입니다.

files 속성은 밑에서 배울 exclude보다 우선순위가 높습니다. 만일 이 속성이 생략되면 include와 exclude 속성으로 컴파일 대상을 결정합니다.

```json
{
  "files": [
    //파일 확장자까지 정확히 작성해줘야 합니다.
    "src/main.ts",
    "src/utils.ts",
    "src/types.d.ts"
  ]
}
```

## extends

extends는 다른 tsconfig.json 파일의 설정들을 가져와 재사용할 수 있게 해주는 옵션입니다.  
보통 extends 속성은 tsconfig.json 파일의 최상위에 위치합니다.

예를들어 config/base.json 파일의 속성 설정을 현 tsconfig.json 파일에 포맷이 맞으면 base파일의 설정을 상속 받게 됩니다.

```json
// config/base.json
{
  "compilerOptions": {
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

```json
{
  "extends": "./configs/base",
  "compilerOptions": {
    "strictNullChecks": false
  },
  "files": ["src/main.ts", "src/utils.ts", "src/types.d.ts"]
}
```

## include

include 속성은 files 속성과 같이 프로젝트에서 컴파일할 파일들을 지정하는 속성이지만, **와일드 카드**패턴으로 지정한다는 점에서 차이가 있습니다.  
또한 include는 files 속성과는 달리 exclude보다 약해 include에 명시되어 있어도 exclude에도 명시되어 있으면 제외 되게 됩니다.

```json
{
  "compilerOptions": {
    ...
  },
  "include": [
    "src/*.ts", // src 디렉토리에 있는 모든 .ts 파일
    "dist/test?.ts" // dist 디렉토리에 있는 test1.ts, test2.ts , test3.ts ..등에 일치
    "test/**/*.spec.ts" // test 디렉토리와 그 하위 디렉토리에 있는 모든 .spec.ts 파일
  ]
}
```

와일드 카드 패턴이란 tsconfig.json 파일에서 include나 exclude 속성에 사용할 수 있는 파일이나 디렉토리를 그룹화하여 일치시키는 기호라고 보면 됩니다.

- \* : 해당 디렉토리에 있는 모든 파일
- ? : 해당 디렉토리에 있는 파일들의 이름 중 한 글자라도 포함하면 해당
- \*\* : 해당 디렉토리의 하위 디렉토리의 모든 파일을 포함

## exclude

exclude 속성은 프로젝트에서 컴파일 대상에서 제외할 파일들을 와일드카드 패턴으로 지정하는 속성입니다.  
즉, include의 반대 버전이라 보면 됩니다.

```json
{
  "compilerOptions": {
    ...
  },

  "exclude": [
    "node_modules", // node_modules 디렉토리를 제외
    "**/*.test.ts" // 모든 .test.ts 파일을 제외
  ]
}
```

# compilerOptions

컴파일 대상 파일들을 어떻게 변환할지 세세히 정하는 옵션입니다.  
이중 가장 애용되는 옵션들만 설명하겠습니다.

<details>
<summary> json 펼치기</summary>

```json
{
  "compilerOptions": {
    /* 기본 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "incremental": true /* 증분 컴파일 활성화 */,
    "target": "es5" /* ECMAScript 목표 버전 설정: 'ES3'(기본), 'ES5', 'ES2015', 'ES2016', 'ES2017','ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */,
    "module": "esnext" /* 생성될 모듈 코드 설정: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
    "lib": ["dom", "dom.iterable", "esnext"] /* 컴파일 과정에 사용될 라이브러리 파일 설정 */,
    "allowJs": true /* JavaScript 파일 컴파일 허용 */,
    "checkJs": true /* .js 파일 오류 리포트 설정 */,
    "jsx": "react" /* 생성될 JSX 코드 설정: 'preserve', 'react-native', or 'react'. */,
    "declaration": true /* '.d.ts' 파일 생성 설정 */,
    "declarationMap": true /* 해당하는 각 '.d.ts'파일에 대한 소스 맵 생성 */,
    "sourceMap": true /* 소스맵 '.map' 파일 생성 설정 */,
    "outFile": "./" /* 복수 파일을 묶어 하나의 파일로 출력 설정 */,
    "outDir": "./dist" /* 출력될 디렉토리 설정 */,
    "rootDir": "./" /* 입력 파일들의 루트 디렉토리 설정. --outDir 옵션을 사용해 출력 디렉토리 설정이 가능 */,
    "composite": true /* 프로젝트 컴파일 활성화 */,
    "tsBuildInfoFile": "./" /* 증분 컴파일 정보를 저장할 파일 지정 */,
    "removeComments": true /* 출력 시, 주석 제거 설정 */,
    "noEmit": true /* 출력 방출(emit) 유무 설정 */,
    "importHelpers": true /* 'tslib'로부터 헬퍼를 호출할 지 설정 */,
    "downlevelIteration": true /* 'ES5' 혹은 'ES3' 타겟 설정 시 Iterables 'for-of', 'spread', 'destructuring' 완벽 지원 설정 */,
    "isolatedModules": true /* 각 파일을 별도 모듈로 변환 ('ts.transpileModule'과 유사) */,

    /* 엄격한 유형 검사 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "strict": true /* 모든 엄격한 유형 검사 옵션 활성화 */,
    "noImplicitAny": true /* 명시적이지 않은 'any' 유형으로 표현식 및 선언 사용 시 오류 발생 */,
    "strictNullChecks": true /* 엄격한 null 검사 사용 */,
    "strictFunctionTypes": true /* 엄격한 함수 유형 검사 사용 */,
    "strictBindCallApply": true /* 엄격한 'bind', 'call', 'apply' 함수 메서드 사용 */,
    "strictPropertyInitialization": true /* 클래스에서 속성 초기화 엄격 검사 사용 */,
    "noImplicitThis": true /* 명시적이지 않은 'any'유형으로 'this' 표현식 사용 시 오류 발생 */,
    "alwaysStrict": true /* 엄격모드에서 구문 분석 후, 각 소스 파일에 "use strict" 코드를 출력 */,

    /* 추가 검사 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "noUnusedLocals": true /* 사용되지 않은 로컬이 있을 경우, 오류로 보고 */,
    "noUnusedParameters": true /* 사용되지 않은 매개변수가 있을 경우, 오류로 보고 */,
    "noImplicitReturns": true /* 함수가 값을 반환하지 않을 경우, 오류로 보고 */,
    "noFallthroughCasesInSwitch": true /* switch 문 오류 유형에 대한 오류 보고 */,
    "noUncheckedIndexedAccess": true /* 인덱스 시그니처 결과에 'undefined' 포함 */,

    /* 모듈 분석 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "moduleResolution": "node" /* 모듈 분석 방법 설정: 'node' (Node.js) 또는 'classic' (TypeScript pre-1.6). */,
    "baseUrl": "./" /* 절대 경로 모듈이 아닌, 모듈이 기본적으로 위치한 디렉토리 설정 (예: './modules-name') */,
    "paths": {} /* 'baseUrl'을 기준으로 상대 위치로 가져오기를 다시 매핑하는 항목 설정 */,
    "rootDirs": [] /* 런타임 시 프로젝트 구조를 나타내는 로트 디렉토리 목록 */,
    "typeRoots": [] /* 유형 정의를 포함할 디렉토리 목록 */,
    "types": [] /* 컴파일 시 포함될 유형 선언 파일 입력 */,
    "allowSyntheticDefaultImports": true /* 기본 출력(default export)이 없는 모듈로부터 기본 호출을 허용 (이 코드는 단지 유형 검사만 수행) */,
    "esModuleInterop": true /* 모든 가져오기에 대한 네임스페이스 객체 생성을 통해 CommonJS와 ES 모듈 간의 상호 운용성을 제공. 'allowSyntheticDefaultImports' 암시 */,
    "preserveSymlinks": true /* symlinks 실제 경로로 결정하지 않음 */,
    "allowUmdGlobalAccess": true /* 모듈에서 UMD 글로벌에 접근 허용 */,

    /* 소스맵 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "sourceRoot": "./" /* 디버거(debugger)가 소스 위치 대신 TypeScript 파일을 찾을 위치 설정 */,
    "mapRoot": "./" /* 디버거가 생성된 위치 대신 맵 파일을 찾을 위치 설정 */,
    "inlineSourceMap": true /* 하나의 인라인 소스맵을 내보내도록 설정 */,
    "inlineSources": true /* 하나의 파일 안에 소스와 소스 코드를 함께 내보내도록 설정. '--inlineSourceMap' 또는 '--sourceMap' 설정이 필요 */,

    /* 실험적인 기능 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "experimentalDecorators": true /* ES7 데코레이터(decorators) 실험 기능 지원 설정 */,
    "emitDecoratorMetadata": true /* 데코레이터를 위한 유형 메타데이터 방출 실험 기능 지원 설정 */,

    /* 고급 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "skipLibCheck": true /* 선언 파일 유형 검사 스킵 */,
    "forceConsistentCasingInFileNames": true /* 동일한 파일에 대한 일관되지 않은 케이스 참조를 허용하지 않음 */
  }
}
```

</details>

## Language and Environment 옵션

### target

어떠한 버전의 javaScript 코드로 컴파일 할지 지정합니다.  
만약 코드가 구식 환경에서 배포된다면 더 낮은 버전을 지정하고, 신식의 환경은 높은 타겟으로 지정합니다.  
버전 지정 값을 넣지 않으면 default로 es5로 컴파일 됩니다.

```json
{
  "compilerOptions": {
    "target": "ES6" // 어떤 버전의 자바스크립트로 컴파일 될 것인지 설정
    // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
  }
}
```

### lib

lib 옵션은 컴파일에 필요한 JavaScript 내장 라이브러리를 지정할 수 있다.  
Math API, document API 등이 그 예시입니다.
이 프로퍼티가 지정되어 있지 않다면 target 프로퍼티에 지정된 버전에 따라 필요한 타입 정의들에 대한 정보가 자동으로 지정됩니다.

- target이 es3이면 디폴트로 lib.d.ts를 사용
- target이 es5이면, 디폴트로 dom, es5, scripthost를 사용
- target이 es6이면, 디폴트로 dom, es6, dom.iterable, scripthost를 사용

```json
{
  "compilerOptions": {
    "lib": ["es5", "es2015.promise", "dom"] // 컴파일 과정에 사용될 라이브러리 파일 설정
    /*
    es5: global type을 사용하기 위함 (Array, Boolean, Function 등..)
    es2015.promise: Promise를 사용하기 위함
    dom: setTimeout, console.log 등 dom에서 지원해주는 기능들을 사용하기 위함
    dom.iterable : 반복 가능한 DOM 컬렉션에 대한 기능을 추가합니다.
    esnext : ECMAScript의 최신 버전인 ESNext에서 도입된 기능을 사용할 수 있도록 해줍니다.
    */
  }
}
```

### module / moduleResolution

프로그램에서 사용할 모듈 시스템을 결정한다.

### baseURL / paths

import 구문의 모듈 해석 시에 기준이 되는 경로를 지정한다.  
baseUrl 속성에 기본 경로를 설정해 주고 그 바로 아래에 paths 속성에 절대 경로를 지정하고 싶은 경로들을 지정해 주면 된다. 추가적으로 outDir 도 설정해준다.

```json
{
  "compilerOptions": {
    "baseUrl": "./", // 절대 경로 모듈이 아닌, 모듈이 기본적으로 위치한 디렉토리 설정
    "paths": {
      // 'baseUrl'을 기준으로 상대 위치로 가져오기를 다시 매핑하는 항목 설정
      "@components/*": [
        "src/components/*" // import {} from '@components/파일' 할때 어느 경로로 찾아들어갈지 paths 설정
      ],
      "@utils/*": ["src/utils/*"]
    },
    "outDir": "./dist" // 컴파일할때 js 파일 위치 경로 지정
  }
}
```

그러나 실제 ts-node를 이용해 소스 파일을 실행해보면 오류가 난다. 왜냐하면 tsconfig.json 설정은 경로 alias만 준것이지 실제 경로를 바꾼게 아니기 때문이다.

따라서 별도로 tsconfig-paths와 tsc-alias 라는 모듈이 설치해주어야 한다.

#### tsconfig-paths

tsconfig.json 내에 baseurl 이나 paths필드에 명시된 모듈을 실제로 호출하게 도와주는 라이브러리

#### tsc-alias

js 파일로 컴파일되면 baseurl 이나 paths 필드로 명시된 경로가 그대로 트랜스파일링 되서 js 모듈을 인식할수 없는 현상이 일어나게 되는데, 이 패키지를 이용하면 컴파일된 경로를 상대경로로 바꿔서 해결이 가능하다.

```json
{
  "compilerOptions": {
    "baseUrl": "./"
    "path": ...
  },

  // 전역 속성으로 추가해준다.
  "ts-node": {
    "require": ["tsconfig-paths/register"]
  }
}
```

배포환경의 빌드 같은 경우, 컴파일 명렁어를 다음과 같이 하면 됩니다.

```
> tsc && tsc-alias
```

```json
"scripts": {
  "build": "tsc --project tsconfig.json && npx tsc-alias -p tsconfig.json",
}
```

# Craco (Create-React-App Configuration Override)

- [ref](https://craco.js.org/docs/)

CRA에 config 설정을 덮어쓰기 위한 패키지이다.

Webpack의 번거로운 설정을 줄이기 위해 다양한 오버라이딩 패키지들이 등장했으며, 대표적으로 Craco, react-app-rewired 등이 있다.

```
npm install @craco/craco --save
yarn add @craco/craco
```

```json
// package.json
{
  "scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "craco eject"
  }
}
```

**craco.config.js**

```js
const path = require('path');

module.exports = {
  webpack: {
    alias: {
      // 절대경로 설정
      '@': path.resolve(__dirname, 'src'),
    },
  },
};
```

위와같이 설정하면 따로 ts에서 tsconfig-paths같은 라이브러리를 세팅해주지 않아도 절대경로 설정이 가능합니다.

**tsconfig.json**

```json
{
  "baseUrl": ".",
  "paths": {
    "@/*": ["./src/*"]
  }
}
```

## 왜 필요한가

CRA는 기본적으로 프로젝트 디렉토리를 간결하게 유지하기 위해 웹팩 설정이나 script 폴더들을 숨겨놓는다.

하지만, 이를 커스텀해야 할 경우 eject 명령어를 통해 이를 추출할 수 있다.

이렇게 추출된 폴더 및 파일들은 이제 디렉토리 상에 노출되며, 단 eject를 한 번 하면 이전 상태로 돌아갈 수 없다는 단점이 있다.
