---
name: code-review-farm
description: Run multiple code review agents in parallel (Claude's /review-code and Codex's $review-code) and synthesize their findings into one unified review.
argument-hint: [branch-name or PR-url]
allowed-tools: Agent, Bash
disable-model-invocation: true
---

# Code Review Farm

Run two code reviewers in parallel, then hand both outputs to a synthesis subagent that produces a single unified report.

The reviewers:
1. **Claude** — subagent running the `/review-code` skill.
2. **Codex** — OpenAI Codex CLI running its `$review-code` skill (different model, different stance).

## Step 1: Kick off both reviewers in parallel

In a single response, issue the Agent and Bash tool calls together so they run concurrently. Pass the user's argument (branch name or PR/MR URL) to each. With no argument, both review the current branch against `main`.

### Claude reviewer (Agent tool)

`subagent_type: general-purpose`. Prompt:

```
Invoke the Skill tool with skill="review-code" and args="<argument>" (omit args if none).

Return the skill's Summary / Findings / Verdict output verbatim. No preamble, no commentary, no extra sections.
```

### Codex reviewer (Bash tool)

Use `timeout: 600000` — codex runs high-reasoning models that can take several minutes.

```bash
out=$(mktemp)
codex exec --skip-git-repo-check --color never --output-last-message "$out" - <<'PROMPT' >/dev/null 2>&1
$review-code <argument>
PROMPT
cat "$out"
rm -f "$out"
```

With no argument, send just `$review-code`.

## Step 2: Synthesize via subagent

Once both outputs arrive, spawn a third subagent (`subagent_type: general-purpose`) to merge them. This keeps the raw reviewer outputs out of the main context.

Pass both raw outputs in the synthesis prompt. The synthesis agent must not add findings of its own — its job is to merge what the reviewers produced.

Prompt template:

```
You are synthesizing two independent code reviews into one unified report. Do not add findings of your own. Merge what the reviewers produced.

=== CLAUDE REVIEW ===
<claude output>

=== CODEX REVIEW ===
<codex output>

Produce a single report in this exact format:

**Summary**

One or two paragraphs that fold both summaries together. Note where the reviewers agreed on intent and tradeoffs. If they disagreed on whether the approach is sound, surface that disagreement explicitly.

**Findings**

Group by severity, highest first: CRITICAL → HIGH → MEDIUM → LOW → SUGGESTION. Tag each finding with its source: `[claude]`, `[codex]`, or `[both]`.

Deduplication rules:
- Same issue at same severity from both → one `[both]` entry, using the clearer explanation and most actionable fix.
- Same issue at different severities → use the higher severity, tag `[both]`, note the gap in parentheses.
- Only one reviewer flagged it → keep it tagged with that single reviewer.

If neither found anything worth raising, write `No findings worth raising.`

**Verdict**

Take the more conservative verdict:
- 🔴 **Needs rework** — if either reviewer said rework.
- ⚠️ **Ship with minor fixes** — if either said minor fixes and neither said rework.
- ✅ **Ship it** — only if both said ship.

If the verdicts diverged, add a one-line note under the verdict explaining the divergence.

Return only the merged report. No preamble, no commentary.
```

Return the synthesis agent's output to the user verbatim.

## Failure handling

If one reviewer fails (timeout, error, empty output), skip the synthesis step. Surface the failure to the user and return the surviving reviewer's output directly with a one-line note about which reviewer dropped.
