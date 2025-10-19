# Git Conflict 해결 가이드

## 목차
- [Conflict란?](#conflict란)
- [Conflict 발생 상황](#conflict-발생-상황)
- [Conflict 해결 방법](#conflict-해결-방법)
- [충돌 마커 이해하기](#충돌-마커-이해하기)
- [실습 시나리오](#실습-시나리오)
- [Source Tree에서 해결하기](#source-tree에서-해결하기)

## Conflict란?

**Conflict(충돌)**는 두 개 이상의 브랜치에서 같은 파일의 같은 부분을 서로 다르게 수정했을 때 발생합니다. Git은 어떤 변경사항을 선택해야 할지 자동으로 결정할 수 없어 사용자에게 수동으로 해결하도록 요청합니다.

### Conflict가 발생하지 않는 경우
- 다른 파일을 수정한 경우
- 같은 파일의 다른 부분을 수정한 경우
- 한쪽만 수정한 경우

### Conflict가 발생하는 경우
- 같은 파일의 같은 라인을 서로 다르게 수정
- 한쪽은 파일을 수정하고 다른 쪽은 삭제
- 같은 이름의 파일을 각각 생성

## Conflict 발생 상황

### 1. Merge 시 충돌
```bash
git checkout main
git merge feature/new-feature
# CONFLICT (content): Merge conflict in file.txt
# Automatic merge failed; fix conflicts and then commit the result.
```

### 2. Rebase 시 충돌
```bash
git checkout feature/new-feature
git rebase main
# CONFLICT (content): Merge conflict in file.txt
# error: could not apply abc1234... commit message
```

### 3. Pull 시 충돌
```bash
git pull origin main
# CONFLICT (content): Merge conflict in file.txt
# Automatic merge failed; fix conflicts and then commit the result.
```

### 4. Cherry-pick 시 충돌
```bash
git cherry-pick commit-hash
# CONFLICT (content): Merge conflict in file.txt
```

## Conflict 해결 방법

### 1. 충돌 확인
```bash
# 충돌 파일 확인
git status

# 충돌 내용 확인
git diff

# 충돌 파일 목록만 보기
git diff --name-only --diff-filter=U
```

### 2. 충돌 해결 전략

#### 전략 A: 수동으로 해결
```bash
# 1. 충돌 파일 열기
code file.txt

# 2. 충돌 마커를 보고 원하는 내용으로 수정
# 3. 충돌 마커 제거
# 4. 파일 저장

# 5. 해결된 파일 스테이징
git add file.txt

# 6. 병합 커밋 생성 (merge의 경우)
git commit

# 또는 rebase 계속 진행
git rebase --continue
```

#### 전략 B: 한쪽 변경사항 선택
```bash
# 우리쪽(현재 브랜치) 변경사항 사용
git checkout --ours file.txt
git add file.txt

# 그들쪽(병합하려는 브랜치) 변경사항 사용
git checkout --theirs file.txt
git add file.txt
```

#### 전략 C: Merge Tool 사용
```bash
# 기본 merge tool 실행
git mergetool

# 특정 merge tool 사용 (예: VSCode)
git mergetool --tool=vscode
```

### 3. 충돌 해결 취소
```bash
# Merge 중단
git merge --abort

# Rebase 중단
git rebase --abort

# Cherry-pick 중단
git cherry-pick --abort
```

## 충돌 마커 이해하기

충돌이 발생하면 파일에 다음과 같은 마커가 표시됩니다:

```
<<<<<<< HEAD (현재 브랜치)
이것은 현재 브랜치의 내용입니다.
여기가 우리쪽(ours) 변경사항입니다.
=======
이것은 병합하려는 브랜치의 내용입니다.
여기가 그들쪽(theirs) 변경사항입니다.
>>>>>>> feature/new-feature (병합하려는 브랜치)
```

### 마커 설명
- `<<<<<<< HEAD`: 현재 브랜치의 변경사항 시작
- `=======`: 두 변경사항의 구분선
- `>>>>>>> branch-name`: 병합하려는 브랜치의 변경사항 끝

### 해결 예시

**충돌 전 상태:**
```
<<<<<<< HEAD
const version = "1.0.0";
=======
const version = "2.0.0";
>>>>>>> feature/update-version
```

**해결 후 상태 (옵션 1 - 최신 버전 선택):**
```
const version = "2.0.0";
```

**해결 후 상태 (옵션 2 - 두 정보 모두 유지):**
```
const version = "2.0.0"; // Updated from 1.0.0
```

## 실습 시나리오

### 시나리오 1: 기본 Merge Conflict

#### 준비 단계
```bash
# 1. 실습용 파일 생성
echo "Hello World" > greeting.txt
git add greeting.txt
git commit -m "Initial commit"

# 2. main 브랜치에서 수정
echo "Hello World from Main" > greeting.txt
git commit -am "Update greeting in main"

# 3. feature 브랜치 생성 (이전 커밋에서)
git checkout HEAD~1
git checkout -b feature/update-greeting

# 4. feature 브랜치에서 같은 부분 수정
echo "Hello World from Feature" > greeting.txt
git commit -am "Update greeting in feature"
```

#### Conflict 발생
```bash
# 5. main으로 돌아가서 병합 시도
git checkout main
git merge feature/update-greeting

# 출력:
# CONFLICT (content): Merge conflict in greeting.txt
# Automatic merge failed; fix conflicts and then commit the result.
```

#### Conflict 해결
```bash
# 6. 충돌 확인
git status
cat greeting.txt

# 출력:
# <<<<<<< HEAD
# Hello World from Main
# =======
# Hello World from Feature
# >>>>>>> feature/update-greeting

# 7. 수동으로 해결 (두 내용 합치기)
echo "Hello World from Main and Feature" > greeting.txt

# 8. 해결 완료
git add greeting.txt
git commit -m "Merge feature/update-greeting - resolved conflicts"
```

### 시나리오 2: 여러 파일 충돌

```bash
# 1. 초기 설정
git checkout main
echo "User: John" > user.txt
echo "Version: 1.0" > version.txt
git add .
git commit -m "Add user and version files"

# 2. feature 브랜치 생성 및 수정
git checkout -b feature/updates
echo "User: John Doe" > user.txt
echo "Version: 1.1" > version.txt
git commit -am "Update user and version in feature"

# 3. main 브랜치로 돌아가서 수정
git checkout main
echo "User: Jane" > user.txt
echo "Version: 2.0" > version.txt
git commit -am "Update user and version in main"

# 4. 병합 시도
git merge feature/updates
# 두 파일 모두 충돌 발생

# 5. user.txt - theirs 사용
git checkout --theirs user.txt

# 6. version.txt - ours 사용
git checkout --ours version.txt

# 7. 해결 완료
git add user.txt version.txt
git commit -m "Merge feature/updates - resolved conflicts"
```

### 시나리오 3: Rebase Conflict

```bash
# 1. 초기 설정
git checkout main
echo "Line 1" > file.txt
git add file.txt
git commit -m "Add line 1"

# 2. feature 브랜치에서 작업
git checkout -b feature/add-lines
echo "Line 2 from feature" >> file.txt
git commit -am "Add line 2 in feature"

# 3. main 브랜치에서 다른 작업
git checkout main
echo "Line 2 from main" >> file.txt
git commit -am "Add line 2 in main"

# 4. feature 브랜치에서 rebase 시도
git checkout feature/add-lines
git rebase main
# CONFLICT 발생

# 5. 충돌 해결
cat file.txt
# 충돌 마커 확인 후 수정
echo -e "Line 1\nLine 2 from main\nLine 2 from feature" > file.txt

# 6. rebase 계속
git add file.txt
git rebase --continue
```

## Source Tree에서 해결하기

### 1. Conflict 발생 확인
1. Source Tree에서 병합 또는 Pull 시 충돌 발생
2. 파일 상태 영역에서 충돌 파일에 느낌표(!) 표시 확인

### 2. Conflict 해결 방법

#### 방법 A: 내장 Merge Tool 사용
1. 충돌 파일 우클릭
2. "충돌 해결" > "Merge Tool 실행" 선택
3. 좌측(Local/Ours), 우측(Remote/Theirs), 하단(Result) 확인
4. 원하는 변경사항 선택하여 Result 창에 반영
5. 저장 후 종료

#### 방법 B: 외부 에디터 사용
1. 충돌 파일 우클릭
2. "외부 에디터로 열기" 선택
3. VSCode 등에서 충돌 마커 확인
4. 수동으로 해결 후 저장
5. Source Tree로 돌아와서 "해결됨으로 표시" 클릭

#### 방법 C: 한쪽 선택
1. 충돌 파일 우클릭
2. "충돌 해결" 선택
3. "내 것 사용" 또는 "그들 것 사용" 선택

### 3. 해결 완료
1. 모든 충돌 해결 후 "커밋" 버튼 활성화 확인
2. 커밋 메시지 작성
3. 커밋 실행

## VSCode에서 해결하기

VSCode는 충돌 해결을 위한 편리한 UI를 제공합니다:

### 1. 충돌 파일 열기
충돌 파일을 열면 다음 옵션이 표시됩니다:

```
<<<<<<< HEAD (Current Change)
[Accept Current Change] [Accept Incoming Change] [Accept Both Changes] [Compare Changes]
=======
>>>>>>> branch-name (Incoming Change)
```

### 2. 해결 옵션
- **Accept Current Change**: 현재 브랜치 변경사항 사용
- **Accept Incoming Change**: 병합하려는 브랜치 변경사항 사용
- **Accept Both Changes**: 두 변경사항 모두 유지
- **Compare Changes**: 변경사항 비교 화면 열기

### 3. Source Control에서 확인
1. 좌측 Source Control 아이콘 클릭
2. "Merge Changes" 섹션에서 충돌 파일 확인
3. 파일 클릭하여 해결
4. 해결 후 Stage 및 Commit

## 고급 Conflict 해결

### 3-way Merge Tool 설정
```bash
# VSCode를 merge tool로 설정
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# P4Merge 설정 (시각적 도구)
git config --global merge.tool p4merge
git config --global mergetool.p4merge.path "/Applications/p4merge.app/Contents/MacOS/p4merge"

# 실행
git mergetool
```

### Rerere (Reuse Recorded Resolution)
같은 충돌을 반복적으로 해결할 때 유용

```bash
# rerere 활성화
git config --global rerere.enabled true

# 이전에 해결한 방법 자동 적용
git rerere
```

### Conflict 회피 전략
```bash
# Pull 시 항상 rebase 사용
git config --global pull.rebase true

# Merge 전 확인
git fetch origin
git diff HEAD origin/main

# 임시 병합으로 충돌 미리 확인
git merge --no-commit --no-ff feature/branch
git merge --abort  # 확인 후 취소
```

## 베스트 프랙티스

### 1. Conflict 예방
- 자주 pull/merge하여 최신 상태 유지
- 큰 변경사항은 작은 단위로 나누어 커밋
- 팀원과 작업 영역 조율
- 코드 리뷰를 통한 사전 충돌 감지

### 2. Conflict 해결 시
- 충돌 발생 시 당황하지 말고 차근차근 해결
- 양쪽 변경사항의 의도 파악
- 불확실하면 팀원과 상의
- 해결 후 반드시 테스트

### 3. 커밋 메시지
```bash
# 좋은 충돌 해결 커밋 메시지
git commit -m "Merge feature/payment: resolve conflicts in checkout.js

- Keep both validation logics
- Use updated payment API from feature branch
- Preserve error handling from main branch"
```

## 트러블슈팅

### 충돌 마커가 남아있을 때
```bash
# 충돌 마커 검색
grep -r "<<<<<<< HEAD" .
grep -r "=======" .
grep -r ">>>>>>>" .
```

### 잘못 해결한 경우
```bash
# 병합 전으로 되돌리기
git reset --hard ORIG_HEAD

# 특정 파일만 되돌리기
git checkout HEAD -- file.txt
```

### Binary 파일 충돌
```bash
# Binary 파일은 한쪽 선택만 가능
git checkout --ours binary-file.png
# 또는
git checkout --theirs binary-file.png
```

## 실전 팁

### 대규모 충돌 처리
```bash
# 1. 충돌 파일 수 확인
git diff --name-only --diff-filter=U | wc -l

# 2. 카테고리별 처리
# - 자동 생성 파일: theirs 사용
# - 설정 파일: 수동 병합
# - 소스 코드: 주의 깊게 병합

# 3. 단계별 검증
git add file1.txt
# 테스트
git add file2.txt
# 테스트
```

### 충돌 히스토리 추적
```bash
# 충돌이 발생한 커밋 찾기
git log --merge --all

# 특정 파일의 충돌 이력
git log --merge -- file.txt
```

## 핸즈온 체크리스트

실습 시 다음 단계를 따라 해보세요:

- [ ] 기본 merge conflict 생성 및 해결
- [ ] 여러 파일 동시 충돌 해결
- [ ] `--ours`와 `--theirs` 옵션 사용
- [ ] Rebase conflict 해결
- [ ] VSCode에서 충돌 해결
- [ ] Source Tree에서 충돌 해결
- [ ] Merge tool 사용
- [ ] 충돌 해결 취소 및 재시도

## 다음 단계

Conflict 해결을 마스터했다면:
- [Tag 관리 가이드](./04-tag-management.md)
- [핸즈온 실습 가이드](../hands-on-practice.md)
