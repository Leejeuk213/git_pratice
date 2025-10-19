# Git 브랜치 관리 가이드

## 목차
- [브랜치 기본 개념](#브랜치-기본-개념)
- [브랜치 생성과 전환](#브랜치-생성과-전환)
- [브랜치 병합](#브랜치-병합)
- [브랜치 관리](#브랜치-관리)
- [브랜치 전략](#브랜치-전략)

## 브랜치 기본 개념

브랜치는 독립적인 작업 공간을 제공하여 여러 기능을 동시에 개발할 수 있게 해줍니다.

### 브랜치의 장점
- 메인 코드에 영향을 주지 않고 새로운 기능 개발 가능
- 여러 작업을 동시에 진행 가능
- 실험적인 코드 작성 가능
- 코드 리뷰 프로세스 지원

## 브랜치 생성과 전환

### 브랜치 확인
```bash
# 로컬 브랜치 목록
git branch

# 원격 브랜치 포함 모든 브랜치 목록
git branch -a

# 원격 브랜치만 보기
git branch -r

# 브랜치와 마지막 커밋 메시지 함께 보기
git branch -v

# 병합된/안 된 브랜치 확인
git branch --merged
git branch --no-merged
```

### 브랜치 생성
```bash
# 새 브랜치 생성
git branch feature/new-feature

# 특정 커밋에서 브랜치 생성
git branch feature/new-feature commit-hash

# 원격 브랜치 기반으로 브랜치 생성
git branch feature/new-feature origin/main
```

### 브랜치 전환
```bash
# 브랜치 전환
git checkout feature/new-feature

# 또는 (Git 2.23 이상)
git switch feature/new-feature

# 브랜치 생성과 동시에 전환
git checkout -b feature/new-feature

# 또는 (Git 2.23 이상)
git switch -c feature/new-feature

# 이전 브랜치로 돌아가기
git checkout -

# 원격 브랜치 추적하며 생성
git checkout -b feature/new-feature origin/feature/new-feature
```

## 브랜치 병합

### Fast-Forward 병합
브랜치가 단순히 앞으로만 진행된 경우 (커밋 히스토리가 일직선)

```bash
# main 브랜치로 전환
git checkout main

# feature 브랜치 병합
git merge feature/new-feature
```

```
# Fast-Forward 병합 전
main:    A---B
              \
feature:       C---D

# Fast-Forward 병합 후
main:    A---B---C---D
```

### 3-way 병합
두 브랜치가 각각 다른 커밋을 가지고 있는 경우

```bash
# main 브랜치로 전환
git checkout main

# feature 브랜치 병합 (병합 커밋 생성)
git merge feature/new-feature

# Fast-Forward 방지 (항상 병합 커밋 생성)
git merge --no-ff feature/new-feature
```

```
# 3-way 병합 전
main:    A---B---E
              \
feature:       C---D

# 3-way 병합 후
main:    A---B---E---F
              \     /
feature:       C---D
```

### Squash 병합
여러 커밋을 하나로 합쳐서 병합

```bash
# feature 브랜치의 모든 커밋을 하나로 합쳐서 병합
git merge --squash feature/new-feature
git commit -m "Add new feature"
```

### Rebase
브랜치의 베이스를 변경하여 히스토리를 깔끔하게 유지

```bash
# feature 브랜치에서 실행
git checkout feature/new-feature
git rebase main

# 또는 한 번에
git rebase main feature/new-feature
```

```
# Rebase 전
main:    A---B---E
              \
feature:       C---D

# Rebase 후
main:    A---B---E
                  \
feature:           C'---D'
```

⚠️ **주의**: 이미 푸시한 브랜치는 rebase하지 마세요!

### Interactive Rebase
커밋 히스토리를 수정할 때 사용

```bash
# 최근 3개 커밋 수정
git rebase -i HEAD~3

# 특정 커밋부터 수정
git rebase -i commit-hash
```

Interactive rebase 옵션:
- `pick`: 커밋 유지
- `reword`: 커밋 메시지 수정
- `edit`: 커밋 수정
- `squash`: 이전 커밋과 합치기
- `fixup`: 이전 커밋과 합치기 (메시지 버림)
- `drop`: 커밋 삭제

## 브랜치 관리

### 브랜치 삭제
```bash
# 로컬 브랜치 삭제 (병합된 경우만)
git branch -d feature/new-feature

# 로컬 브랜치 강제 삭제
git branch -D feature/new-feature

# 원격 브랜치 삭제
git push origin --delete feature/new-feature

# 또는
git push origin :feature/new-feature
```

### 브랜치 이름 변경
```bash
# 현재 브랜치 이름 변경
git branch -m new-branch-name

# 다른 브랜치 이름 변경
git branch -m old-name new-name

# 원격에서도 변경하려면
git push origin :old-name new-name
git push origin -u new-name
```

### 원격 브랜치 추적
```bash
# 현재 브랜치를 원격 브랜치와 연결
git branch --set-upstream-to=origin/feature/new-feature

# 또는 짧게
git branch -u origin/feature/new-feature

# 푸시하면서 추적 설정
git push -u origin feature/new-feature
```

## 브랜치 전략

### Git Flow
대규모 프로젝트에 적합한 브랜치 전략

```
main (production)
  ├── develop (개발)
  │     ├── feature/user-login (기능 개발)
  │     ├── feature/payment (기능 개발)
  │     └── feature/dashboard (기능 개발)
  ├── release/v1.0 (릴리스 준비)
  └── hotfix/critical-bug (긴급 수정)
```

**브랜치 종류:**
- `main`: 배포 가능한 상태의 코드
- `develop`: 다음 릴리스를 위한 개발 코드
- `feature/`: 새로운 기능 개발
- `release/`: 릴리스 준비
- `hotfix/`: 긴급 버그 수정

### GitHub Flow
간단하고 빠른 배포에 적합

```
main
  ├── feature/new-feature-1
  ├── feature/new-feature-2
  └── bugfix/fix-issue-123
```

**워크플로우:**
1. `main` 브랜치에서 작업 브랜치 생성
2. 작업 후 커밋
3. Pull Request 생성
4. 코드 리뷰
5. `main`에 병합 후 즉시 배포

### Trunk-Based Development
지속적 통합에 최적화된 전략

```
main (trunk)
  ├── short-lived-branch-1 (1-2일)
  └── short-lived-branch-2 (1-2일)
```

**특징:**
- 수명이 짧은 브랜치 (1-2일)
- 자주 main에 병합
- Feature Flag 사용

## 실용적인 브랜치 명명 규칙

### 타입별 접두사
```bash
feature/user-authentication
bugfix/login-error
hotfix/security-patch
release/v1.2.0
docs/update-readme
refactor/optimize-database
test/add-unit-tests
```

### 이슈 번호 포함
```bash
feature/123-add-payment
bugfix/456-fix-login
```

## 실습 예제

### 시나리오: 새 기능 개발

```bash
# 1. main 브랜치 최신 상태로 업데이트
git checkout main
git pull origin main

# 2. 새 기능 브랜치 생성
git checkout -b feature/user-profile

# 3. 작업 수행 및 커밋
echo "User Profile Component" > profile.txt
git add profile.txt
git commit -m "Add user profile component"

# 4. 추가 작업
echo "Profile styling" >> profile.txt
git commit -am "Add profile styling"

# 5. 원격에 푸시
git push -u origin feature/user-profile

# 6. main 브랜치로 전환하여 병합
git checkout main
git merge feature/user-profile

# 7. 원격에 푸시
git push origin main

# 8. 기능 브랜치 삭제
git branch -d feature/user-profile
git push origin --delete feature/user-profile
```

### 시나리오: 여러 브랜치 동시 작업

```bash
# Feature A 작업 시작
git checkout -b feature/payment
echo "Payment module" > payment.txt
git add payment.txt
git commit -m "Start payment feature"

# 긴급 버그 수정 필요
git checkout main
git checkout -b hotfix/critical-bug
echo "Bug fix" > bugfix.txt
git add bugfix.txt
git commit -m "Fix critical bug"

# hotfix를 main에 병합
git checkout main
git merge hotfix/critical-bug
git push origin main

# Feature A 작업 재개
git checkout feature/payment
echo "Payment integration" >> payment.txt
git commit -am "Add payment integration"

# main의 최신 변경사항 가져오기
git merge main

# 작업 완료 후 병합
git checkout main
git merge feature/payment
git push origin main
```

## Source Tree에서 브랜치 관리

### 브랜치 생성
1. 상단 툴바에서 "브랜치" 버튼 클릭
2. 브랜치 이름 입력
3. "브랜치 생성" 클릭

### 브랜치 전환
1. 좌측 사이드바에서 원하는 브랜치 더블클릭
2. 또는 우클릭 > "체크아웃"

### 브랜치 병합
1. 병합할 대상 브랜치로 체크아웃
2. 병합하려는 브랜치 우클릭
3. "현재 브랜치로 병합" 선택

### 브랜치 삭제
1. 삭제할 브랜치 우클릭
2. "삭제" 선택

## 트러블슈팅

### 브랜치 전환 시 변경사항이 있을 때
```bash
# 옵션 1: 변경사항 커밋
git add .
git commit -m "WIP: temporary commit"

# 옵션 2: 변경사항 stash
git stash
git checkout other-branch
git stash pop

# 옵션 3: 변경사항 버리기
git checkout -- .
git checkout other-branch
```

### 병합 후 되돌리기
```bash
# 마지막 병합 커밋 취소
git reset --hard HEAD~1

# 특정 병합 커밋 되돌리기
git revert -m 1 merge-commit-hash
```

## 다음 단계

브랜치 관리를 숙지했다면, 다음 가이드로 넘어가세요:
- [Conflict 해결 가이드](./03-conflict-resolution.md)
- [Tag 관리 가이드](./04-tag-management.md)
