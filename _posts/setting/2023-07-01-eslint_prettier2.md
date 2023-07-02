---
title: 'eslint, prettier 설정파일 설명'
toc: true
toc_sticky: true
categories:
  - setting
tags:
  - setting
last_modified_at: 2023-07-01
---

# prettier 설정

```json
{
  "printWidth": 100, // 줄 바꿈할 길이
  "semi": true, // 모든 구문 끝에 세미콜론 출력
  "singleQuote": true, // 쌍따옴표 대신 홑따옴표 사용
  "trailingComma": "all", // 가능하면 후행 쉼표 사용
  "tabWidth": 2, // 들여쓰기 공백 수
  "bracketSpacing": true, // 객체 괄호에 공백 삽입
  "endOfLine": "auto", // OS에 따른 코드라인 끝 처리 방식 사용
  "useTabs": false // 탭 대신 공백으로 들여쓰기
}
```

[다른 option들](https://prettier.io/docs/en/options.html)

# eslint 설정

1. 프로젝트 루트 폴더 위치에서 터미널에 `npm init @eslint/config` 명령어를 입력하여 `ESLint` 설정을 시작합니다. (이때, `package.json`파일이 미리 생성되어 있어야합니다.)
2. 기본적인 `ESLint` 설정을 어떻게 할지 질문을 여러 가지 할 텐데 답변은 아래와 같다.

How would you like to use ESLint?

- To check syntax, find problems, and enforce code style

What type of modules does your project use?

- CommonJS와 ES modules 중 선호하는 모듈 시스템을 선택 (우아한테크코스 미션들의 모듈 시스템은 CommonJS)

Which framework does your project use?

- 프로젝트가 사용하고 있는 프레임워크를 선택

Does your project use TypeScript?

- 프로젝트가 TypeScript를 사용하는지 여부를 선택

Where does your code run?

- Browser와 Node 중 코드가 실행되는 환경을 선택 (우아한테크코스 미션들의 실행 환경은 Node, 중복 선택도 가능)

How would you like to define a style for your project?

- Use a popular style guide

Which style guide do you want to follow?

- 선호하는 스타일 가이드 선택 (우아한테크코스의 자바스크립트 스타일 가이드 기준은 Airbnb)

What format do you want your config file to be in?

- JavaScript, YAML, JSON 중 선호하는 설정 파일의 포맷을 선택
  Would you like to install them now? (peerDependencies 설치)
- Yes
  Which package manager do you want to use?
- npm, yarn, pnpm 중 선호하는 패키지 매니저를 선택

3. 몇 가지 패키지들이 설치되고 프로젝트 루트 폴더에 `.eslintrc.js`(설정 파일 포맷을 `JavaScript`로 하였을 떄 )파일이 생성됩니다.

# VSCode에서 ESLint, Prettier 설정하기

기본적인 `ESLint`, `Prettier`설정을 마쳤지만 `VSCode`에서 `ESLint`와 `Prettier`을 온전히 사용하기 위해서는 `VSCode`에서 추가적인 설정들을 해주어야 합니다.

1. `VSCode`의 `Extenstions`탭에서 `ESLint`와 `Prettier`확장 프로그램을 설치합니다.
2. `VSCode`의 `setting.json` 파일에 아래 내용을 작성합니다.

```json
// 파일을 저장할 때마다 `eslint` 규칙에 따라 자동으로 코드를 수정
"editor.codeActionsOnSave": { "source.fixAll.eslint": true },
// `prettier`를 기본 포맷터로 지정
"editor.defaultFormatter": "esbenp.prettier-vscode",
// 파일을 저장할 때마다 포매팅 실행
"editor.formatOnSave": true,
```

이제 파일을 저장할 때마다 `ESLint`규칙에 따라 자동으로 코드가 수정되고 `Prettier`설정에 따라 자동으로 파일이 포매팅 됩니다.

# eslintrc

eslint의 설정 파일 이름은 항상 `.eslintrc`가 되어야하며, 원하는 포맷에 따른 파일 확장자를 사용해야 합니다.

### root 옵션

ESLint는 현재 린트(lint)대상의 파일이 위치한 폴더 안에 설정파일이 있는지 우선적으로 확인하고 없으면 그 상위 폴더를 한 단계 씩 거슬러 올라가며 설정 파일을 찾게 됩니다.

`root`옵션이 `true`로 설정되어 있는 설정 파일을 만나면 더 이상 상위 폴더로 올라가지 않는다.

```json
{
  "root": true
}
```

### plugins 옵션

ESLint에는 기본으로 제공되는 규칙(rule) 외에도 추가적인 규칙(rule)을 사용할 수 있도록 만들어주는 다양한 플러그인(plugin)이 있다.

플러그인은 설정 파일의 `plugins`옵션을 통해 설정 가능하다.  
불러오기(import), React와 관련된 규칙등을 추가할 수 있다.

```json
"plugins": [
    "react",
    "@typescript-eslint",
    "react-hooks",
    "import",
    "simple-import-sort",
    "prettier",
    "jsx-a11y"
  ]
```

ESLint 플러그인의 npm 패키지 이름은 `eslint-plugin-`로 시작된다.

플러그인을 설정할 떄 흔히 오해하게 되는 부분이 단순히 플러그인만 추가해주면 관련 규칙이 바로 활성화 되진 않는다.

플러그인은 새로운 규칙을 단순히 설정이 가능한 상태로 만들어주기만 한다.

규칙을 위반하면 오류(error)를 낼지 경고(warn)를 낼지 아니면 해당 규칙을 끌지(off)에 대해서는 `extends`옵션이나 `rules`옵션을 통해 추가 설정을 해줘야 한다.

### extends 옵션

확장이 가능한 ESLint 설정은 npm 패키지 이름이 `eslint-config-`로 시작하며 `extedns` 옵션에 명시할 때는 앞 부분을 생략해도 무방합니다.

```json
"extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:jsx-a11y/recommended",
    "prettier/prettier",
    "plugin:import/recommended"
  ]
```

### rules 옵션

`rules`옵션은 규칙 하나 하나를 세세하게 제어하기 위해서 사용됩니다.

일반적으로 `extends`옵션을 통해 설정된 규칙을 덮어쓰고 싶을 떄 유용하게 쓸 수 있습니다.

### env 옵션

ESLint는 기본적으로 미리 선언하지 않고 접근하는 변수에 대해서 오류를 내기 떄문에 각 실행 환경(runtime)에서 기본적으로 제공되는 전역 객체에 대해서 설정을 통해 알려줘야한다.

이러한 역할을 실행 파일의 `env`옵션이 담당합니다.

예를 들어, ESLint로 린트(lint)를 할 자바스크립트 코드가 브라우저에서 실행될 수도 있고, NodeJS에서도 실행될 수 있다면, 두 가지 실행 환경에서 접근 가능한 모든 전역 객체를 다음과 같이 등록해줄 수 있습니다.

```json
{
  "env": {
    "browser": true,
    "node": true
  }
}
```

### parser & paserOptions 옵션

자바스크립트의 확장 문법이나 최신 문법으로 작성한 코드를 린트(lint)하기 위해서는 그에 상응하는 파서(parser)를 사용하도록 설정해줘야 합니다.

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 11
  }
}
```

### settings 옵션

일부 ESLint 플러그인은 추가적인 설정이 가능하다. 이런 경우에는 설정 파일의 `settings`옵션을 사용합니다.

# ref

[참고한 글](https://velog.io/@2wndrhs/%EC%95%8C%EC%95%84%EB%91%90%EB%A9%B4-%EC%93%B8%EB%8D%B0%EC%9E%88%EB%8A%94-ESLint-Prettier-%EC%84%A4%EC%A0%95-%EB%B0%A9%EB%B2%95)  
[https://poiemaweb.com/eslint](https://poiemaweb.com/eslint)  
[https://www.daleseo.com/eslint-config/](https://www.daleseo.com/eslint-config/)

```json
{
  "env": {
    "browser": true,
    "es2021": true,
    "commonjs": true
  },

  "parser": "@typescript-eslint/parser",

  "plugins": ["react", "@typescript-eslint", "react-hooks", "import", "simple-import-sort", "prettier", "jsx-a11y"],

  "extends": ["eslint:recommended", "plugin:react/recommended", "plugin:@typescript-eslint/recommended", "plugin:jsx-a11y/recommended", "prettier/prettier", "plugin:import/recommended"],

  "rules": {
    "react/react-in-jsx-scope": "off",

    "import/no-anonymous-default-export": "off",
    "import/newline-after-import": "error",
    "import/first": "error",
    "import/no-unresolved": "off",

    "simple-import-sort/imports": "error",

    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",

    "prettier/prettier": "error",

    "no-multiple-empty-lines": ["error", { "max": 1 }],
    "eol-last": ["error", "always"], // 파일의 마지막에 줄바꿈을 넣을 것인가?
    "comma-dangle": ["error", "always-multiline"], // 후행쉼표
    "object-curly-spacing": ["error", "always"], // 중괄호 앞뒤의 공백 여부
    "space-in-parens": ["error", "never"], // 소괄호 앞뒤의 공백 여부
    "computed-property-spacing": ["error", "never"], // object의 key 부분에 공백 여부
    "comma-spacing": ["error", { "before": false, "after": true }], // comma(,) 앞 뒤의 공백 여부
    "quotes": ["error", "single"], // 홑따옴표 혹은 겹따옴표 사용 여부
    "no-tabs": ["error", { "allowIndentationTabs": true }], // tab 사용 여부
    "semi": ["error", "always"], // 세미콜론 사용 여부
    "object-shorthand": "error", // es6에서 객체를 간편하게 사용하는 문법

    // blankline을 삽입하는 기준
    "padding-line-between-statements": [
      "error",
      { "blankLine": "always", "prev": "*", "next": "return" },
      { "blankLine": "always", "prev": ["const", "let", "var"], "next": "*" },
      {
        "blankLine": "any",
        "prev": ["const", "let", "var"],
        "next": ["const", "let", "var"]
      }
    ]
  },

  "settings": {
    "import/resolver": {
      "typescript": {}
    },
    "import/parsers": {
      "@typescript-eslint/parser": [".ts", ".tsx"]
    }
  }
}
```
