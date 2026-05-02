---
name: ship-changelog
description: Turn a week of git commits into a customer-ready release note. Use when the user asks for "changelog", "release notes", or "what shipped this week".
license: MIT
---

# Ship Changelog

Convert raw commit history into a release note people will actually read.

## When to use

- User says "write a changelog", "release notes", "what shipped".
- The end of a sprint, the day of a release, the start of a marketing email.

## Steps

1. **Pick a window.** Default to "since last Monday" unless the user gives one.
   ```bash
   git log --since='last monday' --pretty='%h %s' --no-merges
   ```

2. **Group commits by area.** Look at the file paths each commit touched and bucket into 3-6 themes (e.g. _Editor_, _Billing_, _API_, _Performance_, _Bug fixes_).

3. **Rewrite each commit as a user-visible bullet.** Lead with the change, not the PR number. Drop refactors, dependency bumps, and internal renames.

   - ❌ `refactor(billing): extract InvoiceService`
   - ✅ — _omit_

   - ❌ `fix: null check on customer.email`
   - ✅ `Email confirmations no longer fail when a customer has no recovery email.`

4. **Add a "Highlights" section** with the 1-3 most impactful items, each one sentence.

5. **Output as Markdown** ready to paste into a Linear release, GitHub Releases, or a Slack post.

## Output format

```markdown
## Highlights
- The single biggest thing that shipped this week.
- The second biggest thing.

## Editor
- Bullet, bullet, bullet.

## Billing
- Bullet, bullet.

## Bug fixes
- Bullet, bullet, bullet.
```

## Don't

- Don't include commit hashes unless the user asks.
- Don't use marketing speak ("revolutionary", "blazingly fast"). Read it back to yourself first.
