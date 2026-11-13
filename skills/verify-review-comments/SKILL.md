---
name: verify-review-comments
description: Fetch the review comments/discussion threads you left on a GitLab merge request (or GitHub pull request) and check, one by one, whether the coworker actually addressed each one in the current code — not just whether GitLab/GitHub marked the thread "resolved." Use this whenever the user asks to follow up on a code review they did, check if their comments were addressed, verify feedback was resolved, re-review an MR/PR after changes, or asks things like "did they fix what I flagged" or "go through my comments on !123." Complements (but is distinct from) `review-code` (initial review) — this skill runs *after* the author has pushed changes in response.
disable-model-invocation: true
---

# Verify Review Comments

Checks whether feedback left on a merge request (MR) or pull request (PR) has actually
been addressed in the code — as opposed to trusting the platform's "resolved" flag,
which the author can toggle without changing anything.

This skill is **read-only**: it never resolves threads, posts replies, or pushes
commits. If the user wants a reply posted (e.g. "tell them thread #3 isn't actually
fixed"), propose the comment and ask before posting via `glab mr note` or
`gh pr comment`.

## 1. Identify the MR/PR and the reviewer

Figure out, in order of preference:
1. An MR/PR number or URL the user gave you.
2. The MR/PR associated with the current branch (`glab mr view` or `gh pr view` with
   no args infers it from the checked-out branch).
3. If neither works, ask the user for the MR/PR number or link.

Identify *whose* comments to check — default to the current authenticated user
(`glab api user` → `.username`, or `gh api user` → `.login`) unless the user names
someone else ("check if Priya's comments were addressed").

## 2. Fetch every comment thread with its resolution state and position

Don't rely on a flat comment list — you need the *thread* structure (each thread's
resolved/unresolved flag, and the file/line it's anchored to), so use the
discussions/review-comments API, not just `mr view --comments`.

**GitLab (`glab`):**
```bash
glab api "projects/:id/merge_requests/<iid>/discussions" --paginate
```
Each discussion has `.notes[]` (the thread's comments, first note = the original
comment) and `.notes[0].resolvable` / `.notes[0].resolved`. Position info is under
`.notes[0].position` (`new_path`, `new_line` or `old_line`).

**GitHub (`gh`):**
```bash
gh api "repos/:owner/:repo/pulls/<number>/comments" --paginate
gh pr view <number> --json reviews  # thread-level resolved state needs GraphQL:
gh api graphql -f query='{ repository(owner:"...",name:"..."){ pullRequest(number: <number>){ reviewThreads(first:100){ nodes{ isResolved path line comments(first:20){ nodes{ author{login} body } } } } } } }'
```

Filter to threads whose first/original comment was authored by the target reviewer.
Drop threads that are just the author's own replies with no reviewer comment.

## 3. For each of the reviewer's threads, check the current code

For every thread (regardless of what the resolved flag says):

1. Note the file path and line the comment was anchored to, and the comment text.
2. Look at the **current** state of that file (not the diff at review time) —
   `git show HEAD:<path>` or just read the file at that path in the working tree if
   the branch is checked out. If the surrounding code has moved, search the file for
   the relevant symbol/logic rather than trusting the stale line number.
3. Read any replies in the thread (the author often explains what they changed, or
   pushes back — factor that in).
4. Judge whether the comment was substantively addressed: the concern was fixed,
   explicitly and reasonably declined with a stated reason, or is still outstanding.
   A thread marked "resolved" with no corresponding code change is **not** addressed —
   flag the mismatch explicitly.

## 4. Report

Always cite `file:line` for the current code you're pointing to — don't just restate
the comment. One row per thread:

| # | File:Line | Your comment (summary) | Platform status | Assessment | Evidence |
|---|-----------|------------------------|------------------|------------|----------|
| 1 | `src/foo.ts:42` | asked to null-check `user` | resolved | ✅ Addressed | `src/foo.ts:41` now guards with `if (!user) return` |
| 2 | `src/bar.ts:10` | flagged missing test | resolved | ⚠️ Resolved but not done | no new test found in `bar.test.ts` |
| 3 | `src/baz.ts:88` | suggested extracting helper | unresolved | 🕓 Still open | no change; author hasn't replied |

Close with a one-line summary: how many addressed / mismatched / still open. Lead
with the mismatches (⚠️) — those are the ones worth the user's attention, since the
clearly-addressed and clearly-still-open ones need no action.

If a thread's disposition is genuinely ambiguous (e.g. a partial fix, or a reply
you can't evaluate without more context), say so rather than guessing — ask the
user or point them at the thread rather than asserting a verdict either way.

## Notes

- If `glab`/`gh` isn't authenticated or the repo can't be resolved, say so and ask
  for a manual MR/PR URL rather than guessing at project IDs.
- Large MRs: paginate through all discussions before reporting — don't stop at the
  first page.
- This skill assesses code state, not test/CI results — if "addressed" hinges on
  a test passing, say that's what would confirm it rather than asserting pass/fail.