---
name: draft-jira-ticket
description: Draft a Jira ticket's issue type, title, and description (in Jira wiki markup) from a Slack thread, the current conversation, an MR or PR, notes, or the user's description. Returns the draft only and does not create or edit issues in Jira. Use when the user asks to draft, write up, or turn something into a Jira ticket, issue, story, bug, spike, epic, or initiative.
---

# Draft Jira Ticket

Turn source material into a Jira ticket draft that the user can paste into Jira. Do not create, update, or comment on issues in Jira.

## 1. Read the source

Use the material the user points to: a Slack message and its thread, the current conversation, an MR or PR, notes, or their own description. If the user gives a link to a Slack thread, read the thread, including every reply. If you cannot read it, ask the user to paste it.

Base the ticket only on the source. Do not add requirements, problems, or scope that the source does not contain.

## 2. Classify the issue type

If the user names a type, use it. Otherwise pick one:

| Type | Use when |
|------|----------|
| Initiative | The source describes a broad, long-term goal or program of work that spans multiple epics or teams. |
| Epic | The source describes a large body of work that breaks down into multiple stories, such as a significant feature or deliverable. |
| Story | The source describes a discrete unit of user-facing or developer-facing work that delivers specific value and fits in one sprint. |
| Spike | The source is about research, investigation, prototyping, or exploration needed to answer open questions before committing to implementation. |
| Bug | The source describes unexpected or broken behavior in an existing system: something that used to work or does not match expected behavior. |

## 3. Write the title

- Story: "As a <persona>, I want <goal> so that <benefit>" when the source identifies the persona and benefit. Otherwise, a short title that states the deliverable.
- Bug: a short description of the broken behavior.
- Spike: "[Spike] <topic of investigation>".
- Epic or Initiative: a declarative title that summarizes the scope.

The title is plain text. Do not use wiki markup in it.

## 4. Write the description

Use these sections in this order. Start each with an `h3.` heading, for example `h3. Background Context`.

### Background Context
- What prompted the request or discussion.
- History, prior decisions, and dependencies mentioned in the source.
- Every link, document, MR, PR, or ticket referenced in the source.
- Who raised the issue and who else is involved. Name people as the source names them; for Slack, use their display name. Do not use `[~username]` mentions, because the source does not give Jira account IDs.

### Problem Summary
- What needs to happen and why.
- Bug: current behavior, expected behavior, and steps to reproduce if the source gives them.
- Spike: the questions to answer or the uncertainties to resolve.

### Acceptance Criteria

A numbered list (`#`) of specific, testable conditions that must hold for the issue to be done. Derive them from:
- explicit requirements or asks;
- agreed scope or constraints;
- edge cases or considerations raised;
- for spikes, the expected deliverable, such as a decision document or proof of concept.

If the source does not give enough information for a criterion, state what is missing and write a placeholder criterion that starts with `*\[NEEDS CONFIRMATION\]*`. Escape the brackets; unescaped `[...]` renders as a link.

### Open Questions

Include this section only when the source contains unresolved debate, disagreement, or unanswered questions. List each one and, for disagreements, who holds which position. Omit the section otherwise.

### Writing rules

- Leave out greetings, thanks, emoji reactions, and side conversation. Keep the substance.
- Keep the tone professional and concise so the description can go into Jira without edits.

## 5. Format with Jira wiki markup

Write the description in Jira wiki markup, not Markdown. Read [references/jira-wiki-markup.md](references/jira-wiki-markup.md) before writing, and check the draft against its "Common mistakes" section before returning it.

## Output

Return only this, with no preamble:

```
Issue type: <type>
Title: <title>
```

Then the description in a single fenced code block tagged `text`, so the chat does not render it and the user can copy it as is.
