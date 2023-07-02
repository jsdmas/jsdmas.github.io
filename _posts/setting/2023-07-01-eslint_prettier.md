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
