---
title: 'Git add/commit/push 취소하기'
toc: true
toc_sticky: true
toc_label: '목록'
categories:
  - git
tag:
  - git
last_modified_at: 2023-07-01
---

# git add 취소하기 (파일 상태를 Unstage로 변경하기)

실수로 의도치않은 파일까지 Staging Area에 넣은 경우, staging Area(git add 명령 수행한 후의 상태)에 넣은 파일을 빼고 싶을 때가 있습니다.

## git reset HEAD

- HEAD가 가리키는 시점의 버전으로 파일을 unstage하고 되돌린다.

```
# RESET.md 파일을 Unstage로 변경
git reset HEAD RESET.md

# 전체 취소
git reset HEAD
```

## git restore --staged

```
git restore --staged 파일명
```

# git commit 취소하기

## git reset HEAD^ (단계로 commit 취소)

- 커밋 기록 자체를 말소
- 꺽쇠 수만큼 이전으로 돌아가게 하는 명령
- ^(한단계 앞)
- ^^(두단계 앞)
- ~숫자 로도 가능

```
# [방법 1]
# commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
$ git reset --soft HEAD^


# [방법 2]
# commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
$ git reset HEAD^
$ git reset HEAD~1 # 마지막 commit을 취소. 하나를 되돌림

$ git reset HEAD^^
$ git reset HEAD~2 # 마지막 2개의 commit을 취소


# [방법 3]
# commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
$ git reset --hard HEAD^
```

## git reset 옵션

- -soft : index 보존(add한 상태, staged 상태), 워킹 디렉터리의 파일 보존. 즉, 모두 보존합니다.
- -mixed : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 보존 (기본 옵션)
- -hard : index 취소(add하기 전 상태, unstaged 상태), 워킹 디렉터리의 파일 삭제. 즉, 모두 취소. **작업내용이 모두 사라지니까 사용X**

## git reset \<num> --hard (이전 commit 상태를 지정해 리셋)

- git reset HEAD로 하면 되돌아갈 위치를 숫자로 세어서 해야되는데, 해시아이디로 지정해서 커밋을 돌아간다.  
  ![](https://github.com/jsdmas/jsdmas.github.io/assets/105098581/56532d2b-0956-4253-a698-45971f21910d)

```
# git log를 통해 확인한 커밋번호의 앞 6자리를 이용해 이전 상태로 되돌릴 수 있다.
# git reset 커밋번호여섯자리 --hard
$ git reset a6f30a --hard
```

## git revert \<num> --hard (이전 commit 상태로 되돌리기)

이전 상태로 되돌리는 방법에는 두가지가 있습니다.

- rest은 로그를 아예 지워버리기 때문에 미래로 되돌릴 수 없다.
- 하지만 revert는 로그를 덮어쓰는 것이므로 **다시 미래로 되돌릴 수가 있다.**

```
# [ git reset / revert 차이 ]

$ git reset <이전 커밋번호여섯자리>
# -- <이전 커밋번호여섯자리>로 리셋해라 (삭제)

$ git revert <현재 커밋번호여섯자리>
# -- <현재 커밋번호여섯자리> 에서 이전으로 되돌아가라 (생성 덮어쓰기)
```

> 협업시에는 revert를 자주 이용해야합니다.

## git commit --amend (commit message 변경하기)

commit message를 잘못 적은 경우, message를 변경할 수 있다.

```
git commit -amend
```

## 특정 commit 버젼 상태로 되돌리기

```
$ git restore --source=해시코드 파일명
$ git restore --source=HEAD~2 파일명 # HEAD와 같은 포인터도 사용가능
```

# git push 취소하기

이 명령을 사용하면 자신의 local의 내용을 remote에 강제로 덮어쓰기를 하는 것이기 떄문에 주의해야 합니다.

되돌아간 commit 이후의 모든 commit 정보가 사라지기 때문에 주의해야 합니다.  
특히, 협업 프로젝트에서는 동기화 문제가 발생할 수 있으므로 팀원과 상의 후 진행하는 것이 좋습니다.

1. 워킹 디렉터리에서 commit을 되돌린다.

```
# [ 최근의 commit을 취소하고 워킹 디렉터리를 되돌리는 법 ]
$ git reset HEAD^
```

```
# [ 원하는 시점으로 워킹 디렉터리를 되돌리는 법 ]

# Reflog(브랜치와 HEAD가 지난 몇 달 동안에 가리켰었던 커밋) 목록 확인
$ git reflog
# 또는
$ git log -g

# 원하는 시점으로 워킹 디렉터리를 되돌린다.
$ git reset HEAD@{number}
# 또는
$ git reset [commit id]
```

> 협업시 git reset 명령어로 작업을 취소하면 작업하던 사람들 부분이 꼬여버리기 떄문에 반드시 git revert 명령어로 수행하는것이 좋다. 2. 되돌려진 상태에서 다시 commit을 한다.

```
$ git commit -m "new commit messages"
```

3. 원격 저장소에 강제로 push 한다.

```
$ git push origin [branch name] -f
# 또는
$ git push origin +[branch name]

# master branch를 원격 저장소(origin)에 강제로 push
$ git push origin +master
```
