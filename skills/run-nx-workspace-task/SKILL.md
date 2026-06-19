---
name: run-nx-workspace-task
description: Run and troubleshoot Nx targets in a pnpm TypeScript monorepo, including task graph context and cache behavior. Use when the user says "run nx", "execute this target", "why did nx fail", or "use pnpm nx".
license: MIT
---

# Run Nx Workspace Task

Execute Nx targets consistently and explain failures with enough context to unblock the next action.

## When to use

- The user asks to run a target in an Nx workspace.
- The user asks why an Nx task failed or was skipped.
- The user asks for affected/project-scoped task execution.

## Steps

1. **Confirm target scope.** Identify project, target, configuration, and whether the user wants one project or affected projects.

2. **Use pnpm to invoke Nx.** Prefer `pnpm nx ...` commands to match workspace tooling and lockfile expectations.

3. **Run the requested target with explicit flags.** Include configuration and verbosity when needed for debugging.

4. **Interpret cache behavior.** State whether output came from cache, local execution, or distributed execution.

5. **If failure occurs, isolate first failing task.** Capture the exact task name, root error, and the first actionable stack/log line.

6. **Propose the next command, not just diagnosis.** Provide the exact follow-up command to rerun or narrow the issue.

7. **Summarize completion status.** Report success/failure, produced artifacts, and any residual warnings.

## Output format

```markdown
## Nx run
- Command: <pnpm nx ...>
- Scope: <project/affected>
- Target: <target[:configuration]>

## Result
- Status: success | failed
- Cache: hit | miss | n/a
- First failing task: <task or none>

## Actionable next step
- Command: <exact follow-up command>
- Why: <one sentence>
```

## Don't

- Don't use ad-hoc scripts when Nx target definitions exist.
- Don't report only "task failed" without the first actionable error.
- Don't ignore cache context when explaining unexpected behavior.
- Don't switch package managers away from pnpm.
