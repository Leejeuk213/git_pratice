# Git Tag 관리 가이드

## 목차
- [Tag란?](#tag란)
- [Tag 종류](#tag-종류)
- [Tag 생성](#tag-생성)
- [Tag 조회](#tag-조회)
- [Tag 공유](#tag-공유)
- [Tag 삭제](#tag-삭제)
- [버전 관리 전략](#버전-관리-전략)

## Tag란?

**Tag**는 특정 커밋을 가리키는 참조(reference)로, 주로 **릴리스 버전을 표시**하는 데 사용됩니다. 브랜치와 달리 한 번 생성되면 이동하지 않는 고정된 포인터입니다.

### Tag vs Branch
| 특징 | Tag | Branch |
|------|-----|--------|
| 이동 | 고정 (불변) | 이동 (새 커밋 추가) |
| 용도 | 버전 표시, 마일스톤 | 기능 개발, 작업 분리 |
| 수정 | 불가 (삭제 후 재생성) | 가능 |
| 주 사용처 | 릴리스, 배포 | 개발 작업 |

## Tag 종류

### 1. Lightweight Tag (경량 태그)
특정 커밋을 가리키는 단순한 포인터

```bash
# 경량 태그 생성
git tag v1.0.0

# 특정 커밋에 경량 태그
git tag v1.0.0 commit-hash
```

**특징:**
- 태그 이름만 저장
- 추가 정보 없음
- 임시 마킹용으로 적합

### 2. Annotated Tag (주석 태그)
태그 정보를 포함하는 Git 객체

```bash
# 주석 태그 생성
git tag -a v1.0.0 -m "Release version 1.0.0"

# 상세 정보와 함께 생성 (에디터 열림)
git tag -a v1.0.0

# 특정 커밋에 주석 태그
git tag -a v1.0.0 commit-hash -m "Release version 1.0.0"
```

**특징:**
- 태그 작성자 정보 포함
- 태그 생성 날짜 포함
- 태그 메시지 포함
- GPG 서명 가능
- **공식 릴리스에 권장**

## Tag 생성

### 기본 사용법

```bash
# 현재 커밋에 태그 생성
git tag v1.0.0

# 메시지와 함께 주석 태그 생성
git tag -a v1.0.0 -m "First stable release"

# 이전 커밋에 태그 생성
git tag -a v1.0.0 abc1234 -m "Release v1.0.0"
```

### 버전 넘버링 규칙 (Semantic Versioning)

**형식: MAJOR.MINOR.PATCH**

```bash
# Major 버전: 하위 호환성이 깨지는 변경
git tag -a v2.0.0 -m "Major release with breaking changes"

# Minor 버전: 하위 호환성을 유지하는 기능 추가
git tag -a v1.1.0 -m "Add new features"

# Patch 버전: 버그 수정
git tag -a v1.0.1 -m "Fix critical bugs"
```

**예시:**
- `v1.0.0` → 첫 번째 안정 버전
- `v1.1.0` → 새로운 기능 추가
- `v1.1.1` → 버그 수정
- `v2.0.0` → 주요 변경 (Breaking Changes)

### Pre-release 태그

```bash
# Alpha 버전
git tag -a v1.0.0-alpha -m "Alpha release"
git tag -a v1.0.0-alpha.1 -m "Alpha release 1"

# Beta 버전
git tag -a v1.0.0-beta -m "Beta release"
git tag -a v1.0.0-beta.2 -m "Beta release 2"

# Release Candidate
git tag -a v1.0.0-rc.1 -m "Release candidate 1"
```

### 서명된 태그 (GPG)

```bash
# GPG로 서명된 태그 생성
git tag -s v1.0.0 -m "Signed release v1.0.0"

# 서명 확인
git tag -v v1.0.0
```

## Tag 조회

### Tag 목록 보기

```bash
# 모든 태그 나열
git tag

# 알파벳 순 정렬
git tag -l

# 패턴으로 필터링
git tag -l "v1.*"
git tag -l "v2.0.*"

# 최근 태그만 보기
git describe --tags

# 날짜순 정렬
git tag --sort=-creatordate
```

### Tag 상세 정보 확인

```bash
# 태그 상세 정보 보기
git show v1.0.0

# 경량 태그 정보
git show v1.0.0 --quiet

# 여러 태그 비교
git show v1.0.0 v2.0.0
```

### Tag와 커밋 관계

```bash
# 특정 태그가 가리키는 커밋
git rev-list -n 1 v1.0.0

# 특정 커밋을 포함하는 태그
git tag --contains commit-hash

# 현재 커밋과 가장 가까운 태그
git describe --tags

# 현재 브랜치의 모든 태그
git tag --merged
```

## Tag 공유

### 원격 저장소에 푸시

```bash
# 특정 태그 푸시
git push origin v1.0.0

# 모든 태그 푸시
git push origin --tags

# 주석 태그만 푸시
git push origin --follow-tags

# 특정 태그를 다른 이름으로 푸시
git push origin refs/tags/v1.0.0:refs/tags/release-1.0.0
```

### 원격 태그 가져오기

```bash
# 원격 저장소의 모든 태그 가져오기
git fetch --tags

# 특정 태그 가져오기
git fetch origin tag v1.0.0

# 태그와 함께 pull
git pull --tags
```

### 태그로 체크아웃

```bash
# 태그로 이동 (detached HEAD 상태)
git checkout v1.0.0

# 태그에서 새 브랜치 생성
git checkout -b hotfix/v1.0.1 v1.0.0
```

## Tag 삭제

### 로컬 태그 삭제

```bash
# 로컬 태그 삭제
git tag -d v1.0.0

# 여러 태그 동시 삭제
git tag -d v1.0.0 v1.0.1 v1.0.2

# 패턴으로 삭제
git tag -l "v1.0.*" | xargs git tag -d
```

### 원격 태그 삭제

```bash
# 원격 태그 삭제 (방법 1)
git push origin --delete v1.0.0

# 원격 태그 삭제 (방법 2)
git push origin :refs/tags/v1.0.0

# 로컬과 원격 태그 동시 삭제
git tag -d v1.0.0
git push origin --delete v1.0.0
```

### 태그 수정 (재생성)

```bash
# 1. 기존 태그 삭제
git tag -d v1.0.0
git push origin --delete v1.0.0

# 2. 새로운 태그 생성
git tag -a v1.0.0 commit-hash -m "Updated release notes"

# 3. 강제 푸시
git push origin v1.0.0
```

## 버전 관리 전략

### Main 브랜치 기준 태그 관리

**권장 워크플로우:**

```bash
# 1. main 브랜치에서 최신 상태 확인
git checkout main
git pull origin main

# 2. 버전 태그 생성
git tag -a v1.0.0 -m "Release v1.0.0

- Feature A implementation
- Bug fix for issue #123
- Performance improvements"

# 3. 태그 푸시
git push origin v1.0.0

# 또는 태그와 함께 푸시
git push origin main --follow-tags
```

### Release 브랜치와 태그

```bash
# 1. Release 브랜치 생성
git checkout -b release/v1.0.0 develop

# 2. 릴리스 준비 (버전 넘버 업데이트 등)
echo "1.0.0" > VERSION
git commit -am "Bump version to 1.0.0"

# 3. main에 병합
git checkout main
git merge --no-ff release/v1.0.0

# 4. 태그 생성
git tag -a v1.0.0 -m "Release version 1.0.0"

# 5. develop에도 병합
git checkout develop
git merge --no-ff release/v1.0.0

# 6. release 브랜치 삭제
git branch -d release/v1.0.0

# 7. 푸시
git push origin main develop --tags
```

### Hotfix 태그 관리

```bash
# 1. main에서 hotfix 브랜치 생성
git checkout -b hotfix/v1.0.1 v1.0.0

# 2. 버그 수정
git commit -am "Fix critical bug"

# 3. main에 병합
git checkout main
git merge --no-ff hotfix/v1.0.1

# 4. Patch 버전 태그
git tag -a v1.0.1 -m "Hotfix release v1.0.1"

# 5. develop에도 병합
git checkout develop
git merge --no-ff hotfix/v1.0.1

# 6. 푸시
git push origin main develop --tags
```

## Source Tree에서 Tag 관리

### Tag 생성
1. 태그를 생성할 커밋 선택
2. 우클릭 → "태그..." 선택
3. 태그 이름 입력 (예: v1.0.0)
4. "주석 태그 생성" 체크
5. 메시지 입력
6. "태그 추가" 클릭

### Tag 푸시
1. 좌측 사이드바 "태그" 섹션에서 태그 확인
2. 태그 우클릭 → "태그 푸시..." 선택
3. 원격 저장소 선택
4. "확인" 클릭

또는:
1. 상단 "푸시" 버튼 클릭
2. "모든 태그 푸시" 체크박스 선택
3. "확인" 클릭

### Tag 삭제
1. 좌측 사이드바 "태그" 섹션
2. 삭제할 태그 우클릭
3. "삭제..." 선택
4. "원격에서도 삭제" 체크 (필요 시)
5. "확인" 클릭

### Tag 체크아웃
1. 태그 우클릭
2. "체크아웃..." 선택
3. 새 브랜치 생성 옵션 선택 (권장)
4. "확인" 클릭

## 실습 예제

### 시나리오 1: 첫 릴리스 태깅

```bash
# 1. main 브랜치로 전환
git checkout main

# 2. 현재 상태 확인
git log --oneline -5

# 3. v1.0.0 태그 생성
git tag -a v1.0.0 -m "Initial stable release v1.0.0

Features:
- User authentication
- Dashboard implementation
- API integration

Tested and ready for production"

# 4. 태그 확인
git tag -l
git show v1.0.0

# 5. 원격에 푸시
git push origin v1.0.0
```

### 시나리오 2: 버전 업데이트

```bash
# 1. 새로운 기능 개발 완료
git checkout main
git pull origin main

# 2. Minor 버전 업데이트 (1.0.0 → 1.1.0)
git tag -a v1.1.0 -m "Release v1.1.0

New Features:
- Export to PDF
- Email notifications
- Advanced search

Improvements:
- Performance optimization
- UI/UX enhancements"

# 3. 푸시
git push origin v1.1.0
```

### 시나리오 3: Hotfix 릴리스

```bash
# 1. 현재 프로덕션 버전에서 브랜치 생성
git checkout -b hotfix/critical-fix v1.1.0

# 2. 버그 수정
echo "bug fixed" >> fix.txt
git add fix.txt
git commit -m "Fix critical security vulnerability"

# 3. main에 병합
git checkout main
git merge --no-ff hotfix/critical-fix

# 4. Patch 버전 태그 (1.1.0 → 1.1.1)
git tag -a v1.1.1 -m "Hotfix v1.1.1

Critical Fixes:
- Security vulnerability patch
- Data validation improvement"

# 5. 푸시
git push origin main --follow-tags

# 6. hotfix 브랜치 삭제
git branch -d hotfix/critical-fix
```

## 고급 기능

### Tag 자동화 스크립트

```bash
#!/bin/bash
# create-release-tag.sh

VERSION=$1
MESSAGE=$2

if [ -z "$VERSION" ] || [ -z "$MESSAGE" ]; then
    echo "Usage: ./create-release-tag.sh <version> <message>"
    exit 1
fi

# main 브랜치 확인
BRANCH=$(git branch --show-current)
if [ "$BRANCH" != "main" ]; then
    echo "Error: Must be on main branch"
    exit 1
fi

# 최신 상태 확인
git pull origin main

# 태그 생성
git tag -a "v$VERSION" -m "$MESSAGE"

# 푸시
git push origin "v$VERSION"

echo "Tag v$VERSION created and pushed successfully"
```

사용법:
```bash
chmod +x create-release-tag.sh
./create-release-tag.sh 1.2.0 "Release v1.2.0 with new features"
```

### 버전 번호 추출

```bash
# 최신 태그 버전
LATEST_TAG=$(git describe --tags --abbrev=0)
echo "Latest version: $LATEST_TAG"

# 버전 번호 파싱
VERSION=$(echo $LATEST_TAG | sed 's/v//')
MAJOR=$(echo $VERSION | cut -d. -f1)
MINOR=$(echo $VERSION | cut -d. -f2)
PATCH=$(echo $VERSION | cut -d. -f3)

echo "Major: $MAJOR, Minor: $MINOR, Patch: $PATCH"

# 다음 버전 계산
NEXT_PATCH="v$MAJOR.$MINOR.$((PATCH + 1))"
NEXT_MINOR="v$MAJOR.$((MINOR + 1)).0"
NEXT_MAJOR="v$((MAJOR + 1)).0.0"

echo "Next patch: $NEXT_PATCH"
echo "Next minor: $NEXT_MINOR"
echo "Next major: $NEXT_MAJOR"
```

### Tag 기반 배포

```bash
# 최신 태그로 배포
LATEST_TAG=$(git describe --tags --abbrev=0)
git checkout $LATEST_TAG

# 배포 스크립트 실행
./deploy.sh

# main으로 복귀
git checkout main
```

## 베스트 프랙티스

### 1. 태그 네이밍 컨벤션
```bash
# ✅ 좋은 예
v1.0.0
v1.2.3
v2.0.0-beta.1
release-2024.10.13

# ❌ 나쁜 예
version1
release
final
```

### 2. 항상 주석 태그 사용
```bash
# ✅ 주석 태그 (권장)
git tag -a v1.0.0 -m "Release v1.0.0"

# ❌ 경량 태그 (임시용만)
git tag v1.0.0
```

### 3. 의미 있는 메시지 작성
```bash
git tag -a v1.0.0 -m "Release v1.0.0

Features:
- Feature A
- Feature B

Bug Fixes:
- Fix issue #123
- Fix issue #456

Breaking Changes:
- API endpoint changed from /api/v1 to /api/v2"
```

### 4. Main 브랜치만 태깅
```bash
# ✅ main 브랜치에서 태깅
git checkout main
git tag -a v1.0.0 -m "Release"

# ❌ feature 브랜치에서 태깅 (피하기)
git checkout feature/new-feature
git tag -a v1.0.0 -m "Release"
```

## 트러블슈팅

### 같은 이름의 태그 충돌
```bash
# 로컬과 원격이 다른 커밋을 가리킬 때
git fetch --tags --force

# 원격 태그로 덮어쓰기
git tag -f v1.0.0 origin/v1.0.0
```

### 태그 푸시 실패
```bash
# 원격에 이미 같은 태그가 있을 때
# 1. 로컬 태그 삭제
git tag -d v1.0.0

# 2. 원격 태그 가져오기
git fetch origin tag v1.0.0

# 또는 강제 푸시 (주의!)
git push -f origin v1.0.0
```

### Detached HEAD 상태 해결
```bash
# 태그 체크아웃 후
git checkout v1.0.0
# HEAD detached at v1.0.0 경고

# 해결: 새 브랜치 생성
git checkout -b hotfix/from-v1.0.0
```

## 버전 관리 체크리스트

릴리스 전 확인사항:

- [ ] 모든 기능 테스트 완료
- [ ] 버그 수정 확인
- [ ] 문서 업데이트
- [ ] CHANGELOG.md 작성
- [ ] main 브랜치 최신 상태 확인
- [ ] 적절한 버전 번호 결정
- [ ] 태그 메시지 작성
- [ ] 태그 생성 및 푸시
- [ ] 릴리스 노트 작성
- [ ] 팀에 알림

## 다음 단계

Tag 관리를 마스터했다면:
- [핸즈온 실습 가이드](../hands-on-practice.md) - 종합 실습
- [Git 워크플로우 고급](./05-advanced-workflows.md) - 고급 주제
