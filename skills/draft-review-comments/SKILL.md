---
name: draft-review-comments
description: Draft polite, plain MR or PR review comments from code review findings, one comment per finding, and show them to the user for approval. Use after a review such as review-code, when the user asks for comments to leave on the MR or PR. This skill never posts anything.
argument-hint: [finding numbers, or empty for all findings]
---

# Draft review comments

Turn code review findings into comments the user can leave on a merge request (MR) or pull request (PR). The user decides what to post, where, and how. This skill only drafts.

## Never post without approval

- Do not post, reply to, resolve, approve, or edit anything on the MR or PR while drafting. Do not call `glab`, `gh`, or any GitLab or GitHub write API.
- Show every draft to the user and stop. Wait for the user's instruction.
- Post a comment only after the user explicitly approves that comment by number or with "all". Approving one comment does not approve the others.
- Post the exact text the user approved. If the text changes after approval, for example because the user asks for an edit, show the new text and wait for approval again.

## Inputs

- Use the findings already in the conversation, usually the Findings section from `review-code`. If the user names finding numbers, draft only those. Otherwise, draft one comment per finding.
- If the conversation has no findings, ask the user to paste them or to run `review-code` first. Do not run a review from this skill.

## Steps

1. Read [../plain-direct-writing/SKILL.md](../plain-direct-writing/SKILL.md) and the two reference files it links. Apply them to every comment body. The tone rules below are the user's requested tone, which plain-direct-writing says to preserve.
2. For each finding, reread the code at its `file:line` in the current revision. If the code no longer shows the problem, or the finding is wrong, do not draft a comment. List the finding under "Not drafted" with a one-sentence reason.
3. Classify each finding as blocking or non-blocking. CRITICAL and HIGH findings are blocking. MEDIUM, LOW, and SUGGESTION findings are non-blocking. If a finding has no severity, assign one with the severity definitions in `review-code`.
4. Write one comment per finding, following the comment rules below.
5. Scan each comment body against plain-direct-writing's `references/patterns.md` and fix every match.
6. Show the drafts in the output format below, then stop.

## Comment rules

- Start a non-blocking comment with `nit: `. Do not label blocking comments. Do not mention severity names such as HIGH or MEDIUM.
- Ask; do not instruct. Phrase the proposed change as a question, such as "Should we ...?", "Could we ...?", or "What do you think about ...?". Do not write imperatives such as "Change this", "Please fix", or "You need to".
- State the problem as a fact before the question: what the code does and the concrete consequence, such as the input that fails or the error it raises. Do not hedge the facts. Put the politeness in the question, not in doubt about the evidence.
- Use "we" for the proposed change and "this" for the code under the comment.
- Include the concrete fix. When the fix is a few lines, add it as a fenced code block with a language tag. Do not use platform `suggestion` blocks.
- Write each comment so it makes sense on its own. The author reads it next to one line of the diff, without the review or the other comments. Do not refer to the review, finding numbers, "the reviewer", or other comments.
- Do not repeat the location the comment is anchored to. Cite any other location as `path/to/file.ts:42`.
- Keep the body to one to four sentences, plus the optional code block.
- Do not add praise, apologies, greetings, sign-offs, or emoji.

## Output format

Start with the line `Draft comments (N). Nothing has been posted.`, where N is the number of drafts. Then show each draft as a numbered heading followed by the comment body, verbatim:

### 1. `path/to/file.ts:42` (blocking)

`user.first` is optional, so ...

Use `(non-blocking)` for non-blocking drafts. For a finding with no file and line, write `MR-level` in place of the location. After the drafts, add a "Not drafted" list if any finding was skipped. End there. Do not add a closing question or offer.

## Examples

Blocking finding: `user.first` is optional, and `user.first.trim()` throws a `TypeError` when it is undefined.

Avoid:

> You must handle undefined here. Fix this before merging.

Write:

> `user.first` is optional, so `user.first.trim()` throws a `TypeError` when a user has no first name. Should we default it to an empty string?
>
> ```ts
> const first = (user.first ?? "").trim();
> ```

Non-blocking finding: `formatAge(1)` returns "1 years".

Avoid:

> Nit: Pluralization is wrong. Please use the singular for 1.

Write:

> nit: `formatAge(1)` returns "1 years". Could we return "1 year" when `age` is 1?

Non-blocking finding where the fix is a design choice:

> nit: This retries every error, including 400 responses that fail the same way on every attempt. What do you think about retrying only 5xx responses and network errors?
