---
name: open-pr
description: Open a pull request with the GitHub CLI. Use when the user asks to open, create, or ship a PR — even casually ("PR this", "ship it"). Verifies the gh auth account matches the repo's git config email before pushing, so multiple git profiles don't cross wires.
---

# Open Pull Request

Open a PR from the current branch using `gh`, with the right identity and the right template.

## Steps

### 1. Verify identity

Compare the repo's git identity with the active `gh` account:

1. `git config user.email` — the identity commits are authored with.
2. `gh auth status` — lists authenticated accounts and marks the active one.
3. `gh api user --jq '.email // .login'` — the active account's identity.

The check passes when the active gh account belongs to the same identity as the git config email (same email, or a login you can tie to it — e.g. the email's user/domain clearly maps to that account). If you cannot tie them together, or the active account is clearly a different profile, STOP and ask the user which gh account to use, then `gh auth switch --user <login>`.

**Done when:** the active gh account is confirmed to match the repo's git email, or the user has explicitly chosen an account.

### 2. Gather the change

1. Ensure the branch is pushed (`git push -u origin HEAD` if needed).
2. Read every commit on the branch since it diverged from the base (`git log <base>..HEAD`) and the full diff (`git diff <base>...HEAD`) — the PR body is written from this evidence, not from memory of the session.

**Done when:** every commit and the diff have been read.

### 3. Pick the template

1. Look for a repo template: `.github/PULL_REQUEST_TEMPLATE.md`, `.github/pull_request_template.md`, `PULL_REQUEST_TEMPLATE.md`, or `.github/PULL_REQUEST_TEMPLATE/` (if the directory has several, ask the user which one).
2. If the repo has one, follow it strictly — keep every section and heading, fill each from the diff evidence.
3. If the repo has none, use [TEMPLATE.md](TEMPLATE.md) from this skill folder.

**Done when:** a template is chosen and every one of its sections is filled (or explicitly marked N/A).

### 4. Confirm base and title

1. If the user didn't name the base branch in their prompt, ask them what it should be.
2. If the user didn't provide a title, propose three title suggestions based on the diff and let them pick (or write their own).

**Done when:** the user has confirmed both a base branch and a title.

### 5. Open the PR

Create it with `gh pr create` against the confirmed base branch, with the confirmed title. Report the PR URL.

**Done when:** the PR URL is shown to the user.
