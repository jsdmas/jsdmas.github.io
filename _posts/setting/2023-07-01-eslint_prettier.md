---
title: '코드 일관성과 협업을 위한 초기 코드 setting 방법 (eslint, prettier)'
execrpt: 'eslint, prettier 설명'
toc: true
toc_sticky: true
categories:
  - setting
tags:
  - setting
last_modified_at: 2023-07-01
---

# 서론

[ref](https://xiubindev.tistory.com/136)

## 왜 필요한가?

하나의 프로젝트에서 작업자마다 각자 다른 코딩 스타일을 가지고 있습니다.  
그것이 코드에 드러난다면 프로젝트를 제 3자가 읽기도 어려워지며, 팀원들 끼리도 작성한 코드를 읽고 이해하기 힘들어집니다.

**이러한 요소들은 비효율을 유발**하게되고 이를 극복하기 위해 팀으로 작업을 할 때는 코딩 스타일을 일치시키기 위한 `Linter`와 `Code Formatter`를 사용하는 것이 좋습니다.

javaScript는 **Linter로 ESLint를**, **Code Formatter 로는 Prettier**를 사용하는 것이 일반적입니다.

# ESLint

- 일관되고 버그를 피할수 있는 코드를 짜기위해서 만들어진 코드 분석 툴
- 작성된 코드의 구문을 분석하여 버그가 발생할 여지가 있거나, 불필요한 코드, 혹은 보안상 위험한 코드 등에 대한 경고를 띄워줍니다.
- 설정 커스터마이징을 허용해주기 때문에 필요한 규칙들을 커스텀해서 적용가능합니다.

# Prettier

- 코드 포맷팅 도구로, 포맷팅 Rule을 커스터마이징할 수 있습니다.
- 코드의 포맷팅을 툴로 사용함으로서 팀원 모두가 같은 포멧팅스타일을 공유할 수 있게 됩니다.
- 이로인해 개발자는 다소 중요하지 않은 요소들에 에너지를 낭비하지 않고 핵심적인 개발에 집중할 수 있게 됩니다.

# 설치

## eslint

```
npm install eslint --save-dev
```

CRA의 경우 내장되어 있기 떄문에 따로 설치하지 않아도 됩니다.

## prettier

```
npm install prettier --save-dev
```

## eslint-config-prettier

- eslint는 linting 기능을, prettier는 formatting을 담당하는 구조가 이상적입니다.
- 하지만, eslint에는 일부 formatting 관련된 rule도 포함되어 있습니다.
- 이 rule들이 prettier와 다른 설정을 가지고 있다면 설정 충돌이 발생합니다.
- 따라서, eslint에서 formatting 관련 rule들을 모두 해제해야할 필요가 있습니다.
  - 수동으로 진행할 수도 있지만, 이를 적용해주는 eslint plugin이 존재합니다.
    ```
    npm install eslint-config-prettier --save-dev
    ```

## optional

패키지를 설치하면 터미널에서 명령어를 통해 eslint, prettier를 실행 가능합니다.

IDE에서는 일반적으로 터미널 명령어로 실행하는 것 뿐만 아니라,  
에디터 차원에서 파일을 저장할 때 formatting을 적용해주고,  
에디터에서 eslint의 메세지들을 확인할 수 있게 해주는 기능들을 플러그인 형태로 제공하기에 원할시 사용 가능합니다.

# 설정

- [prettier](https://prettier.io/docs/en/options.html)
- [ESLint](https://eslint.org/docs/latest/)

# Husky

- [husky](https://typicode.github.io/husky/#/?id=articles)

- git hook 설정을 도와주는 npm package
- 번거로운 git hook 설정이 편하고 npm install 과정에서 사전에 세팅해둔 git hook을 다 적용시킬 수 있어서 모든 팀원이 git hook 실행이 되도록 하기가 편합니다.
- husky를 통해 pre-commit, pre-push hook을 설정 가능합니다.

## 사용배경

- eslint, prettier를 도입하면 끝이아닙니다.
- 아무리 패키지를 설치하고, 룰을 설정해도 작업자가 사용을 안하면 효과가 없습니다.
- 하지만 개인이 매번 확인해서 실행하는 것은 실수가 발생할 여지가 있으며 강제성이 없습니다.
- commit된 코드는 무조건 formatting이 완료되어야 하고, push된 코드는 무조건 eslint가 pass된 상태에서 push가 되도록 자동화를 구축해야 합니다.

## 자동화

- git hook 도입
  - git에서 특정 이벤트 발생하기 전, 후로 특정 hook 동작을 실행할 수 있게 하는것 (ex : commit, push)
- git hook 설정은 까다롭고, 모든 팀원들이 사전에 repo를 클론받고 메뉴얼하게 사전 과정을 수행해야지만 hook이 실행됨을 보장할 수 있습니다.
- 이를 더 쉽게 하기위해 Husky를 사용합니다.

## 적용

```
npm install husky --save-dev
```

- 처음 husky를 세팅하는 사람만 실행이 필요합니다. `npx husky install`
  - `npx husky install` : husky에 등록된 hook을 실제 .git에 적용시키기 위한 스크립트
  - add postinstall script in package.json
    - 이후 clone 받아서 사용하는 사람들은 npm install 후 자동으로 `husky install`이 될 수 있도록 하는 설정
  ```json
  {
    // package.json
    "scripts": {
      "postinstall": "husky install"
    }
  }
  ```
- scripts 설정

```json
// package.json
{
  "scripts": {
    "postinstall": "husky install",
    "format": "prettier --cache --write .",
    "lint": "eslint --cache ."
  }
}
```

### lint-staged

[lint-staged](https://github.com/okonet/lint-staged#reformatting-the-code)

위의 과정을 통해 git hook을 활용해 commit 하기 전에 lint를 실행해서 코드를 검사할 수 있게되었습니다.  
하지만 검사해야하는 확장자에 해당하는 모든 파일을 검사하려면 굉장히 오랜 시간이 소요될 것입니다.  
이 떄 lint-staged를 함꼐 사용하면 변경된 파일만 검사할 수 있습니다.

즉 ,**git staging area에 올라온 파일만 검사**할 수 있습니다.

#### lint-staged 설정

ESLint를 실행하여 검사하고 싶은 확장자를 설정하고 원하는 옵션도 세팅해서 명령어를 작성하면 된다.

staged에 적용시킬 기능을 추가

```json
{
  "lint-staged": "lint-staged",
  // project 내 모든 tsx,ts,jsx,js파일에 대해 검사하는 설정
  "lint-staged": {
    "src/**/*.{js,jsx,ts,tsx}": ["eslint --fix", "prettier --write"]
  }
}
```

그 후 husky 파일에 pre-commit 스크립트를 생성하기 위해 아래의 명령어를 입력한다.

```
npx husky add .husky/pre-commit 'npm run lint-staged'
```

위의 husky & lint-staged 한방에 하기

```
npx mrm lint-staged
```

mrm : 오픈소스 프로젝트의 환경 설정을 동기화 하기 위한 도구

[참고 글](https://velog.io/@do_dadu/husky-lint-staged%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90-sub-ESLint-%EC%9E%90%EB%8F%99%ED%99%94%ED%95%98%EA%B8%B0)
