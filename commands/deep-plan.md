---
allowed-tools: Read, Grep, Glob, Agent, Write, Bash
description: "Phase 2: Deep planning - create a detailed implementation plan"
---

# Phase 2: Deep Planning

You are in the **Planning** phase of a Deep Work session.

## Critical Constraints

🚫 **DO NOT implement anything.**
🚫 **DO NOT modify any source code files.**
🚫 **DO NOT create implementation files.**
✅ **ONLY plan and document the plan in `deep-work/plan.md`.**

## Instructions

### 1. Verify prerequisites

Read `.claude/deep-work.local.md` and verify:
- `current_phase` is "plan"
- `research_complete` is true

If not, inform the user which prerequisite step is missing.

Also read `deep-work/research.md` to load the research findings.

### 2. Check for user feedback

Read `deep-work/plan.md` if it already exists — the user may have added feedback notes in the form of:
- `> [!NOTE]` callouts
- `<!-- HUMAN: ... -->` comments
- Inline comments or strikethroughs

If feedback exists, incorporate it into the updated plan.

### 3. Create the implementation plan

Write `deep-work/plan.md` with the following structure:

```markdown
# Implementation Plan: [Task Title]

## Overview
Brief description of what will be implemented and the approach chosen.

## Architecture Decision
Why this approach was chosen over alternatives. Reference research findings.

## Files to Modify

### [File path 1]
- **Action**: Create / Modify / Delete
- **Changes**: Detailed description
- **Code sketch**:
  ```language
  // Pseudocode or actual code snippet showing the change
  ```
- **Reason**: Why this change is needed
- **Risk**: Low / Medium / High — explanation

### [File path 2]
...

## Execution Order
1. First: [file/change] — because [reason]
2. Then: [file/change] — depends on step 1
3. ...

## Dependency Analysis
- [Change A] must happen before [Change B] because...
- [Change C] is independent and can happen anytime

## Trade-offs
| Option | Pros | Cons | Chosen? |
|--------|------|------|---------|
| Option A | ... | ... | ✅ |
| Option B | ... | ... | ❌ |

## Rollback Strategy
If something goes wrong:
1. `git stash` or `git reset` to [commit]
2. Specific rollback steps...

## Task Checklist

- [ ] Task 1: [File path] — [What to do] — [Why]
- [ ] Task 2: [File path] — [What to do] — [Why]
- [ ] Task 3: [File path] — [What to do] — [Why]
...

## Open Questions
- Any unresolved decisions for the user to weigh in on
```

### 4. Present for review

Display:

```
📋 구현 계획이 작성되었습니다!

📄 계획서: deep-work/plan.md

📊 계획 요약:
  - 변경 파일 수: N개
  - 신규 파일: N개
  - 수정 파일: N개
  - 태스크 수: N개
  - 위험도: 낮음/중간/높음

⚠️  아직 구현을 시작하지 않습니다!

👉 다음 단계:
  1. deep-work/plan.md 를 꼼꼼히 검토하세요
  2. 수정이 필요하면:
     - 파일에 직접 메모를 추가하거나 (> [!NOTE], <!-- HUMAN: -->)
     - 채팅으로 피드백을 주세요
  3. /deep-plan 을 다시 실행하면 피드백을 반영합니다
  4. 계획이 만족스러우면 "승인" 이라고 입력하세요
```

### 5. Handle approval

When the user says "승인", "approve", "approved", "LGTM", or similar approval words:

Update `.claude/deep-work.local.md`:
- Set `plan_approved: true`
- Set `current_phase: implement`
- Increment `iteration_count`
- Add a progress log entry

Display:

```
✅ 계획이 승인되었습니다!

⚡ 현재 상태: Implement 단계로 전환됨
   - 코드 파일 수정이 허용됩니다
   - plan.md의 체크리스트에 따라 구현합니다

👉 /deep-implement 를 실행하여 구현을 시작하세요
```

### 6. Handle iteration

If the user provides feedback instead of approval, update the plan accordingly and re-present for review. Track the iteration count.
