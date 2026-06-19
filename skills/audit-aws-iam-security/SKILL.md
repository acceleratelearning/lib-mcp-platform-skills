---
name: audit-aws-iam-security
description: Audit AWS IAM and security posture for infrastructure changes, especially Pulumi-driven updates. Use when the user says "review IAM", "audit AWS security", "check least privilege", or "is this policy safe".
license: MIT
---

# Audit AWS IAM Security

Review IAM and security-sensitive infrastructure changes for least privilege, trust boundaries, and production risk.

## When to use

- The user asks for IAM policy review or AWS security audit.
- Pulumi changes include role, policy, trust policy, KMS, S3, or network-security updates.
- A deployment request needs security sign-off before merge or apply.

## Steps

1. **Collect scope.** Identify changed resources, target account(s), and environment.

2. **Review IAM policy deltas line by line.** Flag wildcard actions/resources, privilege escalation paths, and unmanaged broad grants.

3. **Review trust relationships.** Check principals, external IDs, federation assumptions, and cross-account trust boundaries.

4. **Check service-specific controls.** Verify encryption, public-access settings, key policy access, and logging requirements for affected services.

5. **Evaluate least-privilege fit.** Decide whether each permission is required by runtime behavior, build/deploy process, or neither.

6. **Classify findings by severity.** Use critical/high/medium/low and include explicit remediation per finding.

7. **Return deploy recommendation.** Approve, approve with required fixes, or block.

## Output format

```markdown
## Security decision
Approve | Approve with required fixes | Block

## Findings
- Severity: <critical/high/medium/low>
  - Resource/policy: <name>
  - Issue: <what is risky>
  - Remediation: <specific change>

## Least-privilege checklist
- Wildcards minimized: yes/no
- Trust boundaries validated: yes/no
- Encryption/logging controls validated: yes/no

## Required follow-up
1. <owner + action>
2. <owner + action>
```

## Don't

- Don't treat wildcard grants as acceptable without explicit justification.
- Don't review only identity policies; include trust policies and resource policies.
- Don't provide findings without remediation steps.
- Don't approve when critical issues remain unresolved.
