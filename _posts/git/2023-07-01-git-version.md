---
title: 'Git 버전 관리'
toc: true
toc_sticky: true
toc_label: '목록'
categories:
  - git
tag:
  - git
last_modified_at: 2023-07-01
---

# git init

- 깃 저장소를 초기화한다.
- 저장소나 디렉토리 안에서 이 명령을 실행하기 전까지는 그냥 일반 폴더이다.
- 이것을 입력한 후에야 추가적인 깃 명령어들을 줄 수 있다.

```
# 버전 관리를 하고자 하는 폴더 경로로 가서 .git 폴더 저장소를 추가하여, 버젼관리 시작
$ git init

# 초기 설정
$ git config --global user.name "본인의깃닉네임"
$ git config --global user.email "본인의깃이메일"
```

# git 스테이징

## git add

- 이 명령이 저장소에 새 파일들을 추가하진 않는다.
- 대신, 깃이 파일들을 지켜보게 한다.
- 파일을 추가하면, 깃의 저장소 스냅샷에 포함된다.

```
# git add명령어로 커밋할(깃에 반영할) 내용들을 추가할 수 있으며 이를 스테이징 이라고 한다
git add 파일명

# --all . *  옵션 모두 모든 변경사항을 반영한다는 의미
git add --all
git add .
git add *
```

## git status

- git 워킹 트리의 상태를 보는 명령
- 워킹 트리가 아닌 폴더에서 실행하면 에러 발생

```
$ git status
# 저장소 상태를 체크한다.
# 어떤 파일이 저장소 안에 있는지, 커밋이 필요한 변경사항이 있는지, 현재 저장소의 어떤 브랜치에서 작업하고 있는지 등을 볼 수 있다.

$ git status -s
# git status와 유사하나 워킹 트리의 상태를 요약해서 보여주는 명령
# 변경된 파일이 많을 때 유용
```

## git rm --cached

- git rm \<filename>을 이용하면 **로컬, 원격저장소 모두 파일이 바로 삭제**됩니다.
- --cached 옵션을 사용하면 **로컬에는 파일이 남아있지만 commit하면 원격저장소에서 해당 파일이 삭제**됩니다.

```
git rm --cached <filename>
```

# git 커밋

![](https://github.com/jsdmas/jsdmas.github.io/assets/105098581/ef1c05e9-5b40-4208-82f9-2167f3ab88ab)

## git commit

- 깃의 **의미있는 수정 작업이 끝났을 떄 마침을 알리는 작업**입니다.
- 보통 git commit -m "Message" 형식으로 사용합니다.
  - -m 은 명령어의 다음 부분을 메세지로 남긴다는 뜻입니다.
- 커밋을 한 내용들은 이제 깃이 추적하며 버전관리를 할 수 있도록 도와줍니다.
- 깃은 반영된 소스코드, 리소스 등을 24시간 내내 추적해줍니다.

## git add와 commit 동시에하기

```
# -m : vi에서 별도의 메세지를 작성하지 않고 인라인 형식으로 바로 커밋 메세지를 작성하기 위한 옵션
# -a : 별도의 add 명령어를 사용하지 않고 수정된 파일에 대해 add를 수행하는 옵션
# -am : a 옵션과 m 옵션을 합쳐서 사용하는 방법

$ git commit -am "Initial Commit"
```

# git 기록 보기

## git log

- **커밋 내역을 확인해보고 싶을 때** 사용하는 명령어이다.
- 이를 이용해서 이전 단계로 되돌리거나 버전관리를 할 수 있습니다.

```
git log

# 요약된 로그
git shortlog

# 커밋 로그를 보기 좋게 출력
git log --pretty=online

# git 커밋 그래프를 보여준다
git log --graph
```

# git pull

- 원격 저장소에서 파일 내려받기
- git pull = git fetch + git merge
  - 원격 저장소 커밋과 동기화하고 커밋을 머지 시킨다.
- 원격 저장소와 로컬 저장소의 상태를 같게 만들기 위해 원격 저장소의 소스를 가져오는 것입니다.
- 즉, 다른 사람들의 작업 변경사항을 클라이언트로 내려받기 한다고 보면 됩니다.

# git clone

- 원격 저장소의 저장소를 내 Local에서 이용할 수 있게 **그대로 똑같이 복사해 가져옵니다.**

## git pull vs git clone 차이점

git pull 명령은 원격 저장소에 있는 프로젝트 내용을 가져오는 역할을 합니다.

- git clone 명령을 사용하면 **로컬 저장소의 내용이 원격 저장소의 내용과 일치**해집니다.
- 기존에 작업중이었던 사람이 git 명령을 사용해서 원격 저장소의 내용을 그대로 가져와버리면 기존에 작업했던 내용들은 당연히 없어집니다.
- 즉, git clone은 프로젝트에 처음 투입될 때 사용되어야 하는 명령입니다.

반면, git pull 명령은 원격 저장소의 내용을 가져와서 현재 브랜치와 병합(merge)해주는데, **그 이외의 작업 파일들은 건들지 않는다.**  
git pull 명령은 병합과정도 포함되어 있기 때문에, pull을 하기 전에 commit을 하지 않으면 덮어쓰기 에러가 발생할 수 있다.

# git fetch

- 원격 저장소의 branch와 commit들을 로컬 저장소와 동기화. 단 merge는 안함.
- 옵션을 생략하면 모든 원격 저장소에서 모든 브랜치를 가지고 옵니다.

```
git fetch [원격 저장소명] [브랜치명]
```

# 원격 저장소 연결 및 끊기

```
# 연결되어 있는 원격 레파지토리 확인
git remote -v
origin  https://github.com/jsdmas/hello.git (fetch)
origin  https://github.com/jsdmas/hello.git (push)
```

## 기존 리포지토리 remote 제거

git remote remove \<name> 옵션을 사용해 주면 연결되어 있는 저장소를 간단하게 끊을 수 있습니다.

```
git remote remove origin
```

## 새 리포지토리 remote 추가

```
# git remote add <name> <url>
$ git remote add origin https://github.com/jsdmas/hello.git
```

# 협업 작업시 주의할 사항

팀원과 작업할 때는, 먼저 **fetch와 status를 통해 pull할 내용이 있는지 먼저 확인하는 것이 중요합니다.**

```
git fetch
git status
```

그 후 원격 저장소와 로컬 저장소 commit을 일치시키고 merge 시킵니다.

```
git pull <원격저장소명> <브랜치명>
```

아니면 fetch한 후 merge/저장소명 을 통해 merge한 후 github 페이지에서 pull request를 보내는 방법도 있습니다.
