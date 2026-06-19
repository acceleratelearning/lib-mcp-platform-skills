---
name: investigate-kubernetes-issue
description: Diagnose a Kubernetes workload or platform issue on EKS. Use when the user says "pod is crashing", "deployment is stuck", "investigate this k8s issue", "why is this pod failing", or pastes kubectl output or an ArgoCD sync error.
license: MIT
---

# Investigate Kubernetes Issue

Work from symptom to root cause on an EKS cluster. Gather state, isolate the failure domain, and produce a diagnosis with a concrete next action.

## When to use

- A pod, deployment, or job is failing, stuck, or not scheduling.
- ArgoCD reports a sync error or degraded app health.
- The user pastes `kubectl` output, logs, or an event stream.
- An EKS node or node group is unhealthy.
- A Helm release is stuck in a pending or failed state.

## Steps

1. **Capture the symptom precisely.** Record the namespace, workload name, and failure mode (CrashLoopBackOff, Pending, OOMKilled, ImagePullBackOff, etc.) before running any further commands.

2. **Describe the workload and recent events.**
   ```bash
   kubectl describe <kind>/<name> -n <namespace>
   ```
   Read the `Events` section at the bottom — it usually contains the proximate cause.

3. **Read current pod logs.** If the pod has restarted, also pull previous logs to capture the crash output.
   ```bash
   kubectl logs <pod> -n <namespace> --tail=100
   kubectl logs <pod> -n <namespace> --previous --tail=100
   ```

4. **Check resource pressure.** Confirm the pod's requests and limits, then verify node capacity and any LimitRange or ResourceQuota objects in the namespace.

5. **Narrow the failure domain.** Decide which layer the problem lives in:
   - **Scheduling** — node selectors, taints/tolerations, insufficient capacity.
   - **Image pull** — registry access, image tag, IRSA/pull secret.
   - **Init/startup** — init container failures, readiness/liveness probe misconfiguration.
   - **Runtime** — OOM, application crash, config mount error.
   - **Networking** — service/endpoint mismatch, NetworkPolicy block, DNS resolution.

6. **Check ArgoCD state if applicable.** If this is a GitOps-managed workload, review the ArgoCD Application sync status and health checks before proposing any manual changes.

7. **Check Helm release state if applicable.** Run `helm status <release> -n <namespace>` and `helm history <release> -n <namespace>` to confirm the deployed revision and any pending operations.

8. **Propose one next action.** Provide the single most targeted command or change most likely to resolve or further narrow the issue.

## Output format

```markdown
## Diagnosis
- Workload: <kind/name> in <namespace>
- Failure mode: <CrashLoopBackOff | Pending | OOMKilled | …>
- Root cause layer: <scheduling | image-pull | init | runtime | networking>

## Evidence
- Event: <most relevant event or log line>
- Prior crashes: yes / no

## Findings
- <specific cause + why it matters>

## Next action
- Command/change: <exact command or config fix>
- Expected outcome: <what should happen>

## Additional checks if still unresolved
1. <follow-up command>
2. <follow-up command>
```

### Gotchas
- Use the `--profile <account-name>` when using aws cli to select the correct AWS account. Don't assume the default account is the right one.  The kubernetes cluster name will be the same as the AWS profilename.
- Use the `--context <context-name>` flag when using kubectl to avoid accidentally running commands against the wrong cluster. The default context might not be the one you intend, especially if the user has multiple clusters or recently switched contexts.

## Don't

- Don't delete or restart pods as a first step — gather state first.
- Don't create Kubernetes resources directly. Resources should be created through ArgoCD or Helm to ensure they are tracked in Git and not overwritten by the next deployment.
- Don't make manual `kubectl apply` or `kubectl edit` changes on GitOps-managed workloads without noting the ArgoCD sync conflict.
- Don't report only `kubectl get pods` output without reading events and logs.
- Don't propose node-level fixes before ruling out application-level causes.
- Don't change the kubectl context since that might impact other sessions
