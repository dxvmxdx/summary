# Contents

1. [Local](#1-local)  
   1-1. [git 초기화와 삭제](#1-1-git-초기화와-삭제)  
   1-2. [add](#1-2-add)  
   1-3. [commit](#1-3-commit)  
   1-4. [diff](#1-4-diff)  
   1-5. [checkout](#1-5-checkout)  
   1-6. [log](#1-6-log)  
   1-7. [tag](#1-7-tag)  
   1-8. [stash](#1-8-stash)
2. [Branch](#2-branch)  
   2-1. [merge](#2-1-merge)  
   2-2. [rebase](#2-2-rebase)  
   2-3. [cherry-pick](#2-3-cherry-pick)
3. [undo](#3-undo)  
   3-1. [file](#3-1-file)  
   3-2. [commit](#3-2-commit)

<br>
<br>
<br>
<br>
<br>

# 1. Local

## 1-1. git 초기화와 삭제

```shell
# 초기화
git init

# 삭제
rm -rf .git
```

<br>
<br>
<br>

## 1-2. add

working directory에서 작업하고 있는 파일을 staging area에 추가한다.

```shell
git add *.txt # .txt 확장자를 가진 모든 파일
git add * # 디렉토리 내 모든 파일 (삭제된 파일과 숨김 파일 제외)
git add . # 디렉토리 내 모든 파일 (.gitignore에 있는 파일명 제외)

# 파일 삭제 (후 자동으로 staging area에 추가)
git rm <파일명>
git rm --cached <파일명> # staging area에서 삭제되고 untracked 상태로 돌아감

# 파일 이름 변경 및 이동 (후 자동으로 staging area에 추가)
git mv 파일A 파일B
git mv from.text /logs/from.text
```

<br>
<br>
<br>

## 1-3. commit

버전 등록

```shell
git commit # staged 파일
git commit -m <msg>
```

<br>
<br>
<br>

## 1-4. diff

변경사항을 보여준다.

```shell
# modified ⚖️ 최신 커밋
# staged가 있으면 modified ⚖️ staged
git diff

# staging area ⚖️ 최신 커밋
git diff --staged # git diff --cached

# 두 커밋된 버전의 변경사항을 보여줌
git diff 해시코드 해시코드

# 두 커밋된 버전에서 해당 파일의 변경사항을 보여줌
git diff 해시코드 해시코드 <파일명>
```

<br>
<br>
<br>

## 1-5. checkout

브랜치를 전환하거나 modified를 최신 커밋 상태로 복원할 때 사용한다.  
최신 git 버전에서, 브랜치 전환은 `switch`로 파일 복원은 `restore`로 사용을 권장한다.

```shell
git checkout main # git switch main
git checkout <commit> # 해당 버전으로 HEAD 이동
# 브랜치 생성 후 이동
git checkout -b <브랜치명> # git switch -C <브랜치명>

git checkout -- <파일명> # git restore <파일명>
```

\*`HEAD`: 지금 바라보고 있는 버전을 가리킨다.

<br>
<br>
<br>

## 1-6. log

커밋 로그를 보여준다.

```shell
git log <파일명>
git log -p <파일명> # 해당 파일이 포함된 버전 각각의 정보와 변경사항을 보여준다.


# git show: 최신 커밋의 정보와 변경사항을 보여준다.
git show <commit>
git show <commit> <파일명>
```

<br>
<br>
<br>

## 1-7. tag

보통 릴리즈할 때 사용하며, 특정 버전에 태그를 추가해 관리할 수 있다.

```shell
# 조회
git tag # 모든 태그 목록
git tag -l "v1.0.*" # 와일드카드를 사용하여 태그 목록 검색

# 추가 (annotated 방식 권장)
git tag <name> # lightweight 방식
git tag -a <name> <commit>
git tag -a <name> -m <msg>

# 삭제
git tag -d <name>

# remote
# 자동으로 remote에 태그를 전송하지 않으므로, 별도로 push 해줘야 한다.
git push origin <name>
git push origin --tags # 모든 태그
git push origin -d <name> # 삭제
```

<br>
<br>
<br>

## 1-8. stash

modified이면서 tracked 상태인 파일과 staging area에 있는 파일들을 저장할 수 있다.

```shell
git stash
git stash --keep-index # modified이면서 tracked 상태인 파일만 저장
git stash -u # untracked 상태의 파일까지 저장
git stash list # 목록 확인

# 적용
git stash apply # 최근 stash (스택에는 남아있음)
git stash pop # 최근 stash (스택에서 사라짐)
git stash apply <stash>
git stash apply --index # staged 상태까지 적용
git stash branch <branch> # 브랜치를 생성하고 stash 적용 후 삭제

# 삭제
git stash drop <stash>
git stash clear # 전부 삭제
```

<br>
<br>
<br>
<br>
<br>

# 2. Branch

```shell
git branch # 목록 확인
git branch <name> # 생성
git branch -d <name> # 삭제

git switch <브랜치명> # 이동
git switch -C <브랜치명> # 브랜치 생성 후 이동
```

<br>
<br>
<br>

## 2-1. merge

```shell
git merge <branch>
```

<br>

### 💥 conflict 해결

conflict가 발생한 파일에서 수동으로 해결 후 이어 진행한다.

```shell
git add <파일명>
git merge --continue

# 또는 되돌릴 수도 있다.
git merge --abort
```

<br>
<br>
<br>

## 2-2. rebase

rebase를 이용하면 깔끔한 히스토리 관리를 할 수 있다.  
다음과 같은 상황에서 merge하게 되면 3-way merge가 되는데, rebase를 이용해 좀 더 깔끔한 히스토리를 만들 수 있다.

```
          A---B---C topic
         /
    D---E---F---G main
```

먼저, topic 브랜치를 main 브랜치로 rebase 한다.

```shell
git switch topic
git rebase main
```

```
                  A'--B'--C' topic
                 /
    D---E---F---G main
```

이제 main 브랜치에 merge 한다.

```shell
git switch main
git merge topic
git branch -d topic
```

<br>

> ⚠️ rebase된 커밋은 기존 커밋과 다르므로, 이미 서버에 push된 커밋은 rebase 하면 안된다.

<br>

### --onto 옵션

```
    o---o---o---o---o  main
         \
          o---o---o---o---o  next
                           \
                            o---o---o  topic
```

위 상황에서 topic 브랜치만 main 브랜치에 merge하고 싶을 때, `--onto` 옵션을 사용하면 된다.

```shell
git rebase --onto main next topic
```

```
    o---o---o---o---o  main
        |            \
        |             o'--o'--o'  topic
         \
          o---o---o---o---o  next
```

이후 main 브랜치에 merge 한다.

```shell
git switch main
git merge topic
git branch -d topic
```

<br>
<br>
<br>

## 2-3. cherry-pick

다른 브랜치에 있는 특정 커밋의 변경사항을 가져와, 현재 HEAD가 가리키는 브랜치에 추가한다.

```shell
git cherry-pick <commit>
```

```
              Q--R
             /
    A---B---C---D  main
         \
          X---Y---Z---R'
```

<br>
<br>
<br>
<br>
<br>

# 3. Undo

> ⚠️ 이미 서버에 push된 커밋은 수정하면 안된다.

<br>

## 3-1. file

```shell
# 최근 커밋 상태로 초기화
git reset --hard HEAD

# 모든 untracked 삭제
git clean -fd

# modified >> unmodified (staged된 부분은 제외)
git restore .
git restore <파일명> # git checkout -- <파일명>

# staged >> unstaged
git restore --staged .
git restore --staged <파일명>
git reset HEAD .
git reset HEAD <파일명>

# 파일을 특정 커밋의 내용으로 복원
git restore --source=<commit> <파일명>
```

<br>
<br>
<br>

## 3-2. commit

### --amend (최근 커밋 수정)

> ⚠️ 수정된 커밋은 기존 커밋과 다르다.

```shell
# 수정 후
git add .
git commit --amend

# 커밋 메시지 수정
git commit --amend -m
```

<br>

### reset

특정 커밋으로 되돌린다.

| 옵션                        | HEAD | staging area | working directory |
| --------------------------- | :--: | :----------: | :---------------: |
| `git reset --soft <commit>` | YES  |      NO      |        NO         |
| `git reset <commit>`        | YES  |     YES      |        NO         |
| `git reset --hard <commit>` | YES  |     YES      |        YES        |

<br>

> 📋 **reflog**  
> HEAD가 가리키는 커밋이 바뀔 때마다 Git은 자동으로 그 커밋이 무엇인지 기록한다.  
> `git reflog` 명령어로 그 이력을 확인할 수 있다.
>
> 이를 이용하여 reset한 커밋을 복구할 수 있다.
>
> ```shell
> git reset --hard <commit>
> ```
