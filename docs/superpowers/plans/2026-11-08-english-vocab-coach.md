# English Vocabulary Coach Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build, install, validate, and automate a reusable IELTS vocabulary coach that writes dated local study records and daily summaries.

**Architecture:** Keep the version-controlled skill in `skills/english-vocab-coach/` and install an identical copy under `~/.codex/skills/english-vocab-coach/`. The skill owns lesson and raw-record conventions; a standalone Codex cron automation reads the previous day's records and idempotently maintains a monthly summary.

**Tech Stack:** Codex Skills Markdown/YAML, Codex desktop cron automation, Markdown learning records, Git.

---

### Task 1: Create the repository-owned skill

**Files:**
- Create: `skills/english-vocab-coach/SKILL.md`
- Create: `skills/english-vocab-coach/agents/openai.yaml`

- [x] Initialize the skill with `skill-creator/scripts/init_skill.py`.
- [x] Replace the scaffold with instructions covering UK/US IPA, English definitions, examples, collocations, usage constraints, word families, IELTS relevance, and unsupported-claim safeguards.
- [x] Add the multi-word comparison contract: nuance, register, interchangeability, collocation restrictions, learner errors, contrastive examples, and a decision guide.
- [x] Add the append-only record contract for `learning-records/YYYY-MM-DD/vocabulary.md`, including local timestamp, words, summary, distinctions, IELTS points, and review prompts.
- [x] Validate with:

```bash
python3 /Users/zhangyu/.codex/skills/.system/skill-creator/scripts/quick_validate.py /Users/zhangyu/workSpace/gpt/study_English/skills/english-vocab-coach
```

Expected: validation succeeds without frontmatter, naming, or placeholder errors.

- [x] Commit with `git commit -m "feat: add English vocabulary coach skill"`.

### Task 2: Install and smoke-test the skill

**Files:**
- Create: `~/.codex/skills/english-vocab-coach/SKILL.md`
- Create: `~/.codex/skills/english-vocab-coach/agents/openai.yaml`

- [x] Install the repository copy:

```bash
rsync -a --delete /Users/zhangyu/workSpace/gpt/study_English/skills/english-vocab-coach/ /Users/zhangyu/.codex/skills/english-vocab-coach/
```

- [x] Verify exact identity:

```bash
diff -ru /Users/zhangyu/workSpace/gpt/study_English/skills/english-vocab-coach /Users/zhangyu/.codex/skills/english-vocab-coach
```

Expected: no output and exit status 0.

- [ ] Exercise `Compare affect and effect for IELTS and save the lesson` in a temporary workspace. Confirm IPA, definitions, collocations, IELTS notes, contrastive examples, decision guide, and an appended dated record.

  Blocked on 2026-11-08: the locally installed `codex` CLI launcher fails with `ENOENT` because its native binary is missing. Retry after repairing or reinstalling the CLI, or verify manually in a newly opened Codex desktop chat.

### Task 3: Create the learning-document structure

**Files:**
- Create: `learning-records/.gitkeep`
- Create: `learning-docs/.gitkeep`
- Modify: `README.md`

- [x] Add tracked empty directories.
- [x] Document: `Vocabulary chat → learning-records/YYYY-MM-DD/vocabulary.md`.
- [x] Document: `Daily 00:00 automation → learning-docs/YYYY-MM.md`.
- [x] Commit with `git commit -m "docs: define learning record structure"`.

### Task 4: Create the daily summary automation

**Files:**
- External state: Codex desktop cron automation

- [x] List Codex projects and select the project rooted at `/Users/zhangyu/workSpace/gpt/study_English`.
- [x] Create an active local cron automation named `Summarize daily English learning`, recurring daily at 00:00.
- [x] Use this prompt:

```text
In /Users/zhangyu/workSpace/gpt/study_English, determine yesterday's calendar date in Asia/Shanghai and inspect learning-records/YYYY-MM-DD/. If there are no meaningful learning records, finish silently and do not create or modify a summary. Otherwise, summarize all records by learning type and idempotently create or replace that date's section in learning-docs/YYYY-MM.md. For vocabulary, include learned words, key distinctions, common collocations, IELTS takeaways, recurring mistakes, and a short spaced-review checklist. Preserve other dates and user-authored content, avoid duplicate date sections, then commit and push the changed learning document to origin main. If the push fails, leave the local commit intact and report the failure.
```

- [x] Set `notificationPolicy: failed_runs_only`.
- [x] Read the automation back and verify active status, daily midnight schedule, hub project, Asia/Shanghai date handling, empty-day skip, idempotency, and failure-only notifications.

### Task 5: Verify and publish

**Files:**
- Modify: `docs/superpowers/plans/2026-11-08-english-vocab-coach.md`

- [ ] Re-run the skill validator.
- [ ] Confirm `git status --short` contains no unexpected files.
- [ ] Mark completed plan checkboxes.
- [ ] Commit the completed plan with `git commit -m "docs: complete vocabulary coach implementation plan"`.
- [ ] Run `git push origin main`.
- [ ] Confirm local `main` and `origin/main` point to the same commit.

