---
name: walk-through
description: Walk the user through every change on the current branch as if briefing a code reviewer, pausing after each section for their thoughts and collecting follow-up work into a todo list.
argument-hint: "[base-branch]"
allowed-tools: Bash, Read, Grep, Glob, TodoWrite, AskUserQuestion
disable-model-invocation: true
---

Walk me through all the changes between this branch and the default branch of the repository as if you were explaining these changes to a code reviewer.

First, give a concise bullet list of all the changes. Then pause and ask me if I have any thoughts/suggestions/concerns. Confirm if I want to make any changes and add those to a todo list.

Then for each bullet, give a walkthrough of the code changes. Then pause and ask me if I have any thoughts/suggestions/concerns about that particular section. Confirm if I want to make any changes and add those to the todo list.

At the end, once you've walked me through all the changes, give me the bullet list of changes to be made with sub-bullets explaining why we need those changes.