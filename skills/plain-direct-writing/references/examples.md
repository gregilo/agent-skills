# Before and after examples

The "after" versions preserve the useful meaning while removing rhetorical staging. Match their directness, not their exact wording.

## General prose

**Before**

> It is worth stating plainly that the onboarding experience is doing a lot of work here. The real opportunity is not merely to shorten the form—it is to rethink the journey.

**After**

> The onboarding flow causes many users to leave. Shorten the form and remove steps that are not required to create an account.

**Before**

> There is a deeper point to sit with: speed is not just a convenience. It is the foundation on which user trust is built.

**After**

> Faster responses increase user trust because users can tell whether the system received their actions.

## Technical explanations

**Before**

> The cache serves as a load-bearing boundary between the request path and the database. This is worth noting because invalidation is where the architecture starts to tell a different story.

**After**

> The cache reduces database reads. When a write succeeds, invalidate the related key so later requests do not return stale data.

**Before**

> Retries are a dial worth turning carefully. Too little resilience leaves performance on the table; too much creates a thundering herd.

**After**

> Retry transient failures up to three times with exponential backoff and jitter. More retries increase load during an outage.

## Rhetorical contrast

**Before**

> This is not a performance problem. It is a correctness problem.

**After**

> This is a correctness problem: two requests can update the same record and one update can be lost.

Use contrast when it corrects a likely misunderstanding and both sides matter:

> The timeout limits each request, not the entire batch. A batch can therefore run longer than 30 seconds.

## Metaphor as explanation

**Before**

> Treat the concurrency limit as a pressure valve that keeps the queue from boiling over.

**After**

> Limit concurrency to 20 workers so incoming jobs do not exhaust database connections.

**Before**

> This feature flag gives us a runway for landing the migration safely.

**After**

> The feature flag lets us enable the migration for a small group and disable it without a deployment.

## Abstract-agent prose

**Before**

> The evidence wants us to be cautious, and the architecture tells a different story than the benchmark does.

**After**

> The benchmark shows lower latency, but it does not include the cross-region requests used in production. Test those requests before changing the architecture.

**Before**

> This framing carries the argument, while the distinction between reads and writes does important work.

**After**

> The argument depends on treating reads and writes differently because only writes require conflict detection.

Literal inanimate subjects remain useful:

> The cache stores the result for five minutes.
>
> This parameter controls the timeout.

## Feelings instead of mechanisms

**Before**

> The query builder keeps SQL close at hand and gives you types that follow your schema.

**After**

> `.toSQL()` returns the exact SQL string the query builder sends to the database. Renaming a column fails the build until every query uses the new name.

**Before**

> The API is designed to be fast, reliable, and easy to use.

**After: state a fact about this project, or cut the sentence**

> The API serves cached reads in under 50 ms at p95 and retries failed writes up to three times.

## Over-compression

**Before**

> Parser rejects bad date → exit 2, no write.

**After**

> The parser rejects an invalid date, exits with code 2, and writes nothing.

**Before**

> Cfg missing → fall back to env, warn once.

**After**

> If the config file is missing, the loader reads settings from environment variables and logs one warning.

## Code comments

**Before**

```ts
// Important: This is the load-bearing validation step that ensures correctness.
if (!token) throw new AuthError();
```

**After: omit the comment**

```ts
if (!token) throw new AuthError();
```

The code already explains the check.

**Before**

```ts
// Normalize the input and then check whether it is empty.
const value = input.trim();
if (!value) return null;
```

**After: explain the non-obvious ordering constraint**

```ts
// Check before normalization because whitespace-only input must be preserved.
if (input === "") return null;
const value = input.trim();
```

## Commit messages

**Before**

> Unlock a more resilient future by reimagining retry behavior

**After**

> Add exponential backoff to webhook retries

**Before**

> Fix the critical validation flow that serves as the foundation of secure authentication

**After**

> Reject expired tokens before creating a session

A body may explain motivation when the subject alone is insufficient:

> Cap export jobs at four concurrent workers
>
> Higher concurrency exhausts the reporting database connection pool during peak traffic.
