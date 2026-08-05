---
name: address-pr-comment
description: Address a PR review comment. Use when the user shares a GitHub PR comment link and wants it evaluated, answered, or fixed. Verifies the reviewer's point against the diff and codebase before touching anything.
---

# Address PR Comment

Given a PR comment link, fetch it, judge the reviewer's point against the real code, fix it only when the point holds, and draft a reply.

All `gh` commands in this skill run under the account that matches the repo's git identity — follow the **Verify identity** step in [open-pr/SKILL.md](../open-pr/SKILL.md) first. If no gh account matches the git config email, ask the user which account to use.

## Steps

### 1. Fetch the comment

Fetch the comment via `gh api` from the link (review comments live at `repos/{owner}/{repo}/pulls/comments/{id}`; issue comments at `repos/{owner}/{repo}/issues/comments/{id}`). Capture its body, the file and line it anchors to, and the surrounding review thread if there is one.

**Done when:** the comment body and its anchor (file, line, thread) are in hand.

### 2. Investigate

Validate the reviewer's point with real evidence, not plausibility:

1. Read the PR diff around the comment's anchor.
2. Explore the codebase context — the file itself, callers, and existing patterns the comment touches.

**Done when:** you can state, with file references, whether the codebase supports or contradicts the reviewer's point.

### 3. Verdict

Classify the comment:

- **Wrong** — the evidence contradicts the reviewer's point. Do not change code; explain why in the reply.
- **Valid** — the point holds. Apply the fix now.
- **Worth a decision** — the point may hold but is the user's call, not yours. It lands here when any of these is true:
  - it introduces a pattern new to the codebase;
  - the fix would ripple across too many files for a review-comment fix — judge this by the size of the change, not a fixed count;
  - it explicitly asks for behavior not found in the codebase.

  Share this verdict only with the user in the session — it never appears in the reply. Present the trade-off and let the user decide before touching code.

**Done when:** one verdict is stated with the evidence behind it, and for Valid, the fix is applied and working.

### 4. Check the change

When the fix touched code, run the repo's own lint, prettier/format, and test commands — only the ones that exist (look in `package.json` scripts, Makefile, or the repo's docs). If a test fails because of your changes, fix it and carry that into the summary.

**Done when:** every existing lint/format/test command passes, or the failure is confirmed pre-existing (present without your changes).

### 5. Wrap up

Everything stays local: leave the fix uncommitted and the reply unposted — the user triggers both. Never add yourself as co-author on any commit.

1. Summarize what changed to address the comment (or why nothing changed).
2. Draft a suggested reply: objective and short — a couple of sentences, not an essay. Natural human language, no slang, markdown syntax when mentioning files (`` `path/to/file.ts` ``). For a Wrong verdict, a respectful explanation grounded in the code; for Valid, what was changed.
3. Ask the user whether to post the reply and whether to commit and push the fix. Only do either after they say yes.

**Done when:** summary and suggested reply are shown, and the user has answered on posting/committing.
