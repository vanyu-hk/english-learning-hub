# Vocabulary in Use Study Coach Design

## Goal

Extend the existing `english-vocab-coach` skill into one unified coach for ad hoc vocabulary questions and structured study of the two *English Vocabulary in Use* PDF books stored in `vocabulary/`.

The coach should support first-time unit study, resuming an unfinished unit, targeted review, and individual word questions while keeping durable progress records.

## Source books

The workspace contains two canonical sources:

- `vocabulary/1 English vocabulary in use elementary 3th.pdf` - Book 1, Elementary
- `vocabulary/2 English Vocabulary in Use Pre-Intermediate Intermediate 4th.pdf` - Book 2, Pre-intermediate and Intermediate

When a request names a book number and unit, the coach resolves it to this mapping. It reads only the pages needed for the requested unit, confirms the heading and examples from the source, and does not invent or silently substitute book content. If the unit cannot be located reliably, it explains the problem instead of guessing.

## Learning modes

The skill selects the mode from the user's request and recent records.

### Unit study

For requests such as “开始学习第 2 册 Unit 1,” the coach:

1. Locates and reads the unit in the corresponding PDF.
2. States the lesson goal and divides the material into manageable semantic groups.
3. Teaches one group at a time using concise definitions, pronunciation when useful, collocations, grammar patterns, contrasts, and examples grounded in the book.
4. Prompts the learner to retrieve meanings or make a sentence rather than only rereading explanations.
5. Ends the session with a short mixed quiz and records unfinished material for continuation.

The number of words per group is adaptive: smaller for difficult or easily confused items and larger for familiar material.

### Continue learning

For “继续上次学习,” the coach reads the latest relevant progress entry, identifies the exact book, unit, and next group, briefly recalls the last material, and resumes without reteaching mastered content unless the learner asks.

### Review

For requests to review a date, book, or unit, the coach prioritizes words previously marked incorrect, uncertain, or incomplete. It uses active recall first, gives hints before answers when appropriate, explains errors, and records the new mastery result. It may recommend a next review interval, but it does not claim to schedule a reminder unless the user explicitly requests an automation.

### Ad hoc vocabulary

For direct questions such as “revise 是什么意思,” the coach preserves the existing detailed vocabulary behavior. If the word comes from the active unit, it also explains the local textbook meaning and contrast; otherwise it answers independently without forcing a unit workflow.

## Teaching principles

- Use clear English definitions with concise Chinese clarification for subtle distinctions.
- Prefer active recall, learner-generated examples, and contrastive questions over passive repetition.
- Correct the learner's interpretation directly but encouragingly.
- Distinguish British and American usage when relevant to these British-English-oriented books.
- Do not overload a response with every possible dictionary sense; prioritize the textbook sense, then add only useful common senses.
- Preserve the existing reliable word-formation and IELTS guidance when relevant, but do not force IELTS commentary into every elementary unit item.

## Records and state

Continue appending to `learning-records/YYYY-MM-DD/vocabulary.md` using the learner's local date and time. Each completed session records:

- timestamp and mode;
- book, unit, and unit title when applicable;
- words or groups studied;
- concise learning summary and important collocations;
- learner answers, errors, and uncertain items;
- progress status: `in-progress`, `completed`, or `reviewed`;
- the next starting point for unfinished units;
- short review prompts and a suggested next review interval.

Records are append-only. Repeating a lesson creates a new review entry rather than rewriting history. “Continue” uses the newest applicable entry as state and reconciles it with later review entries when present.

## Skill structure

- `skills/english-vocab-coach/SKILL.md` contains triggering, mode routing, shared teaching rules, and record requirements.
- `skills/english-vocab-coach/references/books-and-workflows.md` contains the two-file mapping, PDF unit lookup rules, mode-specific workflows, and the progress-entry schema.
- `README.md` gives learner-facing invocation examples and explains where records are stored.
- Root `AGENTS.md` gives project-specific instructions for agents: recognize the two books, load the skill for relevant requests, inspect the smallest necessary PDF scope, preserve source fidelity, and append progress records.

The existing skill name remains unchanged so current invocations and installed-skill expectations continue to work.

## Error handling

- If a PDF reader or extraction method fails, try another available read-only PDF method; report failure rather than fabricate content.
- If book or unit wording is ambiguous, use current learning context when it identifies exactly one source; otherwise ask one concise clarification.
- If no progress record exists for “continue,” say so and offer to begin a specified book and unit.
- If a record write fails, provide the prepared Markdown entry and state that it was not saved.
- Never overwrite prior learning records or unrelated user files.

## Validation

- Validate the skill's frontmatter and structure with the bundled skill validator.
- Check that all book paths named by the skill and documentation exist.
- Run representative behavior checks for unit study, continuation, review, and ad hoc lookup.
- Verify that README and AGENTS.md examples match the final skill behavior.
- Confirm unrelated untracked user files remain untouched.

## Scope

This change does not add audio generation, OCR of image-only pages, a graphical dashboard, automatic reminders, or automatic completion of entire units in one response. These can be added later when explicitly requested.
