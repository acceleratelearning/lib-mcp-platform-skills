---
name: deploy-pulumi-stack
description: Deploy a Pulumi stack safely in a TypeScript monorepo that uses pnpm and Nx. Use when the user says "deploy this stack", "run pulumi up", "apply infra changes", or asks for a production-safe rollout plan.
license: MIT
---

# Deploy Pulumi Stack

Deploy infrastructure changes with a repeatable safety flow: validate, preview, deploy, verify, and capture rollback notes.

## When to use

- The user asks to deploy infrastructure changes with Pulumi.
- The user says "run pulumi up", "apply this stack", or "ship this infra change".
- A stack promotion is needed (for example dev to staging or staging to prod).

## Steps

1. **Confirm stack and environment.** Record the Pulumi stack name, AWS account, and region before running any deployment command.

2. **Install and sync dependencies** with pnpm at repo root.
   - Run `pnpm install --frozen-lockfile`.
   - If the workspace uses Nx targets for infra, run the target through Nx instead of ad-hoc scripts.

3. **Run pre-deploy checks through Nx.** Execute the workspace checks used by this repo (lint, typecheck, tests, policy checks) before preview.

4. **Run a preview first.** Execute `pulumi preview` (or the Nx target that wraps it) and inspect creates, updates, replaces, and deletes.

5. **Gate risky operations.** If preview contains replacements, deletes, IAM broadening, or networking changes, pause and summarize risk before deploy.

6. **Deploy with `pulumi up`.** Run the deploy command for the confirmed stack. Do not skip confirmation unless the user explicitly asks for non-interactive CI behavior.

7. **Verify outputs and health checks.** Capture key stack outputs, confirm expected AWS resources exist, and note any post-deploy smoke checks.

8. **Document rollback path.** Provide the exact commands and conditions for rollback (for example `pulumi cancel`, revert commit, then redeploy previous state).

## Output format

```markdown
## Deployment plan
- Stack: <stack>
- AWS account/region: <account>/<region>
- Command path: <nx target or pulumi cli>

## Preview summary
- Create: <count>
- Update: <count>
- Replace: <count>
- Delete: <count>
- Risk notes: <iam/networking/deletes>

## Deployment result
- Status: success | blocked | failed
- Key outputs:
  - <name>: <value>

## Rollback notes
- Trigger conditions: <when to rollback>
- Commands:
  1. <command>
  2. <command>
```

## Don't

- Don't run `pulumi up` before previewing.
- Don't deploy without confirming stack/account/region.
- Don't bypass Nx-based checks when the repo defines them.
- Don't hide risky replaces/deletes in a long log dump.
