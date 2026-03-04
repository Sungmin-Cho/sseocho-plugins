---
allowed-tools: Bash, Read, Write, Glob
description: "Start a deep work session with 3-phase workflow (Research → Plan → Implement)"
argument-hint: task description
---

# Deep Work Session Initialization

You are initializing a **Deep Work** session — a structured 3-phase workflow that enforces strict separation between planning and coding.

## Your Task

The user wants to work on: **$ARGUMENTS**

## Instructions

Follow these steps exactly:

### 1. Create the deep-work output directory

Create a `deep-work/` directory in the project root if it doesn't already exist.

```bash
mkdir -p deep-work
```

### 2. Create placeholder files

Create these empty files if they don't exist:
- `deep-work/research.md` — will be filled during Phase 1
- `deep-work/plan.md` — will be filled during Phase 2

### 3. Select work mode

Ask the user to choose the work mode using AskUserQuestion:

```
🤝 작업 모드를 선택하세요:
  1. Solo — 혼자 진행 (기본)
  2. Team — Agent Team으로 병렬 진행
```

**If the user selects Team:**

1. Check the environment variable:
   ```bash
   echo "${CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS:-not_set}"
   ```

2. If the result is `not_set` or empty, display the following and fall back to Solo:
   ```
   ⚠️ Agent Teams 기능이 활성화되지 않았습니다.

   활성화 방법:
     ~/.claude/settings.json 에 다음을 추가하세요:
     {
       "env": {
         "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
       }
     }
     저장 후 Claude Code를 재시작하세요.

   ℹ️ Solo 모드로 전환하여 진행합니다.
   ```
   Set `team_mode` to `solo`.

3. If the variable is set (any non-empty value), set `team_mode` to `team`.

**If the user selects Solo or default:** Set `team_mode` to `solo`.

### 4. Create the state file

Create or overwrite `.claude/deep-work.local.md` with the following content. Use the current timestamp and the determined `team_mode` value.

```markdown
---
current_phase: research
task_description: "$ARGUMENTS"
iteration_count: 0
research_complete: false
plan_approved: false
team_mode: <solo or team>
started_at: "<current ISO timestamp>"
---

# Deep Work Session

## Task
$ARGUMENTS

## Progress Log
- [$(date)] Phase 1 (Research) started
```

### 5. Confirm and guide

Display the following to the user:

```
✅ Deep Work 세션이 시작되었습니다!

📋 작업: $ARGUMENTS
🤝 작업 모드: Solo / Team (Agent Team)

🔄 워크플로우:
  Phase 1: /deep-research  ← 현재 단계
  Phase 2: /deep-plan
  Phase 3: /deep-implement

⚡ 현재 상태: Research 단계
   - 코드 파일 수정이 차단됩니다
   - deep-work/ 내 문서만 작성 가능합니다

👉 다음 단계: /deep-research 를 실행하여 코드베이스 분석을 시작하세요.
```

If `team_mode` is `team`, add the following after the mode line:
```
   - /deep-research: 3명의 분석 에이전트가 병렬로 코드베이스 분석
   - /deep-implement: 파일 소유권 기반으로 작업을 에이전트에게 분배
```

**IMPORTANT**: Do NOT start researching or writing code automatically. Wait for the user to explicitly run `/deep-research`.
