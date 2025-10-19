# 핸즈온 준비 가이드 (강사용)

## 📋 개요

**대상**: 사내 Git 활용이 미숙한 엔지니어  
**목적**: Git 기본 개념 이해 및 Conflict 해결 능력 향상  
**실습 방법**: Text 파일 기반 실습  
**소요 시간**: 약 4시간 (2일차 진행 시)

## 📚 준비된 자료

### 가이드 문서
1. **[QUICKSTART.md](./QUICKSTART.md)** - 5분 퀵스타트
2. **[README.md](./README.md)** - 메인 가이드 (업데이트됨)
3. **[HANDS_ON_CHECKLIST.md](./HANDS_ON_CHECKLIST.md)** - 실습 체크리스트

### 상세 학습 자료
- **[docs/01-basic-commands.md](./docs/01-basic-commands.md)** - 기본 명령어
- **[docs/02-branch-management.md](./docs/02-branch-management.md)** - 브랜치 관리
- **[docs/03-conflict-resolution.md](./docs/03-conflict-resolution.md)** - Conflict 해결 ⭐
- **[docs/04-tag-management.md](./docs/04-tag-management.md)** - Tag 관리

### 실습 가이드
- **[hands-on-practice.md](./hands-on-practice.md)** - 종합 실습 가이드

### 예제 파일
- **[examples/conflict-example.txt](./examples/conflict-example.txt)** - Conflict 실습 예제
- **practice/** - 실습용 디렉토리

## 🎯 핸즈온 구성 (추천)

### 1일차: Git 기본 & 브랜치 (2시간)

#### 세션 1: Git 기본 (1시간)
- **이론** (15분)
  - Git이란?
  - Working Directory, Staging Area, Repository 개념
  - 기본 워크플로우
  
- **실습** (45분)
  - 저장소 초기화
  - 파일 생성 및 커밋
  - 히스토리 조회
  - 실습 가이드: `hands-on-practice.md` - 실습 1

#### 세션 2: 브랜치 관리 (1시간)
- **이론** (15분)
  - 브랜치 개념
  - Fast-forward vs 3-way 병합
  - 브랜치 전략 소개
  
- **실습** (45분)
  - 브랜치 생성 및 전환
  - 브랜치에서 작업
  - 병합 실습
  - 실습 가이드: `hands-on-practice.md` - 실습 2

### 2일차: Conflict 해결 & Tag 관리 (2시간)

#### 세션 3: Conflict 해결 ⭐ (1시간 30분)
- **이론** (20분)
  - Conflict 발생 원인
  - 충돌 마커 이해
  - 해결 전략
  
- **실습** (70분) - **메인 실습!**
  - 기본 conflict 생성 및 해결 (20분)
  - 여러 파일 충돌 (15분)
  - `--ours`, `--theirs` 옵션 (15분)
  - Source Tree/VSCode 활용 (20분)
  - 실습 가이드: `hands-on-practice.md` - 실습 3

#### 세션 4: Tag 관리 (30분)
- **이론** (10분)
  - Tag 개념
  - Semantic Versioning
  - Tag 관리 전략
  
- **실습** (20분)
  - 릴리스 태그 생성
  - 버전 업데이트
  - Hotfix 태그
  - 실습 가이드: `hands-on-practice.md` - 실습 4

## 🔥 핸즈온 진행 팁

### 시작 전 체크리스트
- [ ] 참가자 Git 설치 확인
- [ ] Source Tree 설치 안내 (선택)
- [ ] 저장소 Clone 완료 확인
- [ ] 사용자 정보 설정 확인
- [ ] 스크린 공유 준비

### 진행 시 주의사항

#### ✅ DO
- 명령어를 천천히 설명하며 실행
- 참가자가 따라하는 시간 충분히 제공
- 에러가 발생해도 당황하지 말고 함께 해결
- 실제 업무 상황과 연결하여 설명
- 자주 `git status`, `git log` 확인

#### ❌ DON'T
- 너무 빠르게 진행하지 말 것
- 고급 개념(rebase -i, cherry-pick 등)은 나중에
- 이론만 길게 설명하지 말 것
- 참가자가 이해했는지 확인하지 않고 넘어가지 말 것

### Conflict 실습 시나리오 (핵심!)

**기본 시나리오** (반드시 진행):
```bash
# 1. 초기 파일 생성
echo "Hello World" > greeting.txt
git add greeting.txt
git commit -m "Initial commit"

# 2. main 브랜치에서 수정
echo "Hello from Main" > greeting.txt
git commit -am "Update in main"

# 3. feature 브랜치 생성 (이전 커밋에서)
git checkout HEAD~1
git checkout -b feature/update
echo "Hello from Feature" > greeting.txt
git commit -am "Update in feature"

# 4. main으로 돌아가서 병합
git checkout main
git merge feature/update
# CONFLICT!

# 5. 해결
# 방법 1: 수동 해결
# 방법 2: --ours 사용
# 방법 3: --theirs 사용
# 방법 4: Source Tree 사용
```

## 📊 실습 진행 체크

### 참가자 진행 상황 확인 방법

각 실습 후 다음 명령어로 확인:
```bash
# 브랜치 확인
git branch

# 커밋 히스토리 확인
git log --oneline --graph --all

# 태그 확인
git tag

# 파일 상태 확인
git status
```

### 문제 해결 가이드

**참가자가 막혔을 때:**

1. **실습이 꼬였을 때**
   ```bash
   git reset --hard HEAD
   git clean -fd
   ```

2. **브랜치를 잘못 생성했을 때**
   ```bash
   git branch -D wrong-branch
   ```

3. **Conflict 해결 중 실수했을 때**
   ```bash
   git merge --abort
   # 또는
   git rebase --abort
   ```

## 📝 Q&A 예상 질문

### 자주 나오는 질문

**Q: git add와 git commit의 차이는?**
A: add는 staging area에 올리는 것, commit은 저장소에 기록하는 것

**Q: Fast-forward와 3-way merge의 차이는?**
A: Fast-forward는 단순히 포인터만 이동, 3-way는 새로운 병합 커밋 생성

**Q: --ours와 --theirs 중 어떤 걸 사용해야 하나?**
A: ours는 현재 브랜치, theirs는 병합하려는 브랜치. 상황에 따라 선택

**Q: rebase와 merge의 차이는?**
A: 히스토리를 깔끔하게 유지하려면 rebase, 병합 기록을 남기려면 merge

**Q: 이미 push한 커밋을 수정하려면?**
A: `--amend`나 `rebase -i` 사용 가능하지만, 이미 공유된 커밋은 수정 지양

## 🎁 추가 자료

### 핸즈온 후 공유 자료
- Pro Git Book 한글판 링크
- GitHub Learning Lab 추천
- Git Cheat Sheet PDF
- 사내 Git 워크플로우 문서 (있다면)

### 후속 학습 추천
1. Git Flow 실습
2. GitHub/GitLab PR/MR 프로세스
3. CI/CD와 Git 연동
4. Git Hooks 활용

## 📞 강사 준비 사항

### 당일 준비물
- [ ] 노트북 (Git 설치 및 테스트 완료)
- [ ] Source Tree 설치
- [ ] 예제 코드 준비
- [ ] 스크린 공유 환경 테스트
- [ ] 참가자 명단 및 Git 경험 수준 파악

### 시간 배분 (총 4시간 기준)
- **1일차** (2시간)
  - 00:00-00:15 소개 및 환경 설정
  - 00:15-01:00 기본 명령어 실습
  - 01:00-01:10 휴식
  - 01:10-02:00 브랜치 관리 실습

- **2일차** (2시간)
  - 00:00-00:20 복습 및 Conflict 개념
  - 00:20-01:30 **Conflict 해결 실습** ⭐
  - 01:30-01:40 휴식
  - 01:40-02:00 Tag 관리 및 마무리

## ✨ 성공적인 핸즈온을 위한 팁

1. **실습 위주로 진행**: 이론 30%, 실습 70%
2. **작은 성공 경험**: 단계별로 성취감 제공
3. **실무 연결**: "이런 경우에 사용합니다" 예시 제공
4. **에러 환영**: 에러가 발생하면 학습 기회로 활용
5. **반복 연습**: 같은 내용을 다른 방법으로 여러 번

---

**모든 준비가 완료되었습니다! 멋진 핸즈온 되세요! 🚀**
