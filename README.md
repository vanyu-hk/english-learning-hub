# English Learning Hub

Shared English-learning skills, study records, summaries, and workspace documentation.

## Child repositories

The following directories remain independent local Git repositories and are intentionally not tracked by this hub:

- `ielts-prep-site/`
- `personal-blog/`

## Hub structure

- `skills/` — reusable Codex skills maintained by this repository
- `learning-records/` — dated raw study records
- `learning-docs/` — monthly consolidated learning documents
- `docs/superpowers/` — designs and implementation plans

## Learning data flow

```text
Vocabulary chat → learning-records/YYYY-MM-DD/vocabulary.md
Daily 00:00 automation → learning-docs/YYYY-MM.md
```

The daily automation uses the `Asia/Shanghai` calendar, summarizes the previous day, and skips days without meaningful records.

