---
name: grill-me
description: Interview the user relentlessly about a plan or design until every branch of the decision tree is resolved. Use when the user says "grill me", "stress-test this plan", "poke holes in this", or "what am I missing".
license: MIT
---

# Grill Me

Walk the user down their own decision tree until there's nothing left unresolved. The point is not to *agree* with the plan — it's to surface the assumptions they're making without realizing it.

## When to use

- The user shares a plan, RFC, design, or proposal and asks you to challenge it.
- The user says "grill me", "ask me hard questions", "what am I missing".
- Before a big commit / deploy / launch and the user wants a sanity check.

## Steps

1. **Read the plan once, without interrupting.** Hold it in your head before asking anything.

2. **Ask one question at a time.** Wait for the answer before asking the next. Multiple questions at once = the user picks the easy one and skips the hard one.

3. **Walk the decision tree depth-first.**
   - Pick the most load-bearing assumption.
   - Resolve every branch under it before moving to the next assumption.
   - Don't context-switch between unrelated decisions.

4. **For each question, also state your recommended answer.** Don't just interrogate — give the user something to push back on. A question without a hypothesis is a survey, not a grilling.

5. **If a question can be answered by reading the codebase, read the codebase.** Don't make the user look up something you can grep yourself.

6. **Track open threads.** If the user defers a decision, add it to a running "Open" list and circle back at the end.

## Question patterns that work

- "What happens if <the assumption you're depending on> is wrong?"
- "Who is the first person to notice this is broken? How long after it breaks?"
- "What's the dumbest version of this we could ship in a day? Why isn't that good enough?"
- "When you say <vague word> — do you mean A, B, or C?"
- "Walk me through the worst-case rollback. How bad is it?"

## Output format

```markdown
## Question 1
<the question>

**My guess:** <your recommended answer + 1-sentence rationale>

---
(after the user answers)

## Question 2
...

---

## Open at end of session
- ...
- ...
```

## Don't

- Don't ask five questions at once. One.
- Don't cheerlead. "Great plan!" is the opposite of the job.
- Don't ask questions you could answer yourself by reading the repo.
- Don't stop early because the plan "seems fine". The whole point is to find what *doesn't* seem fine.
