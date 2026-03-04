# Deep Work Plugin

AI 코딩 도구 사용 시 **기획과 코딩의 철저한 분리**를 강제하는 3단계 워크플로우 플러그인.

## 문제

AI 코딩 도구가 복잡한 작업을 수행할 때 흔히 발생하는 문제:
- 기존 아키텍처를 무시하고 새로운 패턴을 도입
- 이미 존재하는 유틸리티를 중복 구현
- 코드베이스를 충분히 이해하기 전에 구현 시작
- 요청하지 않은 "개선"을 추가하여 버그 유발

## 해결책

**Research → Plan → Implement** 3단계 워크플로우로 분석, 계획, 구현을 엄격히 분리합니다.

- **Phase 1 (Research)**: 코드베이스를 깊이 분석하여 문서화
- **Phase 2 (Plan)**: 분석 결과를 바탕으로 상세 구현 계획 작성, 사용자 승인
- **Phase 3 (Implement)**: 승인된 계획을 기계적으로 실행

Phase 1, 2에서는 **코드 파일 수정이 물리적으로 차단**됩니다 (PreToolUse 훅).

## 사용법

```bash
# 1. 세션 시작
/deep-work "JWT 기반 사용자 인증 구현"

# 2. 코드베이스 분석
/deep-research

# 3. deep-work/research.md 검토 후 계획 작성
/deep-plan

# 4. deep-work/plan.md 검토, 피드백, 반복
#    만족스러우면 "승인" 입력

# 5. 구현 실행
/deep-implement

# 상태 확인 (아무 때나)
/deep-status
```

## 커맨드

| 커맨드 | 설명 |
|--------|------|
| `/deep-work <task>` | 세션 초기화, 작업 디렉토리 생성 |
| `/deep-research` | Phase 1: 코드베이스 분석 → `research.md` |
| `/deep-plan` | Phase 2: 구현 계획 작성 → `plan.md` |
| `/deep-implement` | Phase 3: 계획 실행 |
| `/deep-status` | 현재 상태 및 진행률 확인 |

## 산출물

- `deep-work/research.md` — 코드베이스 분석 결과
- `deep-work/plan.md` — 상세 구현 계획 (체크리스트 포함)
- `.claude/deep-work.local.md` — 세션 상태 (phase, 승인 여부 등)

## Phase Guard

`hooks/scripts/phase-guard.sh`가 Write/Edit 도구 호출을 감시합니다:

- **Research/Plan 단계**: `deep-work/` 내 문서와 상태 파일만 수정 허용
- **Implement 단계**: 모든 파일 수정 허용
- **세션 비활성**: 제한 없음

## 설치

### 방법 1: GitHub Marketplace (권장)

```bash
# 1. 마켓플레이스 추가
/plugin marketplace add Sungmin-Cho/sseocho-plugins

# 2. 플러그인 설치
/plugin install deep-work@sseocho
```

### 방법 2: npm

```bash
npm install @sseocho/claude-deep-work
```

### 방법 3: 로컬 (개발용)

이 저장소를 `~/.claude/plugins/deep-work/`에 클론합니다.
Claude Code가 자동으로 플러그인을 감지합니다.
