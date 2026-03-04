---
allowed-tools: Read, Glob
description: "Check the current deep work session status and progress"
---

# Deep Work Status

Display the current state of the Deep Work session.

## Instructions

### 1. Check if a session exists

Look for `.claude/deep-work.local.md`. If it doesn't exist, display:

```
ℹ️  활성화된 Deep Work 세션이 없습니다.

새 세션을 시작하려면: /deep-work <작업 설명>
```

### 2. Read state and artifacts

Read the following files:
- `.claude/deep-work.local.md` — session state
- `deep-work/research.md` — if it exists, check if it has content
- `deep-work/plan.md` — if it exists, count checklist progress

### 3. Calculate progress

From `deep-work/plan.md`, count:
- Total tasks: number of lines matching `- [ ]` or `- [x]`
- Completed tasks: number of lines matching `- [x]`
- Progress percentage: completed / total * 100

### 4. Display status

Show a comprehensive status report. If the `team_mode` field is missing from the state file, treat it as "Solo" (backward compatibility).

```
📊 Deep Work 세션 상태
━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 작업: [task description]
🕐 시작: [started_at]
🔄 반복 횟수: [iteration_count]
🤝 작업 모드: [Solo / Team]

📍 현재 단계: [Phase name with emoji]
   🔬 Phase 1 (Research): [✅ 완료 / ⏳ 진행중 / ⬜ 대기]
   📐 Phase 2 (Plan):     [✅ 승인됨 / ⏳ 진행중 / ⬜ 대기]
   🔨 Phase 3 (Implement):[✅ 완료 / ⏳ 진행중 / ⬜ 대기]

📈 구현 진행률: [N/M 완료 (XX%)]
   ████████░░ XX%

📁 산출물:
   - deep-work/research.md: [존재함 ✅ / 없음 ⬜]
   - deep-work/plan.md: [존재함 ✅ / 없음 ⬜]

👉 다음 행동: [안내 메시지]
```

Adjust the "다음 행동" based on the current phase:
- **research**: `/deep-research 를 실행하세요`
- **plan**: `/deep-plan 을 실행하세요` (or "plan.md를 검토하고 승인하세요" if plan exists)
- **implement**: `/deep-implement 를 실행하세요`
- **idle**: `세션이 완료되었습니다. 새 세션: /deep-work <작업>`
