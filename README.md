# skills

My personal collection of [agent skills](https://code.claude.com/docs/en/skills) — portable instructions for AI coding tools, kept in one repo so they are easy to sync across machines and try with new tools.

## Structure

Each skill lives in its own folder with a `SKILL.md` inside (plus any supporting files the skill needs):

```
skills/
├── open-pr/
│   ├── SKILL.md
│   └── TEMPLATE.md
└── address-pr-comment/
    └── SKILL.md
```

## Skills

- **[open-pr](open-pr/SKILL.md)** — open a pull request with the GitHub CLI, checking that the gh account matches the repo's git identity, and following the repo's PR template (or a simple fallback).
- **[address-pr-comment](address-pr-comment/SKILL.md)** — fetch a PR review comment, validate the reviewer's point against the codebase, fix it when it holds, and draft a reply.
