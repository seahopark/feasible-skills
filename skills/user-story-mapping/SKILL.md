---
name: user-story-mapping
argument-hint: "[product or workflow]"
description: Create a user story map that lays out activities, steps, tasks, and release slices. Use when planning a workflow, backlog, or MVP around the user journey.
intent: >-
  Visualize the user journey by creating a hierarchical map that breaks down high-level activities into steps and tasks, organized left-to-right as a narrative flow. Use this to build shared understanding across product, design, and engineering, prioritize features based on user workflows, and identify gaps or opportunities in the user experience.
source: https://github.com/deanpeters/Product-Manager-Skills/blob/main/skills/user-story-mapping/SKILL.md
type: component
---

## Purpose

Visualize the user journey by creating a hierarchical map that breaks down high-level activities into steps and tasks, organized left-to-right as a narrative flow.

This is not a backlog — it's a strategic artifact that shows *how* users accomplish their goals, which then informs *what* to build.

## Input

**Works best with:** The product or user workflow being mapped.
**Also useful:** The primary user, the end-to-end narrative as you understand it, existing backlog items to place, and release goals.

**Example invocation:** `Story map for our expense-reporting flow, from receipt capture to reimbursement, with an MVP slice for the pilot.`

---

## Key Concepts

### The Jeff Patton Story Mapping Framework

**Horizontal axis (left-to-right):** User journey over time
- **Backbone:** High-level activities the user performs (3–5개)
- **Steps:** Specific actions within each activity (각 3–5개)
- **Tasks:** Detailed work required to complete each step (각 5–7개)

**Vertical axis (top-to-bottom):** Priority and releases
- **Top rows:** Essential tasks (MVP / Release 1)
- **Lower rows:** Nice-to-have tasks (Future releases)

### Story Map Structure

```
Segment → Persona → Narrative (User's goal)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Activity 1] → [Activity 2] → [Activity 3] → [Activity 4] → [Activity 5]
     ↓              ↓              ↓              ↓              ↓
  [Step 1.1]     [Step 2.1]     [Step 3.1]     [Step 4.1]     [Step 5.1]
  [Step 1.2]     [Step 2.2]     [Step 3.2]     [Step 4.2]     [Step 5.2]
     ↓              ↓              ↓              ↓              ↓
  [Task 1.1.1]   [Task 2.1.1]   [Task 3.1.1]   [Task 4.1.1]   [Task 5.1.1]
  [Task 1.1.2]   [Task 2.1.2]   [Task 3.1.2]   [Task 4.1.2]   [Task 5.1.2]
━━━━━━━━━━━━━━ Release 1 (MVP) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  [Task 1.1.3]   [Task 2.1.3]   ...
━━━━━━━━━━━━━━ Release 2 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ...
```

### Anti-Patterns

- **백로그가 아님**: Activity는 제품이 제공하는 기능이 아니라 사용자의 행동
- **간트차트가 아님**: 프로젝트 일정이 아니라 사용자 여정 시각화
- **정적이지 않음**: 사용자를 더 알아갈수록 지속적으로 업데이트

---

## Application

### Step 1: Segment & Persona 정의

```markdown
### Segment:
- [타겟 세그먼트. 예: "50인 이상 자산운용사의 IR 담당자"]

### Persona:
- [이름, 역할, 주요 페인포인트, 목표]
```

**체크:** "사용자"가 아니라 구체적 직군/역할로 명시

---

### Step 2: Narrative 정의

사용자가 달성하려는 것을 Jobs-to-be-Done 문장으로:

```markdown
### Narrative:
- [한 문장. 예: "DDQ를 받아서 정확하고 빠르게 완성해 기관투자자에게 제출한다"]
```

**체크:** "제품을 사용한다"가 아니라 사용자가 원하는 결과로 작성

---

### Step 3: Activities (Backbone) 정의

3–5개의 고수준 Activity. 사용자가 *하는 것* 기준:

```markdown
### Activities:
1. [Activity 1]
2. [Activity 2]
3. [Activity 3]
4. [Activity 4]
5. [Activity 5, optional]
```

**체크:** 순서가 있고(왼→오), 사용자 행동 기준

---

### Step 4: Steps 정의

각 Activity를 3–5개 Step으로 세분화:

```markdown
### Steps:

**Activity 1: [이름]**
- Step 1:
- Step 2:
- Step 3:

**Activity 2: [이름]**
- Step 1:
...
```

**체크:** 관찰 가능한 행동, 논리적 순서

---

### Step 5: Tasks 정의

각 Step을 5–7개 Task로 세분화:

```markdown
### Tasks:

**Activity 1 / Step 1: [이름]**
- Task 1:
- Task 2:
- Task 3:
```

**체크:** 실제로 빌드할 수 있는 단위

---

### Step 6: Release Slices 정의

수평선으로 릴리즈 컷:

```markdown
### Release Slices:

**Release 1 (MVP):**
- [포함 Task 목록 — 핵심 흐름만]

**Release 2:**
- [추가 Task — 협업/자동화 등]

**Release 3 (Growth):**
- [확장 기능]
```

**MVP 기준:** "이 선 위의 Task만으로 사용자가 목표를 달성할 수 있는가?"

---

## Output 형식 (Obsidian 마크다운)

결과물은 아래 구조로 Obsidian에 저장:

```markdown
# [제품명] 유저 스토리 맵

## 컨텍스트
- **Segment:** ...
- **Persona:** ...
- **Narrative:** ...

## Backbone (Activities)
| Activity 1 | Activity 2 | Activity 3 | Activity 4 | Activity 5 |
|---|---|---|---|---|

## Story Map

### Activity 1: [이름]
#### Steps & Tasks
| Step | MVP Tasks | R2 Tasks | R3 Tasks |
|---|---|---|---|

...

## 비기능 요건 (기술 요건)
별도 파일로 분리: `기술 요건.md`

## 업데이트 로그
- YYYY-MM-DD: 초기 작성 (가설 기반)
- YYYY-MM-DD: 팀 워크샵 후 업데이트
```
