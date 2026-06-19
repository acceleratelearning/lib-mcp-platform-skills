---
name: review-pr
description: Read a pull request diff like a senior engineer. Flag risk, suggest tests, point out missing edge cases. Use when the user asks to "review PR", "look at this diff", or pastes a GitHub PR URL.
license: MIT
---

# Review PR

Read a diff the way a thoughtful senior engineer would. Optimize for **catching bugs and missing tests**, not for nitpicks.

## When to use

- User pastes a GitHub PR URL.
- User says "review this PR", "what do you think of this diff", "ship-check this".

## Steps

1. **Fetch the diff.** If a URL was given, run `gh pr diff <number>` (or `gh pr view <number> --json files,title,body`). If it's a local branch, `git diff main...HEAD`.

2. **Read the description first.** What is this PR trying to accomplish? Hold that intent in mind for the rest of the review.

3. **Walk the diff in this order:**
   1. New files / new entry points (highest risk).
   2. Modified business logic.
   3. Tests.
   4. Configuration, dependencies, infra.

4. **For each substantive change, check the checklist** in `references/checklist.md`.

5. **Write the review** in this shape:

   ```markdown
   ## Summary
   <one paragraph: what changed, what it's trying to do>

   ## Must-fix
   - [ ] <hard bugs, missing error handling, security issues>

   ## Should-fix
   - [ ] <missing tests, brittle code, unclear intent>

   ## Nits
   - [ ] <style, naming, suggestions — keep these short>

   ## Questions
   - <things you genuinely don't understand>
   ```

6. **Default posture: approve unless there's a Must-fix.** Senior reviewers unblock; they don't gatekeep.

## Don't

- Don't comment on formatting if the repo has a formatter — that's the linter's job.
- Don't suggest renames unless the existing name is genuinely misleading.
- Don't ask the author to "consider" something — either it's a Must-fix, a Should-fix, or it's not in the review.
