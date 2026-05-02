---
name: write-rfc
description: Draft a one-page technical RFC for a proposed change — context, options, recommendation, risks, rollout. Use when the user says "write an RFC", "design doc", "tech spec", or "I'm proposing X".
license: MIT
---

# Write RFC

Turn a fuzzy proposal into a one-page RFC people can actually review. Optimize for **getting a decision**, not for completeness.

## When to use

- The user says "write an RFC", "draft a design doc", "I want to propose X".
- A change is big enough that it deserves a thumbs-up from someone other than the author.

## Steps

1. **Get the question first.** Push the user to state what specifically they need a decision on. "Should we X?" beats "I'm thinking about X."

2. **Sketch in this order** — and stop when each section is one paragraph:
   - **Context** — why does this question exist now?
   - **Options** — at least two, including "do nothing".
   - **Recommendation** — the author's pick and why.
   - **Risks** — the two or three things most likely to go wrong.
   - **Rollout** — how would we ship this safely?

3. **Cap it at one page.** If you're writing more, you're hiding the decision.

## Output format

```markdown
# RFC: <one-line proposal>

**Status:** Draft · **Author:** ... · **Decision needed by:** ...

## Context
<one paragraph — why is this question on the table now?>

## Options
1. **Do nothing.** ...
2. **Option A.** ...
3. **Option B.** ...

## Recommendation
<which option, in one paragraph, with the trade-off>

## Risks
- ...
- ...

## Rollout
<how to ship — feature flag, migration order, owners>
```

## Don't

- Don't bury the recommendation. State it in the first line of the section.
- Don't list five options. If you have five, you haven't done the thinking yet.
- Don't write the implementation. RFC is about the decision; PR is about the code.
