# PR Review Checklist

A working checklist to mentally run over each substantive hunk in a diff.

## Correctness

- [ ] Does the new code do what the PR description says?
- [ ] Off-by-one errors in loops, slices, ranges?
- [ ] Null / undefined / empty-array cases handled?
- [ ] Are floating-point comparisons safe (no `===` on floats)?
- [ ] Is async correctly awaited? Any unhandled promises?
- [ ] Are there any `TODO` / `FIXME` comments left in the diff?

## Error handling

- [ ] What happens when the network call fails?
- [ ] What happens when the database returns zero rows?
- [ ] Are errors swallowed silently anywhere?
- [ ] Is sensitive data scrubbed before logging?

## Concurrency / race conditions

- [ ] Multiple requests / tabs / clicks — does it still behave?
- [ ] Are shared resources (files, sockets, DB rows) protected?
- [ ] Idempotency — can this safely retry?

## Security

- [ ] User input validated and escaped?
- [ ] Auth checked on the new endpoint(s)?
- [ ] Secrets in code or commit history?
- [ ] PII logged?

## Performance

- [ ] N+1 queries?
- [ ] Unbounded loops on user input?
- [ ] New blocking work on a hot path?

## Tests

- [ ] Is the new behavior covered by tests?
- [ ] Do the tests actually fail without the implementation?
- [ ] Are edge cases tested, not just the happy path?

## Reversibility

- [ ] If we have to roll this back tomorrow, can we?
- [ ] Any irreversible migrations / data writes?
