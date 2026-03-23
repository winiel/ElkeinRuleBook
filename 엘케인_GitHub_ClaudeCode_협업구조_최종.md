# 엘케인 + GitHub + Claude Code 협업 구조 (최종 확정)

**작성일**: 2026년 3월 16일  
**목적**: GitHub를 브릿지로 사용한 엘케인-Claude Code 완전 자동화 구조

---

## 🎯 핵심 아이디어

### GitHub를 중간 브릿지로 사용!

```
엘케인 (OpenClaw)
  ↓ GitHub Issue 또는 PR Comment 생성
GitHub (저장소)
  ↓ GitHub Actions 자동 트리거
Claude Code GitHub Action
  ↓ 코드 생성 → Pull Request 생성
GitHub (PR 대기)
  ↓ 알림 (Telegram/Slack)
엘케인 (OpenClaw)
  ↓ PR 리뷰 + Approve
GitHub (자동 Merge)
```

**이 방법은 Claude Code 공식 지원 기능입니다!** ✅

---

## ✅ 왜 이 방법이 완벽한가?

### 1. 완전 자동화 가능
- ✅ 엘케인이 GitHub Issue/PR Comment만 생성
- ✅ GitHub Actions가 자동으로 Claude Code 실행
- ✅ PR이 자동으로 생성됨
- ✅ 엘케인이 PR 리뷰만 하면 끝

### 2. API 독립성 문제 해결
- ✅ **Anthropic API 키는 GitHub Actions에서 사용**
- ✅ 엘케인 API와 완전 분리
- ✅ Claude Pro 구독 불필요! (API만 있으면 됨)

### 3. 비동기 처리
- ✅ 엘케인이 기다릴 필요 없음
- ✅ Claude Code가 백그라운드에서 작업
- ✅ 완료 시 GitHub 알림

### 4. Git 이력 자동 관리
- ✅ 모든 변경사항이 PR로 기록
- ✅ 코드 리뷰 히스토리 보존
- ✅ Blame, History 추적 가능

### 5. CLAUDE.md 자동 인식
- ✅ GitHub Actions가 자동으로 CLAUDE.md 읽음
- ✅ 프로젝트 컨텍스트 유지

---

## 🏗️ 구조 설계

### 전체 워크플로우

```
┌─────────────────────────────────────────────────────────┐
│  Step 1: 엘케인이 GitHub Issue 생성                        │
├─────────────────────────────────────────────────────────┤
│  엘케인 (OpenClaw)                                        │
│    ↓ exec: gh issue create --title "..." --body "@claude ..." │
│  GitHub Issue #123 생성                                   │
│    Title: "FastAPI 로그인 API 구현"                        │
│    Body: "@claude FastAPI로 POST /api/v1/auth/login 구현  │
│           - JWT 토큰 발급                                 │
│           - bcrypt 해싱                                   │
│           - 테스트 코드 포함"                              │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  Step 2: GitHub Actions 자동 트리거                        │
├─────────────────────────────────────────────────────────┤
│  GitHub                                                  │
│    ↓ Issue Comment "@claude" 감지                        │
│  GitHub Actions 워크플로우 실행                            │
│    - .github/workflows/claude.yml                        │
│    - anthropics/claude-code-action@v1                   │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  Step 3: Claude Code가 코드 생성                           │
├─────────────────────────────────────────────────────────┤
│  Claude Code GitHub Action                              │
│    1. 저장소 체크아웃                                      │
│    2. CLAUDE.md 읽기 (프로젝트 컨텍스트)                    │
│    3. Issue 본문 분석                                     │
│    4. 코드 생성:                                          │
│       - app/routers/auth.py (로그인 API)                  │
│       - tests/test_auth.py (테스트 코드)                  │
│    5. Pull Request 생성                                   │
│       - Branch: claude/issue-123-login-api               │
│       - Title: "feat: Implement login API (#123)"        │
│       - Description: "Closes #123\n\n생성된 파일:\n- ..."  │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  Step 4: 엘케인이 PR 리뷰                                  │
├─────────────────────────────────────────────────────────┤
│  GitHub                                                  │
│    ↓ PR #124 생성 알림 (Telegram)                         │
│  엘케인 (OpenClaw)                                        │
│    1. exec: gh pr view 124 --json files                 │
│    2. 코드 리뷰 (Anthropic API 사용)                       │
│       - 보안 체크 (SQL injection, XSS)                    │
│       - CodingGuide.md 준수 확인                          │
│       - 테스트 커버리지 확인                               │
│    3. 리뷰 결과:                                          │
│       - ✅ Approve: exec: gh pr review 124 --approve     │
│       - ❌ Request Changes: gh pr review 124 --comment "..." │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  Step 5: 자동 Merge (선택)                                │
├─────────────────────────────────────────────────────────┤
│  GitHub                                                  │
│    ↓ 엘케인 Approve 시                                    │
│  자동 Merge (GitHub Actions 또는 수동)                     │
│    - Branch 삭제                                         │
│    - Issue #123 자동 Close                               │
└─────────────────────────────────────────────────────────┘
```

---

## 💰 비용 구조 (재계산)

### Anthropic API 사용량

#### 엘케인 (OpenClaw)
```
1. Issue 생성 (업무 분석)
   - 업무당 ~3,000 토큰
   - 하루 10개 업무 × 90일 = 900개
   - 총 토큰: 2,700,000 토큰
   - 비용: $8.1 (Input) + $40.5 (Output) = $48.6

2. PR 리뷰
   - PR당 ~5,000 토큰 (코드 읽기 + 분석)
   - 하루 5개 PR × 90일 = 450개
   - 총 토큰: 2,250,000 토큰
   - 비용: $6.75 (Input) + $33.75 (Output) = $40.5

엘케인 총 비용: $89.1 (₩122,000)
```

#### GitHub Actions (Claude Code)
```
코드 생성 (PR 생성)
   - PR당 ~50,000 토큰 (코드 작성 + 테스트)
   - 하루 5개 PR × 90일 = 450개
   - 총 토큰: 22,500,000 토큰
   - 비용: $67.5 (Input) + $337.5 (Output) = $405

GitHub Actions 총 비용: $405 (₩554,000)
```

### 총 비용 (3개월)

| 항목 | 비용 (USD) | 비용 (KRW) |
|------|-----------|-----------|
| **엘케인 API** | $89.1 | ₩122,000 |
| **GitHub Actions API** | $405 | ₩554,000 |
| **GitHub Actions 런타임** | $0 (무료 2,000분) | ₩0 |
| **총계** | **$494.1** | **₩676,000** |

### 비교

| 방식 | 비용 (3개월) | 절감률 |
|------|-------------|--------|
| 엘케인 단독 | $648 (₩885,000) | - |
| **GitHub + Claude Code** | **$494.1 (₩676,000)** | **24% ↓** |

**주의**: 
- 이전 분석($100.5)은 Claude Pro 구독을 가정했으나, 실제로는 별도 API
- GitHub Actions도 Anthropic API를 직접 사용하므로 토큰 비용 발생
- **하지만 여전히 절감!** (24%)

---

## ⚙️ 구현 가이드

### 1단계: GitHub 저장소 설정

#### 1-1. Private Repository 생성
```bash
# GitHub CLI 사용
gh repo create winiel/ActiveLearningMultiAgentPrototype --private

# 또는 웹에서 생성
https://github.com/new
  - Name: ActiveLearningMultiAgentPrototype
  - Visibility: Private ✅
  - Initialize: README, .gitignore (Python), MIT License
```

#### 1-2. CLAUDE.md 파일 생성
```bash
# 프로젝트 루트에 생성
touch CLAUDE.md
```

**CLAUDE.md 내용**:
```markdown
# Project RG (ALMAP) - 학원 출석 관리 시스템

## 프로젝트 개요
- Multi-Agent Vision 협업 기반 학원 출석 관리 시스템
- Edge AI (Raspberry Pi) + Cloud (AWS)

## 기술 스택
- Backend: FastAPI 0.110+ + Python 3.13
- Database: MariaDB 10.6+ (NOT PostgreSQL)
- Frontend: React 18 + Bootstrap 5 (Mobile-First)
- Infrastructure: AWS (EC2 + RDS + S3 + CloudWatch)
- Vision AI: AWS Rekognition + Gemini Vision + Claude Vision

## 코딩 규칙 (CodingGuide.md 준수)

### Python
- PEP 8 준수
- snake_case (NOT kebab-case)
- Type hints 필수
- Docstring (Google style)

### API
- RESTful 원칙
- Prefix: /api/v1/
- HTTP 상태 코드 정확히 사용
- 에러 응답: {"error": {"code": "...", "message": "..."}}

### Database
- 테이블명: snake_case, 복수형 (students, academies)
- 컬럼명: snake_case
- Primary Key: {table}_id (student_id, NOT id)
- Timestamp: created_at, updated_at
- MariaDB: `ON UPDATE CURRENT_TIMESTAMP` 사용 (NOT trigger)
- Foreign Key 명시적 선언

### 보안
- AWS credentials: 환경변수 (.env) 또는 Secrets Manager
- 절대 하드코딩 금지
- API Key: 환경변수
- 비밀번호: bcrypt 해싱
- SQL Injection 방지: Parameterized Query

### 테스트
- 모든 API는 pytest 테스트 포함
- Coverage 80% 이상
- 파일명: test_{module}.py

## 금지 사항
❌ 하드코딩 (credentials, API keys)
❌ PostgreSQL 사용 (MariaDB만!)
❌ kebab-case 파일명 (Python은 snake_case)
❌ 민감한 정보 로그 출력
❌ root 계정 사용 (DB)

## 참조 문서
- CodingGuide.md (프로젝트 루트)
- AwsServiceGuide.md
- DatabaseGuide.md
```

---

### 2단계: GitHub Actions 설정

#### 2-1. Anthropic API 키 발급
```bash
# Anthropic Console에서 API 키 발급
https://console.anthropic.com/settings/keys

# API 키 복사 (예시)
sk-ant-api03-...
```

#### 2-2. GitHub Secrets 설정
```bash
# GitHub CLI 사용
gh secret set ANTHROPIC_API_KEY --body "sk-ant-api03-..."

# 또는 웹에서 설정
https://github.com/winiel/ActiveLearningMultiAgentPrototype/settings/secrets/actions
  - New repository secret
  - Name: ANTHROPIC_API_KEY
  - Value: sk-ant-api03-...
```

#### 2-3. GitHub 앱 설치 (추천)

**방법 A: Terminal CLI 사용 (가장 쉬움)**
```bash
# Claude Code CLI 설치
curl -fsSL https://claude.ai/install.sh | bash

# Claude Code 로그인
claude

# GitHub 앱 자동 설치
/install-github-app
# → 브라우저에서 자동으로 열림
# → GitHub 앱 승인
# → 저장소 선택: ActiveLearningMultiAgentPrototype
```

**방법 B: 수동 설치**
```bash
# 1. GitHub 앱 설치
https://github.com/apps/claude
  - Install → Select repositories
  - ActiveLearningMultiAgentPrototype 선택

# 2. 앱 ID 및 Private Key 확인
https://github.com/settings/apps
  - Claude 앱 선택
  - App ID 복사
  - Generate a private key → 다운로드

# 3. GitHub Secrets 추가
gh secret set APP_ID --body "123456"
gh secret set APP_PRIVATE_KEY --body "$(cat downloaded-key.pem)"
```

#### 2-4. GitHub Actions 워크플로우 파일 생성

```bash
# 디렉토리 생성
mkdir -p .github/workflows

# 워크플로우 파일 생성
touch .github/workflows/claude.yml
```

**.github/workflows/claude.yml**:
```yaml
name: Claude Code Actions

permissions:
  contents: write
  pull-requests: write
  issues: write
  id-token: write

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]

jobs:
  claude-code:
    # @claude 멘션이 있을 때만 실행
    if: |
      (github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'pull_request_review_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'issues' && contains(github.event.issue.body, '@claude'))
    
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Generate GitHub App token
        id: app-token
        uses: actions/create-github-app-token@v2
        with:
          app-id: ${{ secrets.APP_ID }}
          private-key: ${{ secrets.APP_PRIVATE_KEY }}
      
      - name: Run Claude Code
        uses: anthropics/claude-code-action@v1
        with:
          github_token: ${{ steps.app-token.outputs.token }}
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          claude_args: |
            --max-turns 10
            --model claude-sonnet-4-6
```

#### 2-5. Git Push
```bash
git add .github/workflows/claude.yml CLAUDE.md
git commit -m "feat: Add Claude Code GitHub Actions"
git push origin main
```

---

### 3단계: 엘케인 워크플로우 구현

#### 3-1. GitHub CLI 설치 (엘케인 환경)
```bash
# Raspberry Pi / Linux
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh

# 로그인
gh auth login
  - GitHub.com
  - HTTPS
  - Authenticate with your GitHub credentials
```

#### 3-2. 엘케인 스크립트 예시

**엘케인이 할 일**:
```python
# 1. 업무 분석 (Anthropic API 사용)
업무 = "로그인 API 만들어줘"
상세_프롬프트 = claude_api_call(f"""
업무: {업무}

다음을 포함한 상세 개발 지시사항을 작성해줘:
- API 엔드포인트 경로
- Request/Response 스키마
- 보안 요구사항 (bcrypt, JWT 등)
- 테스트 케이스
- CodingGuide.md 준수 사항

형식: GitHub Issue Body
""")

# 2. GitHub Issue 생성 (exec 명령)
result = exec(f"""
gh issue create \
  --repo winiel/ActiveLearningMultiAgentPrototype \
  --title "FastAPI 로그인 API 구현" \
  --body "{상세_프롬프트}\n\n@claude 위 내용대로 구현해줘"
""")

# 3. Issue 번호 추출
issue_number = parse_issue_number(result)
print(f"✅ Issue #{issue_number} 생성 완료")
print(f"🔄 Claude Code가 자동으로 작업을 시작합니다...")

# 4. PR 생성 대기 (비동기)
# 엘케인은 다른 작업 계속 진행...
```

**PR 생성 알림 시** (webhook 또는 polling):
```python
# 5. PR 리뷰 (Anthropic API 사용)
pr_number = 124  # GitHub webhook에서 받음

# PR 파일 가져오기
pr_files = exec(f"gh pr view {pr_number} --json files -q '.files[] | .path'")

# 각 파일 내용 읽기
code = ""
for file_path in pr_files.split('\n'):
    content = exec(f"gh pr view {pr_number} --json files -q '.files[] | select(.path==\"{file_path}\") | .additions'")
    code += f"\n# {file_path}\n{content}\n"

# 코드 리뷰 (Anthropic API)
review = claude_api_call(f"""
다음 코드를 리뷰해줘:

{code}

체크 항목:
- 보안 (SQL injection, XSS, 하드코딩된 credentials)
- CodingGuide.md 준수 (snake_case, docstring, type hints)
- 테스트 커버리지
- 버그 가능성

형식: GitHub PR Review Comment
""")

# 6. 리뷰 결과에 따라 Approve 또는 Request Changes
if "보안 이슈" in review or "버그" in review:
    exec(f'gh pr review {pr_number} --request-changes --body "{review}"')
    print("❌ 수정 요청")
else:
    exec(f'gh pr review {pr_number} --approve --body "{review}"')
    print("✅ Approve 완료")
    
    # 자동 Merge (선택)
    exec(f'gh pr merge {pr_number} --squash --delete-branch')
    print("🎉 Merge 완료!")
```

---

### 4단계: 실제 사용 예시

#### 시나리오 1: FastAPI 엔드포인트 10개 생성

**위니엘님 → 엘케인**:
```
"FastAPI로 학생 관리 CRUD API 10개 만들어줘"
```

**엘케인 → GitHub Issue**:
```bash
gh issue create \
  --title "학생 관리 CRUD API 구현" \
  --body "@claude FastAPI로 다음 API들을 구현해줘:

1. POST /api/v1/students - 학생 생성
2. GET /api/v1/students - 학생 목록 조회 (pagination)
3. GET /api/v1/students/{student_id} - 학생 상세 조회
4. PUT /api/v1/students/{student_id} - 학생 정보 수정
5. DELETE /api/v1/students/{student_id} - 학생 삭제 (logical)
6. POST /api/v1/students/bulk - 학생 일괄 등록
7. GET /api/v1/students/search - 학생 검색
8. POST /api/v1/students/{student_id}/photo - 학생 사진 업로드
9. GET /api/v1/students/{student_id}/attendance - 출석 기록 조회
10. POST /api/v1/students/{student_id}/parents - 학부모 연결

요구사항:
- CLAUDE.md 준수
- MariaDB students 테이블 사용
- 모든 API에 pytest 테스트 포함
- Pagination: limit, offset 파라미터
- 에러 핸들링 포함"
```

**GitHub Actions → Claude Code**:
- 자동으로 10개 API 생성
- 테스트 코드 작성
- PR 생성 (30분~1시간 소요)

**엘케인 → PR 리뷰**:
- 보안, 품질 체크
- Approve → 자동 Merge

---

#### 시나리오 2: 버그 수정

**위니엘님 → 엘케인**:
```
"로그인 API에서 JWT 토큰이 None으로 나와"
```

**엘케인 → GitHub Issue**:
```bash
gh issue create \
  --title "Bug: 로그인 API JWT 토큰 None 반환" \
  --body "@claude app/routers/auth.py의 login 함수에서 JWT 토큰이 None으로 반환되는 버그가 있어.

재현 방법:
POST /api/v1/auth/login
{
  \"email\": \"test@example.com\",
  \"password\": \"password123\"
}

예상: {\"access_token\": \"eyJ...\"}
실제: {\"access_token\": null}

조사 후 수정해줘."
```

**GitHub Actions → Claude Code**:
- 코드 분석
- 버그 원인 파악 (예: JWT secret 환경변수 누락)
- 수정 코드 작성
- PR 생성

---

## 🎯 장단점 분석

### ✅ 장점

1. **완전 자동화**
   - 엘케인이 Issue만 생성하면 끝
   - Claude Code가 자동으로 코드 작성
   - PR이 자동으로 생성됨

2. **API 독립성 문제 해결**
   - GitHub Actions가 별도 Anthropic API 사용
   - Claude Pro 구독 불필요
   - 비용 예측 가능

3. **비동기 처리**
   - 엘케인이 기다릴 필요 없음
   - 다른 작업 병렬 진행 가능

4. **Git 이력 관리**
   - 모든 변경사항이 PR로 기록
   - 코드 리뷰 히스토리 보존
   - Rollback 용이

5. **팀 확장 가능**
   - 다른 개발자도 @claude 사용 가능
   - 일관된 코딩 스타일 (CLAUDE.md)

6. **보안**
   - Private repository
   - API 키는 GitHub Secrets에 암호화
   - PR 리뷰 후 Merge (승인 과정)

### ⚠️ 단점

1. **GitHub Actions 런타임 제한**
   - 무료: 월 2,000분
   - 1회 실행: 5~30분
   - 월 40~400회 실행 가능
   - 초과 시: $0.008/분 (약 ₩11/분)

2. **비용이 예상보다 높음**
   - 이전 분석: $100.5 (Claude Pro 가정)
   - 실제: $494.1 (GitHub Actions도 API 사용)
   - 하지만 여전히 24% 절감

3. **네트워크 의존성**
   - GitHub.com 장애 시 작동 불가
   - API 응답 지연 가능

4. **디버깅 어려움**
   - GitHub Actions 로그 확인 필요
   - 로컬에서 테스트 불가

---

## 💡 최적화 전략

### 비용 절감

#### 1. max-turns 제한
```yaml
claude_args: |
  --max-turns 5  # 10 → 5로 줄임 (토큰 50% 절감)
```

#### 2. 작은 작업 묶기
```bash
# 나쁜 예: Issue 10개 (각각 Claude 호출)
gh issue create --body "@claude API 1개 만들어줘"
gh issue create --body "@claude API 1개 만들어줘"
... (10번)

# 좋은 예: Issue 1개 (Claude 1번 호출)
gh issue create --body "@claude API 10개 만들어줘\n1. ...\n2. ..."
```

#### 3. 프롬프트 최적화
```bash
# 나쁜 예: 장황한 설명
"FastAPI로 학생을 생성하는 API를 만들어줘. 이 API는 POST 메서드를 사용하고, /api/v1/students 경로에 있어야 해. Request는 name, age, grade를 받고..."

# 좋은 예: 간결한 지시
"@claude CRUD API 생성:
- Model: Student (name, age, grade)
- Endpoints: POST/GET/PUT/DELETE /api/v1/students
- CLAUDE.md 준수"
```

#### 4. 캐싱 활용
- CLAUDE.md에 공통 지침 → 매번 반복 설명 불필요
- GitHub Actions 캐싱 (dependencies)

---

## 🎯 최종 추천 구조

### 실제 사용 구조

```
위니엘님
  ↓ "로그인 API 만들어줘"
엘케인 (OpenClaw)
  ↓ 업무 분석 (Anthropic API)
  ↓ 상세 프롬프트 생성
  ↓ exec: gh issue create --body "@claude ..."
GitHub Issue #123
  ↓ @claude 멘션 감지
GitHub Actions (자동 트리거)
  ↓ claude-code-action@v1 실행
Claude Code (GitHub Actions 내)
  ↓ Anthropic API 호출 (별도 API 키)
  ↓ CLAUDE.md 읽기
  ↓ 코드 생성 (10~30분)
  ↓ Pull Request #124 생성
GitHub (알림)
  ↓ Telegram/Slack 알림
엘케인 (OpenClaw)
  ↓ exec: gh pr view 124
  ↓ 코드 리뷰 (Anthropic API)
  ↓ exec: gh pr review 124 --approve
GitHub (자동 Merge)
  ↓ Issue #123 Close
완료 🎉
```

---

## 📊 최종 비용 비교

| 방식 | 3개월 비용 | 장점 | 단점 |
|------|-----------|------|------|
| **엘케인 단독** | $648 (₩885,000) | 완전 자동화, 구조 간단 | 비용 높음 |
| **GitHub + Claude Code** | **$494.1 (₩676,000)** | 24% 절감, Git 이력 관리 | 설정 복잡 |
| ~~Claude Pro 구독~~ | ~~$100.5~~ | ~~큰 절감~~ | ❌ 불가능 (API 별개) |

**추천**: **GitHub + Claude Code** ✅
- ✅ 비용 24% 절감
- ✅ 완전 자동화 가능
- ✅ Git 이력 자동 관리
- ✅ 팀 확장 가능
- ✅ CLAUDE.md 자동 인식

---

## 🚀 실행 계획

### 즉시 시작 (오늘)

1. **GitHub Private Repository 생성** (5분)
2. **CLAUDE.md 파일 작성** (30분)
3. **Anthropic API 키 발급** (5분)
4. **GitHub Secrets 설정** (5분)
5. **GitHub 앱 설치** (10분)
6. **GitHub Actions 워크플로우 생성** (15분)

### 테스트 (내일)

1. **첫 Issue 생성** (엘케인 → GitHub)
2. **Claude Code 실행 확인** (GitHub Actions)
3. **PR 생성 확인**
4. **엘케인 리뷰 테스트**

### 본격 개발 (1주 후~)

- 매일 5~10개 Issue 생성
- Claude Code가 자동으로 PR 생성
- 엘케인이 PR 리뷰 + Approve
- 3개월 후 프로토타입 완성 ✅

---

## 📌 결론

### ✅ 위니엘님의 아이디어가 완벽합니다!

**"GitHub를 중간 브릿지로"**는:
1. ✅ Claude Code 공식 지원 기능
2. ✅ 완전 자동화 가능
3. ✅ API 독립성 문제 해결
4. ✅ 비용 24% 절감
5. ✅ Git 이력 자동 관리

**이 방법을 강력 추천합니다!** 🎯

---

**작성자**: 엘케인 (OpenClaw)  
**참조**: https://code.claude.com/docs/ko/github-actions  
**문의**: Telegram @winiel001
