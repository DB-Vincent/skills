---
name: create-pr
description: Create a pull request. Use when a user asks to open or create a PR.
---

# Create PR

Before creating, verify if there is already a PR for this branch. Review the diff locally against the base branch (usuall `origin/main`) to make sure its contents match the goal.

Titles of PRs become commit messages, so follow the repository's title conventions. Look at recently merged PRs and Git history for a better view. Prefer a concise, human-readable title that explains why the change matters:

BAD

> perf(server): negotiate permessage-deflate on the websocket

GOOD

> perf(server): cut websocket frame size by 70%+ with gzipping

Open the description with a simple explanation of the problem based on the
user's original prompt, then briefly explain the solution. Do not lead with an
implementation inventory:

BAD

> Removed implicit workspace carry-over from every "new thread" entry point (cmd+n / cmd+shift+o, sidebar v1/v2 buttons, command palette). New threads inherit only the project from context; branch, worktree, and env mode always come from the configured defaults. Deleted buildContextualThreadOptions, startNewThreadInProjectFromContext, and the v1 sidebar's seed-context machinery.

GOOD

> My "new worktree" default was ignored when starting new threads on existing worktrees. Super unintuitive. Now your preferences always apply.

Before submission, check the title, description, quotes, and attachments for
secrets and private details.

Open a real PR rather than a draft so review bots run. If `gh pr create` fails
because the GraphQL rate limit is used up, open it through REST with
`gh api repos/<owner>/<repo>/pulls -f title=... -f head=... -f base=... -F body=@<file>`. If the user also asked
to babysit it, continue with the `babysit-pr` skill.

Do not post a follow-up PR comment unless it serves the user's request.
