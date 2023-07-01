---
title: 'Git branch 정리'
toc: true
toc_sticky: true
toc_label: '목록'
categories:
  - git
tag:
  - git
last_modified_at: 2023-07-01
---

# git checkout -b <브랜치명>

하나의 명령어로 브랜치를 만들고 바로 이동 가능합니다.

# git switch <브랜치이름>

브랜치를 전환합니다.

**특정 브랜치나 커밋에서 새로운 브랜치를 만들고 싶으면 브랜치 이름 뒤에 커밋을 지정**해주면 됩니다.

```
# 커밋515c633a 위치에서 새로운 브랜치 'new-branch2'를 만든다.
$ git switch -c new-branch2 515c633a
```

# git merge <브랜치이름>

```
# <브랜치이름>에 merge하는게 아닌, 현재 브랜치 이곳에 <브랜치이름>을 merge하는 것입니다.
git merge <브랜치이름>
```

이후, 헷갈리지 않도록 되도록 브랜치를 삭제해주는 것이 좋습니다.

```
git branch -D <제거할 브랜치이름>
```

## Merge시 주의점

Merge시, **같은 파일 같은 라인에 수정**을 한 두 브랜치를 병합하려면 **Conflict**가 발생하게 됩니다.  
파일 둘 중 1개를 선택해서 다시 Merge를 수행해주어야 하니, 되도록 같은 파일을 다른 브랜치가 작업하게 만드는 것은 지양해야 합니다.
