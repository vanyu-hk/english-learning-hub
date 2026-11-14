# Vocabulary in Use Study Coach Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Upgrade `english-vocab-coach` so it can teach, resume, and review units from the two local *English Vocabulary in Use* books while retaining direct word explanations and durable study records.

**Architecture:** Keep routing and shared teaching constraints in the concise `SKILL.md`; place project-specific book mappings, PDF lookup rules, mode workflows, and the append-only progress schema in one reference file. Document learner usage in `README.md` and enforce workspace behavior in a root `AGENTS.md`, then synchronize the validated source skill to the installed Codex skill directory.

**Tech Stack:** Markdown Agent Skill, local PDF sources, shell validation with `rg`, `diff`, and the bundled `quick_validate.py`.

---

## File map

- Modify `skills/english-vocab-coach/SKILL.md`: skill trigger, routing, shared teaching rules, and reference-loading rules.
- Create `skills/english-vocab-coach/references/books-and-workflows.md`: canonical PDF mapping, source lookup rules, four learning workflows, and progress record schema.
- Modify `README.md`: learner-facing usage and examples.
- Create `AGENTS.md`: workspace-wide source fidelity, skill usage, and record preservation rules.
- Synchronize the three skill files to `/Users/zhangyu/.codex/skills/english-vocab-coach/`: installed copy used by Codex.

### Task 1: Define behavioral acceptance checks

**Files:**
- Test: command-line checks against `skills/english-vocab-coach/`, `README.md`, and `AGENTS.md`

- [ ] **Step 1: Run the pre-change structure check**

Run:

```bash
test -f skills/english-vocab-coach/references/books-and-workflows.md
```

Expected: FAIL because the reference does not exist.

- [ ] **Step 2: Run the pre-change mode check**

Run:

```bash
rg -n "Unit study|Continue learning|Review mode|Ad hoc vocabulary" skills/english-vocab-coach/SKILL.md
```

Expected: FAIL because the current skill only handles direct vocabulary teaching.

### Task 2: Upgrade the skill and add the book workflow reference

**Files:**
- Modify: `skills/english-vocab-coach/SKILL.md`
- Create: `skills/english-vocab-coach/references/books-and-workflows.md`

- [ ] **Step 1: Replace the skill entrypoint**

Write a concise entrypoint that:

```markdown
---
name: english-vocab-coach
description: Use when the user wants to study, continue, or review either local English Vocabulary in Use book, or learn, explain, compare, distinguish, or review English vocabulary.
---

# English Vocabulary Coach

Route requests among unit study, continuation, review, and ad hoc vocabulary. For any book-based request, read `references/books-and-workflows.md` before answering. Preserve direct word teaching, reliable word-formation analysis, active recall, source fidelity, and append-only dated learning records.
```

Expand this with the exact routing signals, shared response rules, and record requirements from the approved design while keeping book-specific detail in the reference.

- [ ] **Step 2: Add the reference file**

Create `references/books-and-workflows.md` with these exact sections and content requirements:

```markdown
# Books and Workflows

## Canonical books
- Book 1 -> `vocabulary/1 English vocabulary in use elementary 3th.pdf`
- Book 2 -> `vocabulary/2 English Vocabulary in Use Pre-Intermediate Intermediate 4th.pdf`

## Source lookup
Resolve paths from the active workspace, locate the unit heading, inspect only necessary pages, verify examples, try a second read-only PDF method after extraction failure, and never guess missing content.

## Unit study
Locate -> group adaptively -> teach one group -> active production -> mixed quiz -> save next position.

## Continue learning
Read newest applicable record -> reconcile later reviews -> recall briefly -> resume exact next group.

## Review mode
Test incorrect/uncertain/incomplete items first -> hint before answer -> explain errors -> update mastery.

## Ad hoc vocabulary
Prioritize the active unit sense when applicable; otherwise answer independently.

## Progress entry schema
Timestamp, mode, book/unit/title, studied items, summary/collocations, learner answers/errors/uncertain items, status, next position, review prompts, suggested interval.
```

Include ambiguity handling, no-record continuation behavior, British/American usage, adaptive batch sizing, and the prohibition on claiming a reminder was scheduled.

- [ ] **Step 3: Validate mode and source coverage**

Run:

```bash
rg -n "Unit study|Continue learning|Review mode|Ad hoc vocabulary" skills/english-vocab-coach/SKILL.md skills/english-vocab-coach/references/books-and-workflows.md
rg -nF "vocabulary/1 English vocabulary in use elementary 3th.pdf" skills/english-vocab-coach/references/books-and-workflows.md
rg -nF "vocabulary/2 English Vocabulary in Use Pre-Intermediate Intermediate 4th.pdf" skills/english-vocab-coach/references/books-and-workflows.md
```

Expected: all commands find the named modes and both exact book paths.

- [ ] **Step 4: Commit the skill source**

```bash
git add skills/english-vocab-coach/SKILL.md skills/english-vocab-coach/references/books-and-workflows.md
git commit -m "feat: teach Vocabulary in Use units"
```

### Task 3: Document learner and agent usage

**Files:**
- Modify: `README.md`
- Create: `AGENTS.md`

- [ ] **Step 1: Add learner-facing README guidance**

Add a `Vocabulary in Use coach` section containing:

```markdown
## Vocabulary in Use coach

The `english-vocab-coach` supports both books in `vocabulary/`.

- `开始学习第 2 册 Unit 1`
- `继续上次学习`
- `复习第 2 册 Unit 1`
- `revise 是什么意思？`

The coach reads the relevant PDF unit, teaches in manageable groups, quizzes with active recall, and appends progress to `learning-records/YYYY-MM-DD/vocabulary.md`.
```

Also document that “continue” uses the latest record and that reminders are separate, explicit automation requests.

- [ ] **Step 2: Create root agent instructions**

Create `AGENTS.md` with:

```markdown
# English Learning Hub Instructions

## Vocabulary study

- Load `english-vocab-coach` for either local Vocabulary in Use book or vocabulary teaching/review.
- Resolve “Book 1” and “Book 2” only to the exact files documented by the skill.
- For book questions, inspect the smallest relevant PDF scope and verify the unit heading and examples.
- Never invent textbook wording or overwrite prior learning records.
- Append each completed lesson or review to the local-date vocabulary record.
- Use the newest applicable record to continue an unfinished unit.
```

Include preservation of unrelated files and a pointer to `skills/english-vocab-coach/references/books-and-workflows.md` for detailed behavior.

- [ ] **Step 3: Verify documentation examples**

Run:

```bash
rg -n "开始学习第 2 册 Unit 1|继续上次学习|复习第 2 册 Unit 1|revise" README.md
rg -n "smallest relevant PDF|Never invent|newest applicable record" AGENTS.md
```

Expected: all example and invariant phrases are present.

- [ ] **Step 4: Commit the documentation**

```bash
git add README.md AGENTS.md
git commit -m "docs: explain Vocabulary in Use coach"
```

### Task 4: Validate and install the skill

**Files:**
- Modify: `/Users/zhangyu/.codex/skills/english-vocab-coach/SKILL.md`
- Create: `/Users/zhangyu/.codex/skills/english-vocab-coach/references/books-and-workflows.md`
- Preserve: `/Users/zhangyu/.codex/skills/english-vocab-coach/agents/openai.yaml`

- [ ] **Step 1: Validate the source skill**

Run:

```bash
python3 /Users/zhangyu/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/english-vocab-coach
```

Expected: validator reports a valid skill.

- [ ] **Step 2: Verify canonical source files exist**

Run:

```bash
test -f "vocabulary/1 English vocabulary in use elementary 3th.pdf"
test -f "vocabulary/2 English Vocabulary in Use Pre-Intermediate Intermediate 4th.pdf"
```

Expected: both commands pass.

- [ ] **Step 3: Synchronize only the maintained skill content**

Run:

```bash
mkdir -p /Users/zhangyu/.codex/skills/english-vocab-coach/references
cp skills/english-vocab-coach/SKILL.md /Users/zhangyu/.codex/skills/english-vocab-coach/SKILL.md
cp skills/english-vocab-coach/references/books-and-workflows.md /Users/zhangyu/.codex/skills/english-vocab-coach/references/books-and-workflows.md
```

Expected: source and installed files match while the existing `agents/openai.yaml` remains untouched.

- [ ] **Step 4: Verify installed parity and final diff quality**

Run:

```bash
diff -u skills/english-vocab-coach/SKILL.md /Users/zhangyu/.codex/skills/english-vocab-coach/SKILL.md
diff -u skills/english-vocab-coach/references/books-and-workflows.md /Users/zhangyu/.codex/skills/english-vocab-coach/references/books-and-workflows.md
git diff --check HEAD~2..HEAD
git status --short
```

Expected: both diffs are empty; diff check passes; only pre-existing untracked user content under `learning-records/2026-11-08/` and `vocabulary/` remains.

### Task 5: Run representative behavior review

**Files:**
- Review: `skills/english-vocab-coach/SKILL.md`
- Review: `skills/english-vocab-coach/references/books-and-workflows.md`
- Review: `README.md`
- Review: `AGENTS.md`

- [ ] **Step 1: Check four request paths against the written instructions**

Review these prompts without mutating learning records:

```text
开始学习第 2 册 Unit 1
继续上次学习
复习第 2 册 Unit 1
revise 是什么意思？
```

Expected: each prompt maps unambiguously to one mode; the first three load the reference; direct lookup retains full vocabulary teaching; all completed sessions append records.

- [ ] **Step 2: Check failure behavior**

Review these conditions:

```text
Unknown unit number
Missing progress record for “continue”
PDF extraction failure
Learning-record write failure
```

Expected: the skill asks one concise clarification only when needed, does not guess source content, offers a starting point when no record exists, retries a second read-only PDF method, and returns unsaved Markdown after a write failure.

- [ ] **Step 3: Report completion with evidence**

Report the validator output, exact files changed, installed parity checks, and the fact that unrelated untracked files were preserved.
