# 간단 Conflict 실습 가이드 (Main 브랜치 중심)

> **대상**: Main 브랜치에서 commit, push만 하다가 conflict를 겪는 엔지니어  
> **소요 시간**: 1-1.5시간  
> **목표**: 실무에서 자주 발생하는 conflict 상황을 경험하고 해결법 습득

## 📋 실습 개요

**실제 상황 재현:**
```
팀원 A: main 브랜치에서 작업 → commit → push ✅
팀원 B: main 브랜치에서 작업 → commit → push ❌ (conflict 발생!)
```

이 실습에서는 **한 명이 두 팀원 역할을 모두 수행**하여 conflict를 직접 만들고 해결합니다.

---

## 🚀 실습 준비

### 1. 저장소 이동
```bash
cd git-command-guide/practice
```

### 2. 현재 상태 확인
```bash
git status
git branch
# main 브랜치에 있는지 확인
```

---

## 📝 실습 시나리오: 팀 협업 중 Conflict 발생

### 상황 설명
- 당신과 팀원이 동시에 같은 파일을 수정 중
- 팀원이 먼저 push 완료
- 당신이 push 시도 → **Conflict 발생!**

---

## 🎯 Part 1: 초기 프로젝트 파일 생성 (5분)

```bash
# 1. 프로젝트 상태 파일 생성
cat > project-status.txt << 'EOF'
# 프로젝트 상태 보고서

프로젝트명: Cloud Infrastructure Migration
담당자: 라모스
시작일: 2024-01-15

## 현재 진행 상황
- 계획 수립: 완료
- 환경 구성: 진행 중
- 마이그레이션: 대기 중
- 테스트: 대기 중

## 다음 작업
TBD
EOF

# 2. 초기 커밋
git add project-status.txt
git commit -m "docs: Add project status file"

# 3. 확인
cat project-status.txt
git log --oneline
```

---

## 🎯 Part 2: 팀원 A의 작업 (먼저 완료) (10분)

**시나리오**: 팀원 A가 환경 구성을 완료하고 상태를 업데이트했습니다.

```bash
# 1. 팀원 A의 수정 작업
cat > project-status.txt << 'EOF'
# 프로젝트 상태 보고서

프로젝트명: Cloud Infrastructure Migration
담당자: 라모스
시작일: 2024-01-15
최종 업데이트: 2024-02-20 (A)

## 현재 진행 상황
- 계획 수립: 완료
- 환경 구성: 완료 ✅
- 마이그레이션: 진행 중
- 테스트: 대기 중

## 다음 작업
- 마이그레이션 1차 실행
- 성능 모니터링 설정
EOF

# 2. 커밋 (팀원 A)
git commit -am "update: Complete environment setup (by A)"

# 3. 확인
git log --oneline -2
```

**✅ 팀원 A는 성공적으로 작업 완료!**

---

## 🎯 Part 3: 팀원 B의 작업 (동시 진행) (10분)

**시나리오**: 팀원 B도 같은 시간에 다른 내용을 수정하고 있었습니다.

```bash
# 1. 팀원 B의 작업 시점으로 이동 (A가 커밋하기 전)
git checkout HEAD~1

# 2. 현재 파일 상태 확인
cat project-status.txt
# (초기 상태의 파일을 보게 됨)

# 3. 팀원 B의 수정 작업
cat > project-status.txt << 'EOF'
# 프로젝트 상태 보고서

프로젝트명: Cloud Infrastructure Migration
담당자: 라모스
시작일: 2024-01-15
최종 업데이트: 2024-02-20 (B)

## 현재 진행 상황
- 계획 수립: 완료
- 환경 구성: 진행 중
- 마이그레이션: 대기 중
- 테스트: 준비 중 ⏳

## 다음 작업
- 테스트 환경 구축
- 테스트 케이스 작성
EOF

# 4. 커밋 (팀원 B)
git add project-status.txt
git commit -m "update: Prepare test environment (by B)"

# 5. 확인
git log --oneline -2
```

---

## 🎯 Part 4: Conflict 발생! (10분)

**시나리오**: 팀원 B가 자신의 작업을 main에 반영하려고 합니다.

```bash
# 1. 팀원 B가 main 브랜치로 전환 시도
git checkout main

# ⚠️ 경고 메시지 확인
# "Your local changes to the following files would be overwritten"

# 2. 변경사항을 임시 저장
git checkout -b temp-b-work
git checkout main

# 3. B의 작업을 main에 병합 시도
git merge temp-b-work
```

**🔥 CONFLICT 발생!**

```
Auto-merging project-status.txt
CONFLICT (content): Merge conflict in project-status.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### Conflict 상태 확인

```bash
# 1. 충돌 상태 확인
git status

# 출력:
# You have unmerged paths.
# Unmerged paths:
#   both modified:   project-status.txt

# 2. 충돌 파일 내용 확인
cat project-status.txt
```

**파일 내용 (충돌 마커):**
```
# 프로젝트 상태 보고서

프로젝트명: Cloud Infrastructure Migration
담당자: 라모스
시작일: 2024-01-15
<<<<<<< HEAD
최종 업데이트: 2024-02-20 (A)

## 현재 진행 상황
- 계획 수립: 완료
- 환경 구성: 완료 ✅
- 마이그레이션: 진행 중
- 테스트: 대기 중

## 다음 작업
- 마이그레이션 1차 실행
- 성능 모니터링 설정
=======
최종 업데이트: 2024-02-20 (B)

## 현재 진행 상황
- 계획 수립: 완료
- 환경 구성: 진행 중
- 마이그레이션: 대기 중
- 테스트: 준비 중 ⏳

## 다음 작업
- 테스트 환경 구축
- 테스트 케이스 작성
>>>>>>> temp-b-work
```

### 충돌 마커 이해하기

```
<<<<<<< HEAD          ← 현재 main 브랜치 내용 (팀원 A의 작업)
팀원 A가 수정한 내용
=======               ← 구분선
팀원 B가 수정한 내용
>>>>>>> temp-b-work   ← 병합하려는 브랜치 내용 (팀원 B의 작업)
```

---

## 🎯 Part 5: Conflict 해결 방법 (30-40분)

### 방법 1: 수동 해결 (가장 일반적, 실무 추천)

```bash
# 1. 에디터로 파일 열기
code project-status.txt
# 또는
vim project-status.txt

# 2. 충돌 마커를 제거하고 두 내용을 합치기
# 예시: 두 팀원의 작업을 모두 반영
```

**해결된 파일 예시:**
```
# 프로젝트 상태 보고서

프로젝트명: Cloud Infrastructure Migration
담당자: 라모스
시작일: 2024-01-15
최종 업데이트: 2024-02-20 (A, B)

## 현재 진행 상황
- 계획 수립: 완료
- 환경 구성: 완료 ✅ (A)
- 마이그레이션: 진행 중
- 테스트: 준비 중 ⏳ (B)

## 다음 작업
- 마이그레이션 1차 실행 (A)
- 성능 모니터링 설정 (A)
- 테스트 환경 구축 (B)
- 테스트 케이스 작성 (B)
```

```bash
# 3. 해결 완료 표시
git add project-status.txt

# 4. 상태 확인
git status
# "All conflicts fixed but you are still merging."

# 5. 병합 커밋 생성
git commit -m "merge: Resolve conflict - combine A and B's work

- Include environment setup completion (A)
- Include test preparation (B)
- Merge next tasks from both members"

# 6. 완료 확인
git log --oneline --graph -5
```

---

### 방법 2: VSCode 활용 (GUI 추천)

```bash
# 1. VSCode로 충돌 파일 열기
code project-status.txt
```

**VSCode에서 표시되는 옵션:**
```
<<<<<<< HEAD (Current Change)
[Accept Current Change] [Accept Incoming Change] [Accept Both Changes] [Compare Changes]
```

**옵션 설명:**
- **Accept Current Change**: 현재 내용 (팀원 A) 선택
- **Accept Incoming Change**: 들어오는 내용 (팀원 B) 선택
- **Accept Both Changes**: 두 내용 모두 유지
- **Compare Changes**: 비교 창 열기

```bash
# 2. 원하는 옵션 클릭 후 저장

# 3. 해결 완료
git add project-status.txt
git commit -m "merge: Resolve conflict using VSCode"
```

---

### 방법 3: Source Tree 활용

**단계:**
1. Source Tree 열기
2. 파일 상태에서 충돌 파일(!) 확인
3. 우클릭 → "충돌 해결" 선택
4. 옵션 선택:
   - **"내 것 사용"**: 팀원 A의 내용 (HEAD)
   - **"그들 것 사용"**: 팀원 B의 내용
   - **"Merge Tool 실행"**: 비주얼 도구로 해결
5. 해결 후 "해결됨으로 표시"
6. 커밋 버튼 클릭

---

### 방법 4: 명령어로 한쪽 선택 (빠른 해결)

**팀원 A의 내용을 선택:**
```bash
git checkout --ours project-status.txt
git add project-status.txt
git commit -m "merge: Keep A's changes"
```

**팀원 B의 내용을 선택:**
```bash
git checkout --theirs project-status.txt
git add project-status.txt
git commit -m "merge: Keep B's changes"
```

**⚠️ 주의**: 한쪽 내용만 선택하므로 다른 쪽 작업이 사라집니다!

---

## 🎯 Part 6: 실수했을 때 되돌리기 (10분)

### Conflict 해결 중 실수한 경우

```bash
# 병합 취소하고 처음부터
git merge --abort

# 다시 병합 시도
git merge temp-b-work
```

### 잘못 해결한 커밋을 되돌리기

```bash
# 마지막 커밋 취소 (변경사항 유지)
git reset --soft HEAD~1

# 마지막 커밋 취소 (변경사항도 삭제)
git reset --hard HEAD~1
```

---

## 🎯 Part 7: 추가 실습 - 더 복잡한 Conflict (선택사항)

### 여러 파일 동시 충돌

```bash
# 1. 설정 파일 추가
echo "config.version=1.0" > config.txt
git add config.txt
git commit -m "Add config file"

# 2. main에서 수정
echo "config.version=1.5
config.env=production" > config.txt
git commit -am "Update config for production"

# 3. 다른 브랜치에서 수정
git checkout -b feature-config HEAD~1
echo "config.version=2.0
config.env=development" > config.txt
git commit -am "Update config for development"

# 4. 병합 시도
git checkout main
git merge feature-config
# CONFLICT!

# 5. 해결
cat > config.txt << 'EOF'
config.version=2.0
config.env=production
config.updated=true
EOF

git add config.txt
git commit -m "merge: Resolve config conflict"
```

---

## ✅ 실습 완료 체크리스트

- [ ] 초기 프로젝트 파일 생성
- [ ] 팀원 A의 작업 시뮬레이션
- [ ] 팀원 B의 작업 시뮬레이션
- [ ] Conflict 발생 확인
- [ ] 충돌 마커 이해 (`<<<<<<<`, `=======`, `>>>>>>>`)
- [ ] 수동으로 Conflict 해결
- [ ] VSCode에서 Conflict 해결 (선택)
- [ ] Source Tree에서 Conflict 해결 (선택)
- [ ] `git merge --abort`로 취소 연습
- [ ] 최종 히스토리 확인

---

## 🔍 실습 후 확인 사항

### 히스토리 확인
```bash
# 그래프로 병합 히스토리 보기
git log --oneline --graph --all

# 예상 출력:
# *   a1b2c3d (HEAD -> main) merge: Resolve conflict
# |\  
# | * d4e5f6g (temp-b-work) update: Prepare test environment (by B)
# * | g7h8i9j update: Complete environment setup (by A)
# |/  
# * j0k1l2m docs: Add project status file
```

### 파일 최종 상태 확인
```bash
cat project-status.txt
```

### 브랜치 정리
```bash
# 실습용 브랜치 삭제
git branch -d temp-b-work
```

---

## 💡 실무 팁

### Conflict 예방 방법
1. **자주 pull 받기**: 작업 시작 전 `git pull origin main`
2. **작은 단위로 커밋**: 큰 변경사항은 여러 번에 나눠서
3. **소통하기**: 같은 파일 작업 시 팀원과 조율
4. **브랜치 사용**: Feature 브랜치에서 작업 후 PR

### Conflict 발생 시 대처
1. **당황하지 말기**: Conflict는 자연스러운 현상
2. **백업 확인**: `git stash` 또는 브랜치 생성
3. **천천히 확인**: 양쪽 변경사항 모두 검토
4. **테스트**: 해결 후 반드시 동작 확인
5. **팀원 확인**: 불확실하면 상대방과 상의

### 자주 사용하는 명령어
```bash
# 현재 상태 확인
git status

# 충돌 파일만 보기
git diff --name-only --diff-filter=U

# 병합 취소
git merge --abort

# 이전 상태로 되돌리기
git reset --hard HEAD~1
```

---

## 📚 추가 학습 자료

### 더 깊이 학습하고 싶다면
- **[docs/03-conflict-resolution.md](./docs/03-conflict-resolution.md)** - Conflict 해결 완벽 가이드
- **[hands-on-practice.md](./hands-on-practice.md)** - 종합 실습 가이드

### 빠른 참고
- **[QUICKSTART.md](./QUICKSTART.md)** - 5분 퀵스타트
- **[README.md](./README.md)** - 전체 가이드 개요

---

## 🎯 다음 단계

### 실무에서 적용하기
1. **다음 작업부터**: 작업 전 `git pull` 습관화
2. **충돌 발생 시**: 이 가이드 참고하여 해결
3. **팀 공유**: 팀원들과 conflict 해결 방법 공유

### 고급 주제 (관심 있다면)
- Git Flow 워크플로우
- Rebase를 사용한 히스토리 관리
- Git Hooks 활용
- CI/CD 파이프라인 연동

---

## ❓ FAQ

**Q: Conflict가 자주 발생하는데 어떻게 하나요?**  
A: 1) 자주 pull 받기, 2) 작업 전 브랜치 분리, 3) 팀원과 작업 영역 조율

**Q: 충돌 마커를 실수로 남겨둔 채 커밋했어요.**  
A: `git reset --soft HEAD~1`로 커밋 취소 후 다시 수정

**Q: 어떤 해결 방법이 가장 좋나요?**  
A: 수동 해결이 가장 안전합니다. 양쪽 변경사항을 모두 검토할 수 있습니다.

**Q: main 브랜치에서만 작업하면 안 되나요?**  
A: 소규모 팀은 가능하지만, 브랜치를 사용하면 더 안전하게 협업할 수 있습니다.

---

## 🎉 실습 완료!

축하합니다! 이제 실무에서 발생하는 기본적인 Git Conflict를 해결할 수 있습니다.

**기억할 핵심:**
- ✅ Conflict는 자연스러운 현상
- ✅ 충돌 마커(`<<<<<<<`, `=======`, `>>>>>>>`)를 이해하면 쉬움
- ✅ 여러 해결 방법 중 상황에 맞게 선택
- ✅ 실수해도 되돌릴 수 있음 (`git merge --abort`)

**Happy Git! 🚀**
