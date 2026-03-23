# 엘케인 규칙집 (Elkein RuleBook)

엘케인(Elkein) AI 어시스턴트의 모든 운영 규칙과 정책을 관리하는 저장소입니다.

## 📋 문서 목록

### 핵심 정체성
- **[IDENTITY.md](IDENTITY.md)** - 엘케인의 정체성 (이름, 역할, 이모지)
- **[SOUL.md](SOUL.md)** - 페르소나 및 행동 원칙
- **[USER.md](USER.md)** - 사용자 정보 및 선호도

### 운영 정책
- **[AGENTS.md](AGENTS.md)** - 에이전트 작업 지침 및 워크플로우
- **[TOKEN_EFFICIENCY_POLICY.md](TOKEN_EFFICIENCY_POLICY.md)** - 토큰 사용 효율화 정책 ⭐
- **[LLM_STRATEGY.md](LLM_STRATEGY.md)** - LLM 모델 선택 전략

### 개발 가이드
- **[DEV_CHECKLIST.md](DEV_CHECKLIST.md)** - 개발 전 필수 체크리스트
- **[CodingGuide.md](CodingGuide.md)** - 코딩 규칙 및 프로젝트 구조
- **[DatabaseGuide.md](DatabaseGuide.md)** - DB/테이블/컬럼 네이밍 규칙
- **[AwsServiceGuide.md](AwsServiceGuide.md)** - AWS 리소스 네이밍 및 태깅
- **[GithubBasicGuide.md](GithubBasicGuide.md)** - GitHub 기본 사용법
- **[GithubBranchStrategyGuide.md](GithubBranchStrategyGuide.md)** - Git 브랜치 전략

### 협업 가이드
- **[엘케인_ClaudeCode_협업_분석.md](엘케인_ClaudeCode_협업_분석.md)** - Claude Code와의 협업 구조
- **[엘케인_GitHub_ClaudeCode_협업구조_최종.md](엘케인_GitHub_ClaudeCode_협업구조_최종.md)** - GitHub Actions 통합 협업
- **[엘케인_AI코딩에디터_협업_분석.md](엘케인_AI코딩에디터_협업_분석.md)** - AI 코딩 도구 협업 분석

### 기능 설정 가이드
- **[엘케인_음성대화_설정가이드.md](엘케인_음성대화_설정가이드.md)** - TTS/STT 음성 대화 설정

### 도구 및 환경
- **[TOOLS.md](TOOLS.md)** - 로컬 환경 설정 (카메라, SSH 등)
- **[HEARTBEAT.md](HEARTBEAT.md)** - 주기적 체크 규칙

## 🔄 업데이트 정책

이 저장소는 엘케인의 규칙 변경 시 자동으로 업데이트됩니다.

### 변경 이력
- **2026-03-23 18:56**: 초기 저장소 생성 및 8개 규칙 문서 추가
- **2026-03-23 18:57**: 개발 가이드 10개 추가 (총 19개 문서)

## 🔗 관련 링크

- **OpenClaw**: https://github.com/openclaw/openclaw
- **OpenClaw Docs**: https://docs.openclaw.ai
- **CodingGuide**: https://github.com/winiel/CodingGuide

## 📝 사용 방법

OpenClaw workspace에서 이 저장소를 clone하여 사용:

```bash
cd /home/winielab/.openclaw/workspace
git clone https://github.com/winiel/ElkeinRuleBook.git
```

규칙 업데이트 시:

```bash
cd ElkeinRuleBook
git pull
```

---

**관리자**: 위니엘 (Winiel Sohn)  
**생성일**: 2026-03-23  
**라이선스**: Private
