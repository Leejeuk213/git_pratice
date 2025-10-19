# Git 핸즈온 퀵 스타트 가이드

> 5분 안에 핸즈온 실습을 시작할 수 있도록 도와드립니다!

## 1단계: 환경 확인 (1분)

```bash
# Git 설치 확인
git --version

# 사용자 정보 확인
git config user.name
git config user.email
```

**설정이 안 되어 있다면:**
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
```

## 2단계: 저장소 준비 (1분)

```bash
# 저장소 클론
git clone https://github.com/alanhakhyeonsong/git-command-guide.git
cd git-command-guide

# 실습 디렉토리로 이동
cd practice
```

## 3단계: 첫 번째 실습 시작 (3분)

### 간단한 Conflict 실습

```bash
# 1. 초기 파일 생성
echo "Line 1: Original" > demo.txt
git add demo.txt
git commit -m "Initial commit"

# 2. main 브랜치에서 수정
echo "Line 2: Main branch" >> demo.txt
git commit -am "Update in main"

# 3. feature 브랜치 생성 (이전 커밋에서)
git checkout HEAD~1
git checkout -b feature/update

# 4. feature 브랜치에서 수정
echo "Line 2: Feature branch" >> demo.txt
git commit -am "Update in feature"

# 5. main으로 돌아가서 병합 (CONFLICT 발생!)
git checkout main
git merge feature/update
```

**예상 출력:**
```
CONFLICT (content): Merge conflict in demo.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### Conflict 해결하기

```bash
# 1. 충돌 내용 확인
cat demo.txt

# 출력 예시:
# Line 1: Original
# <<<<<<< HEAD
# Line 2: Main branch
# =======
# Line 2: Feature branch
# >>>>>>> feature/update

# 2. 수동으로 해결 (두 내용 모두 유지)
echo "Line 1: Original
Line 2: Main branch
Line 2: Feature branch" > demo.txt

# 3. 해결 완료
git add demo.txt
git commit -m "Merge feature/update: resolved conflict"

# 4. 성공 확인
git log --oneline --graph
```

## 다음 단계

✅ 첫 번째 conflict를 해결했습니다! 이제 다음 내용을 진행하세요:

### Option 1: 간단 실습 계속하기 (추천 ⭐)
📖 [간단 Conflict 실습 가이드](./SIMPLE_CONFLICT_PRACTICE.md) - 실무 중심 1-1.5시간 집중 실습

### Option 2: 종합 실습 진행
📖 [핸즈온 실습 가이드](./hands-on-practice.md) - 전체 커리큘럼 4시간

### Option 3: 주제별 학습
- [기본 명령어](./docs/01-basic-commands.md) - Git 기본 사용법
- [브랜치 관리](./docs/02-branch-management.md) - 브랜치 전략
- [Conflict 해결](./docs/03-conflict-resolution.md) - 충돌 해결 심화
- [Tag 관리](./docs/04-tag-management.md) - 버전 관리

### Option 4: 체크리스트 활용
📋 [핸즈온 체크리스트](./HANDS_ON_CHECKLIST.md) - 진행상황 체크

## 자주 사용하는 명령어

```bash
# 상태 확인
git status

# 변경사항 확인
git diff

# 로그 확인
git log --oneline --graph

# 브랜치 목록
git branch

# 현재 위치 초기화 (주의!)
git reset --hard HEAD
```

## 문제 해결

### 실습 중 문제가 생겼을 때
```bash
# 모든 변경사항 버리고 처음부터
git reset --hard HEAD
git clean -fd

# 또는 새로 시작
cd ..
rm -rf practice
mkdir practice
cd practice
```

## 도움말

- 막히면 `git status`로 현재 상태 확인
- `git log --help`로 명령어 도움말 확인
- [가이드 문서](./README.md)에서 상세 내용 참조

---

**준비 완료! 🚀 즐거운 학습 되세요!**
