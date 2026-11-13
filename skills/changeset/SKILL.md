---
name: changeset
description: Write a changeset message for the current code changes, suitable for a `@changesets/cli` markdown file. Use when the user asks for a changeset, a changelog entry, or a release note describing their changes.
allowed-tools: Bash, Read, Glob, Grep
disable-model-invocation: true
---

Analyze the current code changes and produce a changeset message suitable for use with `@changesets/cli`.

## Determining the change scope

First, determine which changes to analyze using this strategy:

1. Check for unstaged file modifications (`git diff`). If there are meaningful changes (not just whitespace), use those.
2. If there are no unstaged changes, check for staged changes (`git diff --cached`). If present, use those.
3. If there are no staged or unstaged changes, identify the repo's default/primary branch (typically `main` or `master`) and diff the current branch against it (`git diff <default-branch>...HEAD`).

## Analyzing the changes

Before writing anything, carefully study the diff output:

- Read every changed file. Understand what was added, removed, and modified.
- Identify the **user-facing effect** of the changes. Ask yourself: "If I were a consumer of this package, what changed for me?" Think in terms of new capabilities, changed behavior, fixed bugs, removed functionality, or updated APIs.
- Distinguish between changes that affect the public surface (exports, APIs, behavior, configuration) and purely internal changes (refactors, test updates, dev tooling, CI). Only public-facing changes need changeset messages.
- If the changes span multiple distinct user-facing concerns, each one should be its own bullet point or sentence — but they belong in a single changeset message unless they affect different packages.

## Writing the changeset message

Write the message as it would appear in a changeset markdown file (the content below the YAML front matter). Follow these rules:

- **Write for the end user, not the developer.** The audience is someone reading a changelog to understand what changed in a new release. They don't care about file names, internal refactors, or implementation details.
- **Use past tense** (e.g., "Added support for...", "Fixed an issue where...", "Removed deprecated...").
- **Be specific about what changed**, but frame it in terms of behavior or capability, not code structure. Good: "Added a `timeout` option to `runAction` for controlling request deadlines." Bad: "Added timeout parameter to the fetch wrapper in src/http/client.ts."
- **Keep it concise.** One to three sentences is typical. If the change is simple, one sentence is fine. If multiple user-facing things changed, use a short bulleted list.
- **Don't editorialize.** No "This is a great improvement" or "Users will love this." Just state what changed.
- **Don't reference internal tickets, MR numbers, or commit SHAs** in the message body.
- **If the changes are entirely internal** (refactors, test-only changes, CI config, dev dependencies) and have no user-facing impact, say so explicitly: state that no changeset is needed and explain why.

## Output format

Provide **only** the changeset message content — the text that goes below the `---` front matter separator in a changeset markdown file. Do not include the YAML front matter itself (the package name and bump type). Do not wrap it in a code block. Do not include commentary about your analysis process.

If no changeset is needed, say so clearly and briefly explain why (e.g., "These changes are internal refactoring with no user-facing impact. No changeset is needed.").