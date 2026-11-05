---
name: git-commit
description: Use when the user wants to make a Git commit or split changes into multiple commits.
allowed-tools: shell(git:*)
---

# Git commit

Examine the diff, stage the related files, write a Conventional Commits message, and run `git commit`. Not for push, pull requests, rebase, or history changes, and not when the user only wants a commit message.

## Message format

```text
<type>(<scope>): <summary>

<body>

<footer>
```

- Scope, body, and footer are optional; without a scope write `<type>: <summary>`.
- Summary in the imperative mood: "add", not "added" or "adds".
- First line 72 characters or less, no trailing period.
- Body: what changed and why, concisely.
- Footer: `Token: value` or `Token #value` (e.g. `Closes #123`); Git only reads `Token: value` as a trailer, and tokens use `-` instead of spaces.

## Types

If the repository has its own list, use it. Otherwise: `feat` new feature, `fix` bug fix, `docs` documentation, `style` formatting only, `refactor` code change that is not a feature or fix, `perf` performance, `test` tests, `build` build system or dependencies, `ci` CI config, `chore` other maintenance, `revert` revert of an earlier commit.

## Breaking changes

Put `!` before the colon (`feat(api)!: remove the v1 endpoints`) or add a `BREAKING CHANGE:` footer; the token is uppercase, and `BREAKING-CHANGE:` is also correct.

## Workflow

### 1. Read the repository rules

Check the commitlint config (`commitlint.config.js`, `.commitlintrc.json`), `CONTRIBUTING.md`, and `git log --oneline -10`. A repository rule overrides this skill.

### 2. Examine the changes

```bash
git status --porcelain
git diff --staged
git diff
```

If there are no changes, tell the user; do not make an empty commit.

### 3. Stage the files

One logical change per commit. If the staged files contain other changes, unstage them first (`git reset -q -- <paths>`), then stage this change:

```bash
git add -- '*.test.*'
git add -- src/components/
```

- Quote globs; unquoted globs are expanded by the shell and miss files in subdirectories.
- No `git add -p` or `git add -i`; they wait for interactive input.
- After resolving a merge conflict, use `git add --resolved`; it stages only resolved files and stops if conflict markers remain.
- Check `git diff --staged --name-only` before committing; never commit secrets (`.env`, `credentials.json`, private keys).

### 4. Write the message

Derive the type, scope, and summary from the diff.

### 5. Commit

Give each paragraph its own `-m`; Git inserts a blank line between them:

```bash
git commit -m "feat(auth): add token refresh" -m "An empty file caused a crash." -m "Closes: #123"
```

For a long message, write it to a temporary file and run `git commit -F <file>`.

### 6. Verify

```bash
git log -1 --stat
git status --porcelain
```

## Safety rules

- Do not change the Git configuration.
- Do not use `--no-verify` unless the user asks.
- If a hook fails, fix the problem, stage the files again, and rerun the same `git commit`; do not use `--amend`.
- Do not change the commit history unless the user asks; no `git reset --hard`, `git rebase`, or `git history`.
- Do not push unless the user asks; never force push to the default branch.
