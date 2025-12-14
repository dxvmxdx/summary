# Contents

1. [Local](#1-local)  
   1-1. [git 초기화와 삭제](#1-1-git-초기화와-삭제)  
   1-2. [add](#1-2-add)  
   1-3. [commit](#1-3-commit)  
   1-4. [diff](#1-4-diff)  
   1-5. [checkout](#1-5-checkout)  
   1-6. [log](#1-6-log)  
   1-7. [tag](#1-7-tag)

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
