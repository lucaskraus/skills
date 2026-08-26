# skills

My personal collection of [agent skills](https://code.claude.com/docs/en/skills) — portable instructions for AI coding tools, kept in one repo so they are easy to sync across machines and try with new tools.

## Structure

Each skill lives in its own folder with a `SKILL.md` inside (plus any supporting files the skill needs):

```
skills/
├── skill-name/
│   ├── SKILL.md
```

## Installing a skill

Install a skill from this repo with the [skills](https://skills.sh) CLI:

```sh
npx skills add lucaskraus/skills
```

It fetches the repo directly from GitHub (no clone needed) and lets you pick which skills to install.
