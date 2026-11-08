# English Vocabulary Coach Design

## Goal

Create a reusable Codex skill for IELTS-oriented English vocabulary study and a daily automation that consolidates the previous day's local learning records.

## Skill location and triggering

Maintain the skill source at `skills/english-vocab-coach` in the `english-learning-hub` repository. Install it at `~/.codex/skills/english-vocab-coach` so it is available across chats while remaining version-controlled by the hub. It should trigger when the user supplies one or more English words and asks to learn, explain, compare, distinguish, or review them, especially in an IELTS context.

## Vocabulary response

For one word, provide:

- UK and US IPA where they differ
- part of speech and concise English definitions
- natural example sentences
- common collocations and usage constraints
- word family, common synonyms, and antonyms when useful
- reliable prefix, root or base, and suffix analysis, with related words and a memory cue when useful
- IELTS topics, task relevance, register, common traps, and high-value paraphrases

For multiple words, also compare meaning, nuance, register, collocation restrictions, interchangeability, and typical learner errors. End with a compact decision guide and contrastive examples.

Distinguish practical modern word formation from historical etymology. Do not force unreliable decompositions or infer meaning mechanically from an affix. Do not invent unsupported etymology, frequency claims, or IELTS scoring guarantees. Flag regional or context-dependent usage where relevant.

## Local learning records

After each vocabulary-learning response, append a durable Markdown record beneath the active project directory:

`learning-records/YYYY-MM-DD/vocabulary.md`

Each entry records the local timestamp, input words, concise learning summary, distinctions, IELTS points, and review prompts. Create missing directories and preserve existing entries. If filesystem writes are unavailable, return the prepared Markdown and clearly state that it was not saved.

## Daily summary automation

Run every day at 00:00 in `Asia/Shanghai`. The automation reads the previous calendar day's files under `learning-records/YYYY-MM-DD/`. If no meaningful record exists, it exits silently without creating an empty summary.

When records exist, append or idempotently update that day's section in:

`learning-docs/YYYY-MM.md`

The summary groups material by learning type. For vocabulary it includes learned words, key distinctions, collocations, IELTS takeaways, recurring mistakes, and a short spaced-review checklist. Re-running the task must not duplicate the same date section.

## Error handling and validation

- Resolve all record paths relative to `/Users/zhangyu/workSpace/gpt/study_English` for this automation.
- Preserve user-authored content and unrelated files.
- Treat absent directories and empty files as “no records.”
- Validate the skill with the bundled skill validator.
- Verify the automation schedule, timezone, and prompt after creation.

## Scope

This version covers vocabulary learning and multi-word comparison. Pronunciation audio generation, quizzes, dashboards, and automatic external dictionary lookup are outside the initial scope.
