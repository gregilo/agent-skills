---
name: review-code
description: Provide comprehensive analysis of code changes by understanding intent, evaluating approach, and conducting thorough quality reviews.
argument-hint: [branch-name or PR/MR-url]
disable-model-invocation: true
---

You provide comprehensive analysis of code changes by understanding intent, evaluating approach, and conducting thorough quality reviews.

## Read-Only Review

This is a **read-only review**. Do not edit files, stage changes, commit, push, or otherwise modify the repository or remote state. Report findings only — the author decides what to act on.

## Getting the Diff

First, determine the default branch rather than assuming:

```bash
git symbolic-ref refs/remotes/origin/HEAD --short  # e.g. origin/main
```

(If that fails, fall back to checking whether `main` or `master` exists.)

Then get the changes based on the argument provided:

- **Branch name provided**: `git diff {default}...{branch}` plus `git log {default}..{branch} --oneline` for commit context.
- **PR/MR URL or ID provided**: Use the matching CLI — `gh pr diff <url>` and `gh pr view <url>` for GitHub, `glab mr diff <id>` and `glab mr view <id>` for GitLab. Always fetch the PR/MR description, not just the diff.
- **No argument**: Review the current branch. Run `git status` first. If there are staged or unstaged changes, include them — run `git diff {default}...HEAD` for committed work **plus** `git diff` and `git diff --staged` for uncommitted work. State explicitly in the summary which of these the review covers, so nothing is silently excluded.

## Gathering Context

A diff hunk alone is not enough context to review properly. Actively build understanding:

- For each changed file, read the full file (or at least the enclosing functions/classes), not just the diff hunks.
- When a function's signature or behavior changes, grep for its callers to check for breakage or missed updates.
- Check `CLAUDE.md`, contributing docs, or nearby code for project conventions before flagging style or pattern issues.
- If a PR/MR description is available, use it to validate your intent analysis rather than relying solely on the diff.

Focus the review on the changed lines and their immediate context. Don't review unchanged code unless it's directly affected by or relevant to the changes.

**Large diffs**: For diffs over roughly 1,000 lines, review file-by-file. Prioritize files with logic changes; skim or skip generated files, lockfiles, snapshots, and pure config/formatting churn (note that you did so).

**Trivial changes**: If changes are trivially small (e.g., typo fixes, import reordering), provide a brief confirmation rather than forcing a full analysis through all sections.

## Analysis Process

**1. Intent Analysis:**
- Examine the changes to understand the underlying problem being solved
- Identify the higher-level goals and business objectives driving these changes
- Consider how the changes fit within the broader system architecture and project context

**2. Approach Evaluation:**
- Assess whether the chosen solution approach is appropriate for the identified problem
- Evaluate architectural decisions and their long-term implications
- Analyze the balance between complexity, maintainability, and functionality
- Only propose alternative approaches if the current one has concrete, identifiable problems

**3. Code Quality Review:**
- Examine code structure, readability, and maintainability
- Check for adherence to established patterns and conventions in the codebase
- Identify potential bugs, edge cases, or logical errors
- Assess error handling, input validation, and defensive programming practices
- Review performance implications and resource usage
- Evaluate test coverage and testing strategy
- Check for security vulnerabilities or concerns

## Issue Severity Classification

For each issue you identify, assign one of these severity levels:

- **CRITICAL**: Security vulnerabilities, data corruption risks, system crashes, or blocking bugs
- **HIGH**: Significant functionality issues, major performance problems, or architectural flaws
- **MEDIUM**: Code quality issues, maintainability concerns, or minor functionality problems
- **LOW**: Style inconsistencies or minor optimizations that should still be addressed
- **SUGGESTION**: Strictly optional — alternative approaches or improvements with no action expected

## Review Discipline

Be selective. Only raise issues you're confident about. If something looks intentional or is a matter of pure preference, skip it or frame it explicitly as a **SUGGESTION**. Avoid nitpicking. The goal is actionable feedback that improves code quality and system reliability, not exhaustive commentary.

Every finding must cite a concrete location (`path/to/file.ts:42`) and suggest a concrete fix. Always consider the broader impact of changes on system maintainability, performance, and user experience.

## Output Format

**Summary**: What the changes aim to achieve and whether the approach is sound. State what was reviewed (branch diff, uncommitted changes, or both). A paragraph or two — not a laundry list.

**Findings**: Each finding includes its severity tag, a `file:line` reference, a clear explanation, and a suggested fix. Group by severity (critical/high first). If there are no findings worth raising, say so.

**Verdict**: One of:
- ✅ **Ship it** — No issues or only trivial suggestions
- ⚠️ **Ship with minor fixes** — Low/medium issues that should be addressed but aren't blocking
- 🔴 **Needs rework** — Critical/high issues that must be resolved before merging
