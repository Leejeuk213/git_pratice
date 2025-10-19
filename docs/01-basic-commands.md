# Git 기본 명령어 가이드

## 목차
- [Git 설정](#git-설정)
- [저장소 초기화](#저장소-초기화)
- [기본 작업 흐름](#기본-작업-흐름)
- [상태 확인](#상태-확인)
- [커밋 히스토리](#커밋-히스토리)

## Git 설정

### 사용자 정보 설정
```bash
# 전역 사용자 이름 설정
git config --global user.name "Your Name"

# 전역 이메일 설정
git config --global user.email "your.email@company.com"

# 설정 확인
git config --list
```

### 기본 에디터 설정
```bash
# VSCode를 기본 에디터로 설정
git config --global core.editor "code --wait"

# Vim을 기본 에디터로 설정
git config --global core.editor "vim"
```

## 저장소 초기화

### 새 저장소 생성
```bash
# 현재 디렉토리를 Git 저장소로 초기화
git init

# 새 디렉토리를 만들고 Git 저장소로 초기화
git init my-project
```

### 원격 저장소 복제
```bash
# HTTPS를 사용한 복제
git clone https://github.com/username/repository.git

# SSH를 사용한 복제
git clone git@github.com:username/repository.git

# 특정 디렉토리 이름으로 복제
git clone https://github.com/username/repository.git my-folder
```

## 기본 작업 흐름

Git의 기본 작업 흐름은 다음과 같습니다:
1. **작업 디렉토리 (Working Directory)**: 파일 수정
2. **스테이징 영역 (Staging Area)**: 커밋할 변경사항 선택
3. **저장소 (Repository)**: 변경사항 저장

### 파일 추가 (Staging)
```bash
# 특정 파일 추가
git add file.txt

# 여러 파일 추가
git add file1.txt file2.txt

# 모든 변경된 파일 추가
git add .

# 특정 확장자 파일 추가
git add *.js

# 대화형 모드로 추가
git add -i
```

### 변경사항 커밋
```bash
# 커밋 메시지와 함께 커밋
git commit -m "커밋 메시지"

# 스테이징과 커밋을 동시에 (추적 중인 파일만)
git commit -am "커밋 메시지"

# 에디터를 열어 상세한 커밋 메시지 작성
git commit

# 이전 커밋 수정 (메시지 변경 또는 파일 추가)
git commit --amend
```

### 원격 저장소와 동기화
```bash
# 원격 저장소에서 변경사항 가져오기 (병합 안 함)
git fetch origin

# 원격 저장소에서 변경사항 가져오고 병합
git pull origin main

# 로컬 변경사항을 원격 저장소에 푸시
git push origin main

# 강제 푸시 (주의: 원격 저장소 히스토리 덮어씀)
git push -f origin main
```

## 상태 확인

### 작업 상태 확인
```bash
# 현재 상태 확인
git status

# 간단한 형식으로 상태 확인
git status -s

# 브랜치 정보 포함
git status -sb
```

### 변경사항 확인
```bash
# 작업 디렉토리와 스테이징 영역 비교
git diff

# 스테이징된 변경사항 확인
git diff --staged

# 특정 파일의 변경사항 확인
git diff file.txt

# 특정 커밋 간 비교
git diff commit1 commit2
```

## 커밋 히스토리

### 로그 확인
```bash
# 기본 로그 보기
git log

# 한 줄로 요약해서 보기
git log --oneline

# 그래프 형태로 보기
git log --graph --oneline --all

# 최근 n개 커밋만 보기
git log -n 5

# 특정 파일의 히스토리 보기
git log file.txt

# 특정 저자의 커밋만 보기
git log --author="Author Name"

# 날짜별 필터링
git log --since="2024-01-01" --until="2024-12-31"
```

### 특정 커밋 확인
```bash
# 특정 커밋의 상세 정보
git show commit-hash

# 특정 커밋의 변경된 파일만 보기
git show --name-only commit-hash
```

## 파일 제거 및 이동

### 파일 제거
```bash
# Git에서 추적 제거 및 파일 삭제
git rm file.txt

# Git에서만 추적 제거 (파일은 유지)
git rm --cached file.txt

# 디렉토리 제거
git rm -r directory/
```

### 파일 이동/이름 변경
```bash
# 파일 이름 변경
git mv old-name.txt new-name.txt

# 파일 이동
git mv file.txt directory/
```

## 변경사항 취소

### 작업 디렉토리 변경사항 취소
```bash
# 특정 파일의 변경사항 취소
git checkout -- file.txt

# 또는 (Git 2.23 이상)
git restore file.txt

# 모든 변경사항 취소
git checkout -- .
```

### 스테이징 취소
```bash
# 특정 파일의 스테이징 취소
git reset HEAD file.txt

# 또는 (Git 2.23 이상)
git restore --staged file.txt

# 모든 스테이징 취소
git reset HEAD
```

## 원격 저장소 관리

### 원격 저장소 확인
```bash
# 원격 저장소 목록 확인
git remote

# 원격 저장소 URL 확인
git remote -v

# 특정 원격 저장소 상세 정보
git remote show origin
```

### 원격 저장소 추가/제거
```bash
# 원격 저장소 추가
git remote add origin https://github.com/username/repository.git

# 원격 저장소 제거
git remote remove origin

# 원격 저장소 URL 변경
git remote set-url origin https://github.com/username/new-repository.git
```

## 실용적인 팁

### .gitignore 사용
특정 파일이나 디렉토리를 Git에서 무시하려면 `.gitignore` 파일을 생성합니다:

```bash
# .gitignore 예시
*.log
node_modules/
.env
.DS_Store
```

### Alias 설정
자주 사용하는 명령어를 단축키로 설정:

```bash
# 유용한 alias 설정
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --graph --oneline --all"
```

### Stash (임시 저장)
```bash
# 현재 작업 임시 저장
git stash

# 메시지와 함께 저장
git stash save "작업 중인 내용"

# stash 목록 확인
git stash list

# 가장 최근 stash 적용
git stash apply

# 가장 최근 stash 적용 후 삭제
git stash pop

# 특정 stash 적용
git stash apply stash@{0}

# stash 삭제
git stash drop stash@{0}

# 모든 stash 삭제
git stash clear
```

## 다음 단계

기본 명령어를 숙지했다면, 다음 가이드로 넘어가세요:
- [브랜치 관리 가이드](./02-branch-management.md)
- [Conflict 해결 가이드](./03-conflict-resolution.md)
