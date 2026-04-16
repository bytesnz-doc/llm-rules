---
name: flow-driven-dev
description: Flow-Driven Development (FDD) — a structured AI-friendly methodology that starts from a PRD, identifies and details user flows and software flows (grouped by functional area), derives an architecture, prioritises, creates Taskplane tasks, and implements with TDD. Use when the user wants to plan a feature or project end-to-end, says "let's do FDD", "plan the flows", "break this into flows", or wants to go from requirements to ready-to-run tasks.
---

# Flow-Driven Development (FDD)

A methodology designed for AI-assisted development. It bridges the gap between
a PRD and executable tasks by forcing explicit flow identification before
implementation begins — preventing the common failure mode of building
components without knowing how they connect.

## Overview

```
PRD (includes stack decisions)
 └─► Flow Discovery    → User Flows + Software Flows (broad list)
      └─► Flow Detailing  → Each flow fully specified; may uncover new needs
           └─► Architecture → Synthesise what the flows actually require
                └─► Prioritisation → Dependency order + value ranking
                     └─► Task Creation  → Taskplane tasks (one per flow or cohort)
                          └─► TDD Impl   → Red → Green → Refactor per flow
```

Work through phases **in order**, confirming with the user at each transition
before proceeding. Never skip a phase.

---

## Phase 1 — PRD

**Goal:** A concise, implementation-ready product spec that includes high-level
technology decisions.

If a PRD already exists, read it and confirm it is ready before proceeding.
If not, help the user write one.

Apply the `prd-quality` skill criteria to evaluate completeness. Key gates:
- Problem statement + actors are clear
- Success criteria are measurable
- Scope boundaries are explicit (in/out of scope)
- Technology decisions are named at choice level (not implementation level):
  - Stack / ecosystem
  - App framework / runtime
  - Auth approach
  - Persistence approach
  - Key providers or libraries to use or avoid
  - Testing baseline

These decisions anchor the architecture step later. The PRD names *what* was
chosen and *why* — not how to structure the code.

**Output:** `specifications/PRD.md` (or confirmed existing PRD).

**Transition check:** Ask — *"Is the PRD ready to move on, or are there open questions?"*

---

## Phase 2 — Flow Discovery

**Goal:** A complete list of all flows the system must support, organised by
functional area, derived from the user's brain-dump.

### The FUNCTIONS.md brain-dump file

`specifications/FUNCTIONS.md` is the user's raw input file. It is intentionally
loose — no required format. The user writes whatever comes to mind: rough
descriptions, partial ideas, single words, grouped or flat. The agent interprets
and categorises, not the user.

If the file doesn't exist yet, ask the user to create it:

> "Create `specifications/FUNCTIONS.md` and dump everything the app needs to do
> into it — as rough as you like. We'll work through it together."

Example of valid brain-dump content:

```markdown
# Functions

- user can register
- user can login
- forgot password
- on login failure, lock after 5 attempts
- admin can view all users
- admin can deactivate a user
- user creates a project
- invite someone to a project
- on invite, send email
- projects have members with roles
- user can export their data
```

### Processing FUNCTIONS.md

Read the file and process each **unprocessed item** (items without a `[x]` mark
or a `→ ID` annotation). For each item:

1. **Classify** — is this a User Flow or a Software Flow?
   - User-initiated action → User Flow
   - Triggered by an event or schedule → Software Flow
   - Ambiguous — ask the user before assigning

2. **Assign a functional area** — which domain does this belong to?
   Use existing areas from FLOWS.md if they fit. Create a new area if needed.

3. **Assign an ID** — next available `UF-###` or `SF-###` globally.

4. **Add to FLOWS.md** — under the correct functional area section.

5. **Mark as processed in FUNCTIONS.md** — update the item in-place:
   ```markdown
   - [x] user can register → UF-001
   - [x] on login failure, lock after 5 attempts → SF-003
   ```

Process the whole file in one pass, then confirm with the user before moving on.

### Handling additions at any time

The user can add new items to FUNCTIONS.md at any point — during detailing,
architecture, or even mid-prioritisation. When they do:

- Read FUNCTIONS.md and identify any unprocessed items (no `[x]`, no `→ ID`)
- Process them using the same steps above
- If a new item implies updating an already-detailed flow, note it and ask
  whether to update that flow's detail file or create a new flow

**The file is never "done"** — it accumulates throughout the project.

### Output

`specifications/FUNCTIONS.md` — all items marked as processed with IDs.

`specifications/flows/FLOWS.md` — grouped by functional area:

```markdown
# Flows

## User Management

### User Flows
- [ ] UF-001: User registers an account
- [ ] UF-002: User logs in
- [ ] UF-003: User resets their password

### Software Flows
- [ ] SF-001: On registration, send verification email
- [ ] SF-003: On 5 consecutive login failures, lock account

## Project Management

### User Flows
- [ ] UF-010: User creates a project
- [ ] UF-011: User invites a team member

### Software Flows
- [ ] SF-010: On project creation, initialise default settings
```

Use `UF-###` and `SF-###` IDs incrementing **globally** (not per area).

**Transition check:** Ask — *"Does this list look right? Anything missing,
mis-categorised, or in the wrong area?"*

---

## Phase 3 — Flow Detailing

**Goal:** Each flow fully specified so a developer (or agent) can implement it
without ambiguity.

Detail flows **one at a time**, confirming with the user before moving to the next.
Start with flows that have no dependencies (foundational flows first).

### Flow Detail Template

```markdown
## [ID]: [Name]

**Type:** User Flow | Software Flow
**Actor:** [Who or what triggers this]
**Trigger:** [The specific action or event that starts the flow]

**Preconditions:**
- [What must be true before this flow can run]

**Steps:**
1. [Step description]
2. [Step description]
...

**Success Outcome:** [What is true when the flow completes successfully]

**Error Outcomes:**
- [Error condition] → [What happens / what the user sees]

**Data Involved:**
- [Key data read or written, at concept level — no types or schema]

**Depends On:** [Flow IDs this flow requires to exist first, or "None"]
```

Save each detailed flow to `specifications/flows/[ID]-[slug].md`.

### Detailing tips

- Steps should describe **observable behaviours**, not implementation details.
  Good: "System sends a confirmation email to the user"
  Bad: "Call `sendEmail()` with SMTP config"
- Error outcomes must cover: validation failures, missing preconditions,
  external service failures where relevant
- "Data Involved" is conceptual — no code, no schema syntax
- If a flow has more than ~8 steps, ask the user if it should be split

**Transition check:** After all flows are detailed — *"Are all flows detailed enough to implement? Any gaps?"*

---

## Phase 4 — Architecture

**Goal:** A shared structural map of the system, synthesised from what the
detailed flows actually require — not speculated upfront.

This is the right moment for architecture because flow detailing often uncovers
needs that weren't obvious from the PRD alone: a background job system, a
notification service, a shared audit trail, a caching layer. Architecture done
here reflects reality rather than assumptions.

### What to capture

```markdown
## System Shape
[Monolith / services / serverless — how the system is deployed]

## Layers & Components
[Name each major component and what it owns]
- e.g. API layer — handles all inbound HTTP, auth, validation
- e.g. Domain layer — core business logic, no framework deps
- e.g. Persistence layer — DB access, migrations
- e.g. Job queue — async background processing
- e.g. Notification service — email / push / webhooks

## Flow → Component Mapping
[Which component(s) handle each flow type]
- User Flows → API layer → Domain layer → Persistence
- Software Flows → Job queue → Domain layer → Persistence

## Key Patterns
[Patterns all agents must follow — e.g. error handling, auth checks,
 transaction boundaries, event publishing]

## Out of Scope
[Components or concerns explicitly NOT being built now]
```

### How to build it

1. Read all detailed flow files (`specifications/flows/[ID]-[slug].md`)
2. Identify every distinct capability they collectively require
3. Group into components — name each and its responsibility
4. Map flow types to those components
5. Note patterns that emerge (e.g. every user flow needs auth validation;
   every software flow is async)
6. Flag anything not being built now as out of scope

If detailing revealed requirements not in the PRD, note them explicitly and
ask the user whether to update the PRD or treat them as implementation detail.

**Output:** `specifications/ARCHITECTURE.md`.

**Transition check:** *"Does this architecture cover everything the flows need?
Any components missing or over-specified?"*

---

## Phase 5 — Prioritisation

**Goal:** An ordered implementation sequence that respects dependencies and user value.

### Step 1: Build the dependency graph

For each flow, check its `Depends On` field. A flow cannot be implemented
before the flows it depends on. Software flows typically depend on the user
flow that triggers them.

### Step 2: Assign priority tiers

| Tier | Label | Meaning |
|------|-------|---------|
| P0 | Foundation | Required by many other flows; no standalone user value |
| P1 | Core | Primary value-delivering flows; build these first |
| P2 | Supporting | Enhances core flows; needed for completeness |
| P3 | Nice-to-have | Low dependency, lower urgency |

Ask the user to assign tiers, prompting with — *"Which flows deliver the most
value to the user and are needed to validate the core idea?"*

### Step 3: Produce the implementation order

Sort by: P0 → P1 → P2 → P3, and within each tier, respect dependency order.
If two flows have no dependency between them, note they can be parallelised.

**Output:** Update `specifications/flows/FLOWS.md` with tiers and the ordered table:

```markdown
## Implementation Order

| Order | ID | Name | Tier | Depends On | Parallel? |
|-------|----|------|------|------------|-----------|
| 1 | UF-001 | User creates a project | P1 | None | — |
| 2 | SF-001 | On project creation, init settings | P0 | UF-001 | — |
| 3 | UF-002 | User invites team member | P1 | UF-001 | Yes (with SF-001) |
```

**Transition check:** *"Does this order make sense? Any flows that should move earlier or later?"*

---

## Phase 6 — Task Creation

**Goal:** One Taskplane task per flow (or per cohort of tightly related flows).

Use the `create-taskplane-task` skill to create each task. Key mappings:

| Flow Field | Task Section |
|-----------|-------------|
| Flow name | Mission (what + why) |
| Trigger + Steps | Implementation steps |
| Success Outcome | Acceptance criteria in testing step |
| Error Outcomes | Edge cases in testing step |
| Depends On (flows) | Task dependencies |
| Data Involved | File Scope hints |

### Grouping flows into tasks

- A **User Flow + its directly triggered Software Flows** can be one task when
  they are always implemented together (e.g., "create project" + "initialise
  settings on creation").
- Keep tasks to **M size** (2-4 hours). If a flow is large enough to be XL,
  split the flow first (go back to Phase 3), then create tasks.

### TDD requirement in every task

Every FDD task must include in its PROMPT.md:

```markdown
## TDD Requirement

Implement this flow using Red → Green → Refactor:
1. Write a failing test that exercises the flow's success outcome
2. Write the minimum code to make the test pass
3. Refactor — clean up without breaking the test
4. Repeat for each error outcome

Tests must exercise the flow at the **boundary level** (API/service/handler
boundary), not against internal implementation details.
```

**Output:** One Taskplane task per flow (or cohort), ready to `/orch`.

---

## Phase 7 — TDD Implementation

**Goal:** Implement each flow via Red → Green → Refactor, in priority order.

This phase is executed by the Taskplane worker agents running the tasks created
in Phase 6. Your role here is to monitor, steer when needed, and confirm done.

### The TDD cycle per flow

```
1. RED      — Write a test describing the flow's success outcome. Run it. It must fail.
2. GREEN    — Write the minimum code to make the test pass. No extra structure.
3. REFACTOR — Improve quality without changing behaviour. Tests still pass.
4. REPEAT   — Add a test for the next outcome (error case, edge). Cycle again.
```

### Test scope by flow type

| Flow type | Test scope |
|-----------|-----------|
| User Flow | Integration/acceptance test at the API or UI boundary |
| Software Flow | Unit or integration test on the triggered handler or job |

### If a worker skips the flow-level test

If an agent writes unit tests for internal helpers before any flow-level test
exists, steer them:

> "Write the flow-level test first — one test that exercises the full
> [ID] success path end-to-end. Internal helper tests come after."

### Definition of Done (per flow)

- [ ] All success outcome tests pass
- [ ] All defined error outcome tests pass
- [ ] No pre-existing tests broken
- [ ] Flow detail doc updated if behaviour changed during implementation

---

## Artifacts Summary

| Phase | Artifact |
|-------|----------|
| 1 — PRD | `specifications/PRD.md` |
| 2 — Discovery | `specifications/FUNCTIONS.md` (brain-dump, stays open throughout) |
| 2 — Discovery | `specifications/flows/FLOWS.md` (grouped by functional area) |
| 3 — Detailing | `specifications/flows/[ID]-[slug].md` per flow |
| 4 — Architecture | `specifications/ARCHITECTURE.md` |
| 5 — Prioritisation | `specifications/flows/FLOWS.md` (updated with order table) |
| 6 — Tasks | Taskplane task folders per flow / cohort |
| 7 — TDD | Tests + implementation in project source |

---

## Common Mistakes to Avoid

| Mistake | Correction |
|---------|-----------|
| Skipping flow detailing and going straight to tasks | Tasks become vague and agents hallucinate scope. Always detail first. |
| Mixing user flows and software flows | They have different triggers and test strategies. Keep them separate. |
| Making software flows implementation-specific | "Call `initService()`" is too specific. Describe what happens, not how. |
| Creating one large task for many flows | Context overload and partial completions. One task per flow (or tight cohort). |
| Writing unit tests before any flow-level test | Implements the wrong abstraction first. Flow test always comes first. |
| Detailing flows out of dependency order | Later flows often clarify earlier ones. Start with foundations. |
| Doing architecture before detailing | Detailing reveals what the architecture actually needs. Don't speculate early. |
