---
title: 'Github 잘못 올라간 파일 삭제'
toc: true
toc_sticky: true
toc_label: '목록'
categories:
  - git
tag:
  - git
last_modified_at: 2023-07-01
---

상황을 예를 들어보면

1. 작업이 완료되어 저장소에 push했습니다.
2. 하지만 모르고 작업에 올리지 않아도 되는 private 폴더를 올려버렸습니다.
3. 그래서 폴더를 삭제하고, 다시 push를 날립니다.
4. 하지만 github에서는 삭제되지 않습니다.

폴더를 리팩토링하고 난 후에도 마찬가지입니다.  
삭제 및 이동을 하고 push를 할 시 github에 있는 폴더는 유지된 채 바뀐 폴더가 새로 생성됩니다.

- 원격 저장소에 이미 파일은 저장되어있습니다.
- 로컬에서 삭제만 한다고 해서 원격 저장소에서 삭제가 이루어지지 않습니다.
- 이 경우 git 명령어를 통한 파일 삭제 후 push를 해줘야합니다.
- push를 원격저장소에서 받으면 로컬 커밋의 내용에 따라 업데이트 하기 때문입니다.

# git rm (파일 remove)

1. 원격 저장소에서 파일 삭제하기.

이미 github remote에 push를 한 상황이라 **로컬 저장소에서 직접 파일을 삭제해도, 원격 저장소에서는 삭제되지 않습니다. 따라서 git 명령어를 통해 명령을 전달하는 방식으로 삭제해야 합니다.**

```
# 원격 저장소와 로컬 저장소에 있는 파일을 삭제한다.
$ git rm [File Name]

# 원격 저장소에 있는 파일을 삭제한다. 로컬 저장소에 있는 파일은 삭제하지 않는다.
$ git rm --cached [File Name]
```

따라서 아래와 같이 git rm --cached \[File Name] 명령어를 이용하여 원격 저장소에서 잘못 올라간 파일을 삭제해야 합니다.

```
# .idea/modules.xml 파일 삭제
$ git rm --cached .idea/modules.xml

# .idea 폴더 하위의 모든 파일 삭제
$ git rm --cached -r .idea/
```

2. 원격 저장소에 적용하기

버전 관리에서 완전히 제외하기 위해서는 반드시 commit 명령어와 push를 수행해야 합니다.

```
# 버전 관리에서 완전히 제외하기 위해서는 반드시 commit 명령어를 수행해야 한다.
$ git commit -m "Fixed untracked files"

# 원격 저장소(origin)에 push
$ git push origin master
```
