---
name: fill-mr-template
description: Fill out the repository's GitLab merge request template using the current branch's diff and commit history, returned as raw Markdown ready to paste. Use when the user asks for an MR or PR description.
allowed-tools: Bash, Read, Glob, Grep
---

Find the default GitLab Merge Request template in this repository. It is typically located at `.gitlab/merge_request_templates/Default.md`, but may also be found at `.gitlab/merge_request_templates/default.md` or similar paths within `.gitlab/`. Read the template file contents. If you can't determine this on your own, ask for a path to the default merge request template.

Then, analyze the current branch's changes by examining:

1. The diff between this branch and the target branch (usually `main` or `master`)
2. The commit history on this branch
3. Any relevant context from changed files

Using what you've learned, fill out the merge request template with substantive, useful content. Follow these rules:

- **Preserve the template structure exactly.** Keep all original headings, sections, and formatting. Do not remove, rename, or reorder sections. If a section is not applicable, write "N/A" rather than deleting it.
- **Be concise.** Write short, direct descriptions. Avoid filler phrases and unnecessary preamble. Bullet points over paragraphs where appropriate.
- **Write for a first-time reviewer.** Assume the reader has no context on this work. Highlight the _why_ behind changes, not just the _what_. Call out:
  - Key architectural decisions or trade-offs made
  - Areas of the diff that deserve the closest review attention
  - Any non-obvious side effects or risks
  - Files or sections that are mechanical/generated and can be skimmed
- **Be honest about uncertainty.** If you cannot confidently fill a section from the available context, say so briefly rather than guessing.
- **Check any checkboxes** in the template that are satisfied by the branch's changes. Leave unchecked boxes unchecked.

Respond with the filled-out template in **raw, unrendered Markdown** — i.e. output the literal Markdown source so it can be pasted directly into a GitLab MR description field.
