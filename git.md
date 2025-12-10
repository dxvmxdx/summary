# Contents

1. [Local](#1-local)  
   1-1. [git 초기화와 삭제](#1-1-git-초기화와-삭제)  
   1-2. [add](#1-2-add)

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
git rm file.txt
git rm --cached file.txt # staging area에서 삭제되고 untracked 상태로 돌아감

# 파일 이름 변경 및 이동 (후 자동으로 staging area에 추가)
git mv 파일A 파일B
git mv from.text /logs/from.text
```

<br>
<br>
<br>
