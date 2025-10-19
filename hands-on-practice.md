# Git 핸즈온 실습 가이드

## 목차
- [실습 준비](#실습-준비)
- [실습 1: 기본 Git 워크플로우](#실습-1-기본-git-워크플로우)
- [실습 2: 브랜치 생성과 병합](#실습-2-브랜치-생성과-병합)
- [실습 3: Conflict 해결 실습](#실습-3-conflict-해결-실습)
- [실습 4: Tag 관리](#실습-4-tag-관리)
- [실습 5: 종합 시나리오](#실습-5-종합-시나리오)

## 실습 준비

### 필수 요구사항
- Git 설치 확인: `git --version`
- Source Tree 설치 (선택사항)
- 텍스트 에디터 (VSCode 권장)

### 초기 설정

```bash
# 1. 이 저장소 클론
git clone https://github.com/alanhakhyeonsong/git-command-guide.git
cd git-command-guide

# 2. 사용자 정보 확인
git config user.name
git config user.email

# 3. 사용자 정보 설정 (필요시)
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"

# 4. 실습용 디렉토리 생성
mkdir practice
cd practice
```

---

## 실습 1: 기본 Git 워크플로우

**목표**: Git의 기본적인 작업 흐름을 이해하고 실습합니다.

### Step 1: 파일 생성 및 추가

```bash
# 1. 새 파일 생성
echo "Hello, Git!" > hello.txt

# 2. 상태 확인
git status

# 3. Staging area에 추가
git add hello.txt

# 4. 다시 상태 확인
git status
```

**예상 결과**:
- `hello.txt`가 "Changes to be committed"에 표시됨

### Step 2: 첫 번째 커밋

```bash
# 1. 커밋 생성
git commit -m "Add hello.txt"

# 2. 로그 확인
git log --oneline

# 3. 상태 확인
git status
```

**예상 결과**:
- "nothing to commit, working tree clean" 메시지

### Step 3: 파일 수정

```bash
# 1. 파일 수정
echo "Learning Git is fun!" >> hello.txt

# 2. 변경사항 확인
git diff

# 3. 상태 확인
git status

# 4. 커밋
git add hello.txt
git commit -m "Update hello.txt with new message"
```

### Step 4: 히스토리 확인

```bash
# 1. 로그 보기
git log

# 2. 한 줄로 보기
git log --oneline

# 3. 그래프 형태로 보기
git log --graph --oneline

# 4. 특정 파일의 히스토리
git log hello.txt
```

### 체크포인트 ✅
- [ ] 파일 생성 및 git add 실행
- [ ] 커밋 생성
- [ ] 파일 수정 및 추가 커밋
- [ ] git log로 히스토리 확인

---

## 실습 2: 브랜치 생성과 병합

**목표**: 브랜치를 생성하고 병합하는 방법을 학습합니다.

### Step 1: 브랜치 생성 및 전환

```bash
# 1. 현재 브랜치 확인
git branch

# 2. 새 브랜치 생성
git branch feature/greeting

# 3. 브랜치 목록 확인
git branch

# 4. 브랜치 전환
git checkout feature/greeting

# 또는 생성과 전환을 동시에
# git checkout -b feature/greeting

# 5. 현재 브랜치 확인
git branch
```

### Step 2: 브랜치에서 작업

```bash
# 1. 새 파일 생성
echo "Good morning!" > morning.txt

# 2. 커밋
git add morning.txt
git commit -m "Add morning greeting"

# 3. 또 다른 파일 추가
echo "Good evening!" > evening.txt
git add evening.txt
git commit -m "Add evening greeting"

# 4. 로그 확인
git log --oneline
```

### Step 3: Fast-Forward 병합

```bash
# 1. main 브랜치로 전환
git checkout main

# 2. main 브랜치의 로그 확인
git log --oneline

# 3. feature/greeting 브랜치 병합
git merge feature/greeting

# 4. 병합 후 로그 확인
git log --oneline --graph

# 5. 파일 확인
ls -la
```

**예상 결과**:
- `morning.txt`와 `evening.txt` 파일이 main 브랜치에 존재
- Fast-forward 메시지 표시

### Step 4: 3-way 병합

```bash
# 1. main 브랜치에서 작업
echo "Main branch work" > main-work.txt
git add main-work.txt
git commit -m "Add work in main branch"

# 2. 새 브랜치 생성 (이전 커밋 기준)
git checkout -b feature/afternoon HEAD~1

# 3. feature 브랜치에서 작업
echo "Good afternoon!" > afternoon.txt
git add afternoon.txt
git commit -m "Add afternoon greeting"

# 4. main으로 돌아가서 병합
git checkout main
git merge feature/afternoon

# 5. 그래프로 확인
git log --graph --oneline --all
```

**예상 결과**:
- 병합 커밋이 생성됨
- 그래프에서 브랜치가 합쳐지는 모습 확인

### 체크포인트 ✅
- [ ] 브랜치 생성 및 전환
- [ ] 브랜치에서 작업 및 커밋
- [ ] Fast-forward 병합 수행
- [ ] 3-way 병합 수행
- [ ] git log --graph로 히스토리 확인

---

## 실습 3: Conflict 해결 실습

**목표**: Merge conflict를 직접 발생시키고 해결하는 방법을 학습합니다.

### Step 1: Conflict 상황 만들기

```bash
# 1. 초기 파일 생성
git checkout main
echo "Line 1: Original content" > conflict-demo.txt
git add conflict-demo.txt
git commit -m "Initial: Add conflict-demo.txt"

# 2. main 브랜치에서 수정
echo "Line 2: Main branch edit" >> conflict-demo.txt
git commit -am "Main: Update conflict-demo.txt"

# 3. 새 브랜치 생성 (이전 커밋에서)
git checkout HEAD~1
git checkout -b feature/conflict-test

# 4. feature 브랜치에서 같은 부분 수정
echo "Line 2: Feature branch edit" >> conflict-demo.txt
git commit -am "Feature: Update conflict-demo.txt"

# 5. main으로 돌아가서 병합 시도
git checkout main
git merge feature/conflict-test
```

**예상 결과**:
```
CONFLICT (content): Merge conflict in conflict-demo.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### Step 2: Conflict 확인

```bash
# 1. 상태 확인
git status

# 2. 충돌 파일 내용 확인
cat conflict-demo.txt
```

**파일 내용 예시**:
```
Line 1: Original content
<<<<<<< HEAD
Line 2: Main branch edit
=======
Line 2: Feature branch edit
>>>>>>> feature/conflict-test
```

### Step 3: 수동으로 Conflict 해결

```bash
# 1. 에디터로 파일 열기
# VSCode 사용
code conflict-demo.txt

# 또는 vim 사용
# vim conflict-demo.txt

# 2. 충돌 마커를 제거하고 원하는 내용으로 수정
# 예시: 두 내용을 모두 유지
echo "Line 1: Original content
Line 2: Main branch edit
Line 2: Feature branch edit" > conflict-demo.txt

# 3. 파일 저장 후 확인
cat conflict-demo.txt

# 4. 해결된 파일 추가
git add conflict-demo.txt

# 5. 상태 확인
git status

# 6. 병합 커밋 생성
git commit -m "Merge feature/conflict-test: resolve conflicts"

# 7. 로그 확인
git log --graph --oneline --all
```

### Step 4: --ours와 --theirs 옵션 실습

```bash
# 1. 새로운 충돌 상황 만들기
git checkout main
echo "Version: 1.0 (main)" > version.txt
git add version.txt
git commit -m "Add version file in main"

git checkout -b feature/update-version HEAD~1
echo "Version: 2.0 (feature)" > version.txt
git add version.txt
git commit -m "Add version file in feature"

git checkout main
git merge feature/update-version
# CONFLICT 발생!

# 2. ours 사용 (main 브랜치 내용 선택)
git checkout --ours version.txt
cat version.txt
git add version.txt
git commit -m "Merge: keep main version"

# 3. 다시 충돌 상황 만들기
git reset --hard HEAD~1
git merge feature/update-version

# 4. theirs 사용 (feature 브랜치 내용 선택)
git checkout --theirs version.txt
cat version.txt
git add version.txt
git commit -m "Merge: keep feature version"
```

### Step 5: 여러 파일 충돌 해결

```bash
# 1. 여러 파일에서 충돌 만들기
git checkout main
echo "Config: production" > config.txt
echo "Database: mysql" > database.txt
git add config.txt database.txt
git commit -m "Add config files in main"

git checkout -b feature/config-update HEAD~1
echo "Config: development" > config.txt
echo "Database: postgresql" > database.txt
git add config.txt database.txt
git commit -m "Add config files in feature"

git checkout main
git merge feature/config-update
# 두 파일 모두 충돌!

# 2. 파일별로 다른 전략 사용
# config.txt는 theirs 사용
git checkout --theirs config.txt

# database.txt는 수동으로 해결
echo "Database: mysql, postgresql" > database.txt

# 3. 모두 추가하고 커밋
git add config.txt database.txt
git commit -m "Merge feature/config-update: resolve all conflicts"
```

### 체크포인트 ✅
- [ ] Merge conflict 상황 생성
- [ ] 충돌 파일 내용 확인 (마커 이해)
- [ ] 수동으로 충돌 해결
- [ ] `--ours` 옵션 사용
- [ ] `--theirs` 옵션 사용
- [ ] 여러 파일 충돌 동시 해결

---

## 실습 4: Tag 관리

**목표**: 버전 관리를 위한 Tag 생성, 조회, 관리 방법을 학습합니다.

### Step 1: 첫 번째 릴리스 태그

```bash
# 1. main 브랜치 확인
git checkout main

# 2. 경량 태그 생성
git tag v0.1.0

# 3. 주석 태그 생성
git tag -a v1.0.0 -m "Release version 1.0.0

Initial release with basic features:
- Greeting messages
- Configuration management
- Version control"

# 4. 태그 목록 확인
git tag

# 5. 태그 상세 정보 확인
git show v1.0.0
```

### Step 2: 기능 추가 후 Minor 버전 태그

```bash
# 1. 새 기능 추가
echo "New feature content" > feature.txt
git add feature.txt
git commit -m "Add new feature"

# 2. Minor 버전 태그
git tag -a v1.1.0 -m "Release version 1.1.0

New Features:
- Feature X implementation

Improvements:
- Code optimization"

# 3. 태그 확인
git tag -l "v1.*"
```

### Step 3: Hotfix와 Patch 버전

```bash
# 1. 이전 버전에서 브랜치 생성
git checkout -b hotfix/critical-fix v1.0.0

# 2. 버그 수정
echo "Bug fixed" > bugfix.txt
git add bugfix.txt
git commit -m "Fix critical security bug"

# 3. main에 병합
git checkout main
git merge --no-ff hotfix/critical-fix

# 4. Patch 버전 태그
git tag -a v1.0.1 -m "Hotfix release v1.0.1

Critical Fixes:
- Security vulnerability patch"

# 5. 태그 히스토리 확인
git log --oneline --decorate --graph
```

### Step 4: Tag 관리

```bash
# 1. 모든 태그 보기
git tag

# 2. 태그 정렬해서 보기
git tag --sort=-version:refname

# 3. 특정 태그로 체크아웃
git checkout v1.0.0

# 4. 체크아웃 후 새 브랜치 생성
git checkout -b investigate/v1.0.0

# 5. main으로 돌아가기
git checkout main

# 6. 태그 삭제
git tag -d v0.1.0

# 7. 태그 목록 재확인
git tag
```

### Step 5: 원격 저장소와 Tag (실습용)

```bash
# 실제 원격 저장소가 있다면:

# 1. 특정 태그 푸시
# git push origin v1.0.0

# 2. 모든 태그 푸시
# git push origin --tags

# 3. 원격 태그 삭제
# git push origin --delete v1.0.0
```

### 체크포인트 ✅
- [ ] 경량 태그 생성
- [ ] 주석 태그 생성
- [ ] 버전별 태그 생성 (Major, Minor, Patch)
- [ ] 태그 목록 확인
- [ ] 태그로 체크아웃
- [ ] 태그 삭제

---

## 실습 5: 종합 시나리오

**목표**: 실제 프로젝트와 유사한 상황에서 Git 워크플로우를 실습합니다.

### 시나리오: 팀 프로젝트 시뮬레이션

#### 상황 설정
당신은 웹 애플리케이션 개발 팀의 일원입니다. 다음 작업을 수행해야 합니다:
1. 새로운 로그인 기능 개발
2. 다른 팀원의 변경사항과 충돌 해결
3. 버그 수정
4. 릴리스 버전 태깅

### Part 1: 프로젝트 초기화

```bash
# 1. 새 디렉토리로 이동
cd ~/practice
mkdir team-project
cd team-project
git init

# 2. 초기 파일 구조 생성
mkdir src
echo "# Web Application" > README.md
echo "const app = {};" > src/app.js
echo "body { margin: 0; }" > src/style.css

# 3. 초기 커밋
git add .
git commit -m "Initial commit: project structure"

# 4. 첫 번째 태그
git tag -a v0.1.0 -m "Initial project setup"
```

### Part 2: Feature 개발 (사용자 A)

```bash
# 1. feature 브랜치 생성
git checkout -b feature/user-login

# 2. 로그인 기능 구현
cat > src/login.js << 'EOF'
function login(username, password) {
    console.log('Login attempt:', username);
    // TODO: implement authentication
    return false;
}
EOF

git add src/login.js
git commit -m "Add login function skeleton"

# 3. app.js 수정 (로그인 기능 통합)
cat > src/app.js << 'EOF'
const app = {
    version: '0.2.0',
    features: ['login']
};
EOF

git commit -am "Integrate login feature into app"

# 4. 로그인 UI 추가
cat >> src/style.css << 'EOF'

.login-form {
    width: 300px;
    margin: 50px auto;
}
EOF

git commit -am "Add login form styles"
```

### Part 3: Main 브랜치 업데이트 (사용자 B 시뮬레이션)

```bash
# 1. main 브랜치로 전환
git checkout main

# 2. 다른 기능 추가 (사용자 B의 작업 시뮬레이션)
cat > src/app.js << 'EOF'
const app = {
    version: '0.2.0',
    features: ['dashboard']
};
EOF

git commit -am "Add dashboard feature"

# 3. 스타일 수정
cat >> src/style.css << 'EOF'

.dashboard {
    padding: 20px;
}
EOF

git commit -am "Add dashboard styles"
```

### Part 4: Conflict 발생 및 해결

```bash
# 1. feature 브랜치로 전환
git checkout feature/user-login

# 2. main 브랜치 병합 시도
git merge main
# CONFLICT 발생!

# 3. 충돌 확인
git status
cat src/app.js

# 4. app.js 충돌 해결 (두 기능 모두 유지)
cat > src/app.js << 'EOF'
const app = {
    version: '0.2.0',
    features: ['login', 'dashboard']
};
EOF

# 5. style.css 확인 (자동 병합됨)
cat src/style.css

# 6. 해결 완료
git add src/app.js
git commit -m "Merge main into feature/user-login: resolve conflicts

- Keep both login and dashboard features
- Merge styles successfully"

# 7. 로그 확인
git log --graph --oneline --all
```

### Part 5: Main에 병합 및 릴리스

```bash
# 1. main 브랜치로 전환
git checkout main

# 2. feature 브랜치 병합
git merge --no-ff feature/user-login -m "Merge feature/user-login

Features:
- User login implementation
- Login form UI"

# 3. 릴리스 태그
git tag -a v0.2.0 -m "Release v0.2.0

Features:
- User login
- Dashboard
- Improved styling"

# 4. feature 브랜치 삭제
git branch -d feature/user-login
```

### Part 6: Hotfix

```bash
# 1. 프로덕션 버그 발견 (v0.2.0)
git checkout -b hotfix/login-bug v0.2.0

# 2. 버그 수정
cat > src/login.js << 'EOF'
function login(username, password) {
    if (!username || !password) {
        console.error('Username and password required');
        return false;
    }
    console.log('Login attempt:', username);
    // TODO: implement authentication
    return false;
}
EOF

git commit -am "Fix: Add validation in login function"

# 3. main에 병합
git checkout main
git merge --no-ff hotfix/login-bug

# 4. Patch 버전 태그
git tag -a v0.2.1 -m "Hotfix v0.2.1

Fixes:
- Add input validation to login function"

# 5. hotfix 브랜치 삭제
git branch -d hotfix/login-bug

# 6. 최종 상태 확인
git log --graph --oneline --all --decorate
git tag
```

### Part 7: 프로젝트 정리 및 검토

```bash
# 1. 모든 파일 확인
ls -R

# 2. 모든 커밋 히스토리
git log --oneline

# 3. 모든 태그
git tag -l

# 4. 브랜치 목록
git branch -a

# 5. 통계
echo "Total commits: $(git rev-list --count HEAD)"
echo "Total tags: $(git tag | wc -l)"
echo "Total files: $(git ls-files | wc -l)"
```

### 체크포인트 ✅
- [ ] 프로젝트 초기화
- [ ] Feature 브랜치에서 개발
- [ ] Main 브랜치 업데이트
- [ ] Merge conflict 발생 및 해결
- [ ] Feature를 main에 병합
- [ ] 릴리스 태그 생성
- [ ] Hotfix 브랜치 생성 및 병합
- [ ] Patch 버전 태그 생성

---

## Source Tree 실습 가이드

### Source Tree에서 같은 작업 수행하기

#### 1. 저장소 추가
1. Source Tree 실행
2. "파일" > "복제/새로 만들기"
3. 로컬 저장소 경로 선택

#### 2. 브랜치 생성
1. 상단 "브랜치" 버튼 클릭
2. 브랜치 이름 입력 (예: feature/user-login)
3. "브랜치 생성" 클릭

#### 3. 커밋
1. 파일 수정 후 Source Tree 확인
2. "파일 상태" 탭에서 변경된 파일 확인
3. Stage할 파일 선택 (체크박스)
4. 하단 커밋 메시지 입력
5. "커밋" 버튼 클릭

#### 4. 병합
1. 병합할 대상 브랜치로 체크아웃 (예: main)
2. 좌측 사이드바에서 병합할 브랜치 우클릭
3. "현재 브랜치로 병합" 선택

#### 5. Conflict 해결
1. 충돌 발생 시 파일에 느낌표 표시
2. 충돌 파일 우클릭 > "충돌 해결"
3. "Merge Tool 실행" 또는 "외부 에디터로 열기"
4. 해결 후 "해결됨으로 표시"
5. 커밋 버튼 클릭

#### 6. Tag 생성
1. 태그를 생성할 커밋 선택
2. 우클릭 > "태그..."
3. 태그 이름 입력 및 메시지 작성
4. "태그 추가" 클릭

---

## 추가 실습 과제

### 과제 1: Git Flow 실습
```bash
# 1. develop 브랜치 생성
# 2. feature 브랜치에서 작업
# 3. release 브랜치 생성
# 4. main에 병합 및 태깅
# 5. develop에도 병합
```

### 과제 2: Rebase 실습
```bash
# 1. feature 브랜치 생성
# 2. main과 feature 모두 수정
# 3. feature에서 rebase main
# 4. 충돌 해결
# 5. main에 병합
```

### 과제 3: Cherry-pick 실습
```bash
# 1. feature 브랜치에서 여러 커밋 생성
# 2. 특정 커밋만 main에 적용
# 3. cherry-pick 사용
```

---

## 문제 해결 가이드

### 실습 중 문제가 발생했을 때

#### 1. 작업 취소하고 처음부터
```bash
# 현재 작업 모두 버리기
git reset --hard HEAD
git clean -fd

# 특정 브랜치로 강제 이동
git checkout -f main
```

#### 2. 실습 디렉토리 초기화
```bash
# 디렉토리 삭제 후 재생성
cd ..
rm -rf practice
mkdir practice
cd practice
```

#### 3. 커밋 되돌리기
```bash
# 마지막 커밋 취소 (변경사항 유지)
git reset --soft HEAD~1

# 마지막 커밋 취소 (변경사항 버림)
git reset --hard HEAD~1
```

---

## 실습 완료 체크리스트

### 전체 실습 완료 확인

- [ ] 실습 1: 기본 Git 워크플로우 ✅
- [ ] 실습 2: 브랜치 생성과 병합 ✅
- [ ] 실습 3: Conflict 해결 실습 ✅
- [ ] 실습 4: Tag 관리 ✅
- [ ] 실습 5: 종합 시나리오 ✅
- [ ] Source Tree에서 동일 작업 수행 ✅

### 핵심 명령어 숙지 확인

- [ ] `git add`, `git commit`
- [ ] `git branch`, `git checkout`
- [ ] `git merge`
- [ ] `git status`, `git log`
- [ ] `git tag`
- [ ] Conflict 해결 방법
- [ ] `git checkout --ours/--theirs`

---

## 다음 단계

모든 실습을 완료했다면:

1. **실제 프로젝트 적용**: 작은 개인 프로젝트에 Git을 적용해보세요
2. **팀 협업**: 실제 팀 프로젝트에서 Git Flow를 적용해보세요
3. **고급 주제 학습**:
   - Git Submodules
   - Git Hooks
   - CI/CD 연동
   - GitHub Actions

---

## 참고 자료

- [Pro Git Book (한글)](https://git-scm.com/book/ko/v2)
- [Git 공식 문서](https://git-scm.com/doc)
- [Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials)
- [GitHub Guides](https://guides.github.com/)

---

## 피드백 및 질문

실습 중 궁금한 점이나 개선 사항이 있다면 이슈를 생성해주세요!

**즐거운 Git 학습 되세요! 🚀**
