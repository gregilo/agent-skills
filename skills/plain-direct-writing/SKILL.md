---
name: plain-direct-writing
description: Draft or revise prose, technical explanations, markdown docs, code comments, and commit messages to replace verbose, performative, or formulaic AI-style writing with plain, direct language. Use when writing should be concise and literal without changing its meaning or required tone.
disable-model-invocation: true
---

# Plain Direct Writing

Make the meaning easy to recover. Do not try to make ordinary points sound insightful.

## Rules

- Lead with the answer, conclusion, change, or recommendation. Omit setup that does not help the reader understand it.
- Use plain, declarative sentences. Prefer ordinary words, concrete nouns, active verbs, and `is`, `has`, or `does` when accurate.
- Prefer literal descriptions to metaphors, slogans, aphorisms, dramatic fragments, or memorable labels.
- State claims directly. Do not announce that a point is important, obvious, honest, subtle, or worth considering.
- Avoid editorial framing such as "the real question," "the key insight," "the right move," or "here is where I would push back." Give the question, insight, recommendation, or disagreement instead.
- Use rhetorical contrast, including "not X, but Y," only when both sides convey information needed by the reader.
- Name the actual actor or mechanism. Do not make an abstraction appear to think, want, speak, carry an argument, or do unspecified "work." Inanimate subjects are fine when they literally act: "The cache stores the result."
- Say what something does, not how it feels. Replace impressions such as "keeps the database close at hand" or "types that follow your schema" with the mechanism, a number, or an instruction: "`.toSQL()` returns the exact SQL string sent to the database." If a sentence cannot be restated as a concrete instruction, fact, or number, cut it. If it could appear unchanged in another project's docs, it says nothing about this one, so cut it.
- Explain a point once at the level of detail the task requires. Do not restate it as a summary slogan.
- Preserve necessary nuance, domain terminology, evidence, qualifications, and the user's requested tone. Concision must not remove meaning.
- Do not over-compress. Write whole sentences with their articles and verbs. Spell out arrows, symbols, and abbreviations the reader would have to decode. "Parser rejects bad date → exit 2, no write" becomes "The parser rejects an invalid date, exits with code 2, and writes nothing."
- Do not use "load-bearing," "worth stating plainly," "worth noting," "sit with," or similar rhetorical filler.

For technical writing, favor precision over punchiness. Introduce a term or label only if it improves later discussion.

For code comments, explain a non-obvious constraint, reason, or behavior. Do not paraphrase the code or declare a line important.

For commit messages, state the concrete change. Add motivation only when it will help a future reader understand the change.

Before delivering prose, remove throat-clearing, manufactured emphasis, unnecessary contrasts, decorative metaphors, abstract-agent phrasing, and repeated conclusions. Then scan the draft against [references/patterns.md](references/patterns.md) and fix each match.

Read [references/examples.md](references/examples.md) when drafting or revising prose, technical explanations, code comments, or commit messages, or when a rule above is unclear.
