---
name: evaluate-plan-feedback
description: Evaluate a reviewer agent's feedback on your implementation plan claim by claim, checking each for accuracy and legitimacy, then revise the plan where the feedback holds up. Use when the user passes along review feedback on a plan you drafted.
argument-hint: "[reviewer feedback]"
disable-model-invocation: true
---

# Evaluate plan feedback

A reviewer agent reviewed your implementation plan. Evaluate each claim in the
feedback for accuracy and legitimacy, then update the plan where needed. The
reviewer can be wrong, and so can your plan. Do not accept a claim because a
reviewer made it, and do not reject a claim because it contradicts what you
wrote.

## Find the feedback and the plan

The feedback is the text supplied with the invocation. If no text was
supplied, use the reviewer feedback in the user's most recent message. If
neither exists, ask the user to paste the feedback and stop.

The plan is the one you drafted in this conversation, or the plan file you
wrote. If you cannot find either, ask the user where the plan is and stop.

## Split the feedback into claims

Break the feedback into individual claims. A claim is one assertion about the
plan, the code, or the problem: a factual statement ("`fooService` has no
retry logic"), a gap ("the plan misses the migration for table X"), or a
recommendation ("use the existing `parseConfig` helper instead").

Keep the reviewer's numbering if it has one. When a numbered point contains
more than one claim, split it and number the parts with letters: 3a, 3b. If
the feedback is unnumbered, number the claims 1, 2, 3 in order.

## Evaluate each claim

For each claim, decide two things.

- **Accuracy.** Is the claim true? Check factual claims against the code,
  tests, docs, and history, not against the plan or the reviewer's summary of
  them. Read the cited files and symbols yourself. If you cannot reach the
  evidence, mark the claim unverified and say what would settle it.
- **Legitimacy.** If true, does it matter for this plan? A claim can be accurate
  but out of scope, already handled elsewhere in the plan, a style preference
  with no correctness impact, or a cost that outweighs its benefit. A claim is
  legitimate when acting on it makes the plan more correct, more complete, or
  simpler.

Assign one verdict per claim:

- **Valid**: accurate and legitimate. Revise the plan.
- **Partially valid**: part of the claim holds. Revise the plan for that part
  only, and state which part does not hold.
- **Invalid**: inaccurate, or accurate but not worth acting on. Leave the plan
  unchanged. Start the evidence with "False:" or "Not worth acting on:" so the
  two cases stay distinct.
- **Unverified**: the evidence is out of reach. Leave the plan unchanged unless
  the risk justifies a contingency, and name the missing evidence.
- **Needs user decision**: the claim raises a tradeoff or preference that the
  code cannot settle. Leave that part of the plan unchanged and ask the user,
  with a recommended option.

## Update the plan

Apply every valid and partially valid claim. When a change affects other
steps, update those steps too, so the plan stays consistent and executable
from top to bottom.

If the plan is in a file, edit the file in place. If the plan exists only in
this conversation, write out the full revised plan after the report, not just
the changed parts.

Do not add changes the feedback did not prompt. If you find a separate problem
in the plan while checking claims, list it after the table as an additional
finding instead of editing the plan for it.

## Report

Reply with one row per claim:

| # | Claim (summary) | Verdict | Evidence | Plan change |
|---|-----------------|---------|----------|-------------|

Keep cells short and do not put `|` characters in them. Cite `file:line` or
the source for evidence. If evidence needs more than a sentence, put it in a
numbered note below the table and reference the note in the cell. In "Plan
change", name the plan section you changed, or write "None".

After the table, list additional findings, then questions for the user. If
every claim is invalid and there are no additional findings, say the plan is
unchanged and why.

## Submit

Do not start implementing. The user approves the revised plan separately.

If any claim needs a user decision, end your turn after the report and
questions. Do not submit the plan for approval until the user answers. Then
revise the plan for their answers, report those changes, and submit.

Otherwise, if your environment has a plan approval step, submit the revised
plan through it after the report.
