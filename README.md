# git-command-guide

사내 신규 입사자 및 git 활용이 필요한 클라우드 엔지니어를 위한 Git 명령어 및 Source Tree 사용 가이드입니다.

## 📚 학습 목표

- Git의 기본 명령어 이해 및 실습
- Branch 생성, 관리, 병합 방법 학습
- Conflict 발생 시 해결 방법 마스터
- Tag를 활용한 버전 관리 실습
- 실무에서 활용 가능한 Git 워크플로우 습득

## 📖 가이드 문서

### ⭐ [간단 Conflict 실습 가이드](./SIMPLE_CONFLICT_PRACTICE.md) - **실무 중심 추천!**
Main 브랜치 중심의 실무 상황을 재현한 간단한 실습입니다.
- 팀 협업 중 발생하는 실제 conflict 상황
- 1-1.5시간 집중 실습
- 수동/VSCode/Source Tree 3가지 해결 방법
- **초보자에게 가장 적합한 시작점**

---

### 1. [기본 명령어 가이드](./docs/01-basic-commands.md)
Git의 기본적인 명령어를 학습합니다.
- Git 설정 및 초기화
- 파일 추가, 커밋, 푸시
- 변경사항 확인 및 히스토리 조회
- Stash 활용법

### 2. [브랜치 관리 가이드](./docs/02-branch-management.md)
브랜치 생성, 전환, 병합 방법을 학습합니다.
- 브랜치 개념 이해
- Fast-forward vs 3-way 병합
- Rebase 활용
- 브랜치 전략 (Git Flow, GitHub Flow)

### 3. [Conflict 해결 가이드](./docs/03-conflict-resolution.md)
Merge conflict 발생 시 해결 방법을 학습합니다.
- Conflict 발생 원인 이해
- 충돌 마커 읽기
- 수동 해결 vs 자동 해결 (`--ours`, `--theirs`)
- VSCode 및 Source Tree에서 해결하기

### 4. [Tag 관리 가이드](./docs/04-tag-management.md)
버전 관리를 위한 Tag 사용법을 학습합니다.
- Tag 종류 (Lightweight, Annotated)
- Semantic Versioning
- 릴리스 태그 생성 및 관리
- Main 브랜치 기준 태그 전략

### 5. [핸즈온 실습 가이드](./hands-on-practice.md)
실전 시나리오 기반 종합 실습을 진행합니다.
- 단계별 실습 예제
- Text 파일 기반 conflict 처리 실습
- 팀 프로젝트 시뮬레이션
- Source Tree 활용 실습

## 🚀 Getting Started

### 필수 요구사항
로컬 환경에 다음 도구들이 설치되어 있어야 합니다:
- **Git**: [다운로드](https://git-scm.com/downloads)
- **Source Tree**: [다운로드](https://www.sourcetreeapp.com/) (선택사항)
- **텍스트 에디터**: VSCode 권장 [다운로드](https://code.visualstudio.com/)

### 저장소 Clone

```bash
git clone https://github.com/alanhakhyeonsong/git-command-guide.git
cd git-command-guide
```

### 실습 준비

```bash
# 사용자 정보 설정
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"

# 설정 확인
git config --list

# 실습용 디렉토리로 이동
cd practice
```

## 🎯 빠른 시작

### Option 1: 간단 실습 (추천 ⭐)
**1-1.5시간**으로 핵심만 빠르게 학습하고 싶다면:
```bash
# 간단 Conflict 실습 가이드 따라하기
open SIMPLE_CONFLICT_PRACTICE.md
```
- Main 브랜치 중심 실무 상황
- Conflict 발생부터 해결까지 완벽 실습
- 초보자에게 최적화

### Option 2: 종합 학습
**4시간** 전체 커리큘럼을 통해 체계적으로 학습:
```bash
# 핸즈온 실습 가이드 따라하기
open hands-on-practice.md
```

### Option 3: 주제별 학습
필요한 부분만 골라서 학습:
- 기본이 필요하면: `docs/01-basic-commands.md`
- 브랜치가 궁금하면: `docs/02-branch-management.md`
- Conflict만 집중: `docs/03-conflict-resolution.md`

## 📅 핸즈온 일정 (참고용)

### 간단 실습 (1-1.5시간) ⭐ 추천
**Main 브랜치 중심 실무 실습** - `SIMPLE_CONFLICT_PRACTICE.md`
- Conflict 발생 상황 재현 (20분)
- 수동/VSCode/Source Tree로 해결 (40분)
- 추가 시나리오 및 Q&A (20-30분)

### 전체 실습 (4시간)
**1일차: Git 기본 및 브랜치**
- 기본 명령어 실습 (1시간)
- 브랜치 생성 및 병합 (1시간)

**2일차: Conflict 해결 및 Tag 관리**
- Text 파일 기반 Conflict 실습 (1.5시간)
- Tag 생성 및 버전 관리 (0.5시간)

## 🎯 학습 로드맵

```
[시작] 
  ↓
[간단 실습: SIMPLE_CONFLICT_PRACTICE.md] ← 여기서 시작 추천!
  ├─> 충돌 해결 경험 ✅
  └─> 실무 적용 가능 ✅
  ↓
[필요시 추가 학습]
  ├─> 기본 명령어 (docs/01)
  ├─> 브랜치 관리 (docs/02)
  ├─> Conflict 심화 (docs/03)
  └─> Tag 관리 (docs/04)
  ↓
[종합 실습: hands-on-practice.md]
  └─> 전체 워크플로우 마스터
```

## 🎯 이전 학습 로드맵 (참고용)

```
1. 기본 명령어 학습
   └─> Git add, commit, push, pull 이해
   
2. 브랜치 관리
   └─> 브랜치 생성, 전환, 병합 실습
   
3. Conflict 해결 ⭐ (핵심!)
   └─> Text 파일로 충돌 만들고 해결하기
   
4. Tag 관리
   └─> 버전 태깅 및 릴리스 관리
   
5. 종합 실습
   └─> 실전 시나리오 실습
```

## 💡 핸즈온 실습 내용

### 간단 실습: Main 브랜치 Conflict 시나리오 ⭐

**실제 업무 상황 그대로!**
```
상황: 팀원 A와 B가 동시에 같은 파일을 수정
결과: Push 시 Conflict 발생!
해결: 3가지 방법으로 해결 실습
```

**실습 파일**: `SIMPLE_CONFLICT_PRACTICE.md`
- 프로젝트 상태 파일로 실습 (`.txt` 파일)
- 팀 협업 상황 재현
- 수동 해결 / VSCode / Source Tree 모두 연습

### 종합 실습: Conflict 실습 시나리오 (심화)

실습에서는 간단한 **text 파일**을 사용하여 다음과 같은 충돌 상황을 직접 만들고 해결합니다:

1. **기본 Merge Conflict**
   - `greeting.txt` 파일에서 충돌 발생시키기
   - 충돌 마커 이해하기
   - 수동으로 해결하기

2. **여러 파일 Conflict**
   - `config.txt`, `version.txt` 동시 충돌
   - 파일별로 다른 해결 전략 적용

3. **해결 옵션 실습**
   - `git checkout --ours` 사용
   - `git checkout --theirs` 사용
   - 수동 병합

4. **Source Tree 활용**
   - GUI 도구로 충돌 해결
   - Merge Tool 사용법

## 📁 저장소 구조

```
git-command-guide/
├── README.md                         # 이 파일
├── SIMPLE_CONFLICT_PRACTICE.md       # ⭐ 간단 실습 가이드 (추천!)
├── QUICKSTART.md                     # 5분 퀵스타트
├── HANDS_ON_CHECKLIST.md            # 실습 체크리스트
├── INSTRUCTOR_GUIDE.md              # 강사용 가이드
├── docs/                        # 가이드 문서
│   ├── 01-basic-commands.md    # 기본 명령어
│   ├── 02-branch-management.md # 브랜치 관리
│   ├── 03-conflict-resolution.md # Conflict 해결
│   └── 04-tag-management.md    # Tag 관리
├── hands-on-practice.md        # 실습 가이드
└── practice/                   # 실습용 디렉토리 (생성 예정)
```

## 🔥 핵심 명령어 치트시트

### 기본 작업
```bash
git add .                    # 모든 변경사항 스테이징
git commit -m "message"      # 커밋
git push origin main         # 푸시
git pull origin main         # 풀
```

### 브랜치 작업
```bash
git branch feature/name      # 브랜치 생성
git checkout feature/name    # 브랜치 전환
git checkout -b feature/name # 생성 + 전환
git merge feature/name       # 병합
```

### Conflict 해결
```bash
git status                   # 충돌 파일 확인
git checkout --ours file.txt # 현재 브랜치 선택
git checkout --theirs file.txt # 병합 브랜치 선택
git add file.txt             # 해결 완료 표시
git commit                   # 병합 커밋
```

### Tag 관리
```bash
git tag v1.0.0               # 경량 태그
git tag -a v1.0.0 -m "msg"   # 주석 태그
git push origin v1.0.0       # 태그 푸시
git push --tags              # 모든 태그 푸시
```

## 🤝 참여 방법

질문이나 개선 사항이 있으시면:
1. Issue 생성
2. Pull Request 제출
3. 담당자에게 직접 문의

## 📚 추가 학습 자료

- [Pro Git Book (한글)](https://git-scm.com/book/ko/v2)
- [Git 공식 문서](https://git-scm.com/doc)
- [GitHub Git Cheat Sheet](https://training.github.com/downloads/github-git-cheat-sheet/)
- [Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials)

## ✨ 팁

- 실습 전에 각 가이드 문서를 먼저 읽어보세요
- 명령어를 직접 타이핑하면서 연습하세요
- 막히면 `git status`로 현재 상태를 확인하세요
- 실수해도 괜찮습니다! Git은 대부분 되돌릴 수 있습니다

---

**Happy Learning! 🚀**

문의: HakHyeon Song