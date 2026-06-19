---
name: review-pulumi-preview
description: Review a Pulumi preview or diff for breaking risk, security impact, and rollout safety in AWS. Use when the user says "review this preview", "is this pulumi change safe", "what will this modify", or pastes preview output.
license: MIT
---

# Review Pulumi Preview

Turn Pulumi preview output into a decision: safe to proceed, blocked pending fixes, or needs staged rollout.

## When to use

- The user provides `pulumi preview` output and asks for review.
- The user asks "is this safe to deploy" for infrastructure changes.
- A change includes IAM, networking, data stores, or Kubernetes platform resources.

## Steps

1. **Read the whole preview once.** Capture resource operations by type: create, update, replace, delete, and unchanged.

2. **Identify blast radius.** Note whether the change touches stateful resources, externally reachable endpoints, IAM policy scope, or shared network boundaries.

3. **Classify risky operations.** Flag all replacements and deletes, then explain service impact and likely downtime behavior.

4. **Check IAM deltas explicitly.** Highlight new wildcards, expanded principals, cross-account trust, and managed policy attachments.

5. **Check EKS and Kubernetes impact.** If preview touches cluster/node/security-group/IRSA paths, call out pod restart, permission drift, and rollout ordering risks.

6. **Require validation evidence.** Ask for or suggest concrete checks: policy simulation, staging deploy, smoke tests, and alarms to watch.

7. **Return a go/no-go recommendation.** Decide one of: proceed, proceed with guardrails, or block.

## Output format

```markdown
## Decision
Proceed | Proceed with guardrails | Block

## Change summary
- Create: <count>
- Update: <count>
- Replace: <count>
- Delete: <count>

## High-risk findings
- <resource>: <risk and why it matters>

## Required guardrails
- <staging check or test>
- <monitoring/alarm requirement>
- <rollback prerequisite>

## Reviewer notes
- <assumptions and missing context>
```

## Don't

- Don't approve by default when replacements/deletes exist.
- Don't ignore IAM diffs because they are "just policy text".
- Don't skip rollout/rollback guidance for EKS-related changes.
- Don't provide only a log summary without a recommendation.
