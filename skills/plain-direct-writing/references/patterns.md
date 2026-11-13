# Patterns to scan for

Scan a finished draft for these patterns and rewrite each match. A match prompts a check, not an automatic deletion. Keep the wording when it is literal, accurate, and needed.

## Words and phrases

- **AI vocabulary.** additionally, crucial, delve, enduring, enhance, foster, garner, interplay, intricate, landscape (abstract), pivotal, showcase, tapestry (abstract), testament, underscore, vibrant. Use the plain word or delete it.
- **Inflated words.** "utilize" and "leverage" become "use", "facilitate" becomes "help", "numerous" becomes "many".
- **Fancy ways to say "is" or "has".** "serves as", "stands as", "boasts", "features". Use "is" or "has".
- **Filler phrases.** "in order to" becomes "to", "due to the fact that" becomes "because", "in the event that" becomes "if". Delete "it is important to note that".
- **Metaphor nouns.** wedge, wedge in (add), vector (way, method), locus, vantage, nexus, harness as a metaphor, bedrock, scaffolding as a metaphor, modality, paradigm, gold-plating (more than the job needs), ratchet as a metaphor (a limit that only tightens), evacuate for moving code (move out), endgame (the last phase), north star, flywheel. Use the concrete word. Keep a word when it is the literal technical name, such as a test harness or a vector of numbers.
- **Trailing -ing phrases.** ", highlighting the need for...", ", ensuring reliability", ", reflecting a broader shift". Delete the phrase, or state the claim in its own sentence with its evidence.
- **Vague attributions.** "Experts believe", "industry reports suggest", "some critics argue". Name the source or delete the claim.
- **Adverbs that prop up weak verbs.** "runs quickly" and "significantly improves" need a stronger verb or the number: "is fast", "cuts p95 latency from 800 ms to 200 ms".

## Sentences

- **Dense sentences.** If the reader has to reread a sentence to parse it, split it or drop clauses. Keep one idea per sentence.
- **Passive voice.** "is/are/was/were + past participle" often hides the actor. "Queries are validated" becomes "The compiler validates queries." Keep the passive only when the actor is unknown or does not matter.
- **Stacked hedges.** "could potentially possibly be argued that it might" becomes "may". Hedge once, where the uncertainty is real. Keep any qualification the claim needs.
- **Forced groups of three.** Do not group items in threes for rhythm. Use the number of items that exist.
- **Synonym cycling.** Pick one term for each concept and repeat it. Do not alternate "the service", "the backend", and "the API" for the same thing.

## Chat replies

- **Chatbot phrases.** "I hope this helps!", "Let me know if you have questions", "Of course!", "Certainly!", "Found the smoking gun!". Delete them.
- **Sycophancy.** "Great question!", "You're absolutely right!". Respond to the content directly.

## Formatting

- **Bold overuse.** Bold only what the reader must not miss. Do not bold every proper noun, product name, or acronym.
- **Inline-header lists.** A bold label and colon that restates the line is a tell: "**Performance:** Performance improved by 20%." Rewrite it as a plain sentence. A bold lead-in that ends in a period, names the item, and is followed by new detail is fine: "**Schema in TypeScript.** Tables live in one file."
- **Title case headings.** Use sentence case: "Rate limits and retries", not "Rate Limits and Retries".
- **Decorative emoji.** Remove emoji from headings and bullets.
- **Em dashes.** Do not use em dashes. End the sentence, or use a comma, colon, or parentheses.
- **Curly quotes.** Use straight quotes (`"` and `'`), not curly quotes.
