---
name: change-review
description: Review Kubernetes YAML or intended mutations for production safety, blast radius, rollback, and verification.
metadata:
  short-description: Review Kubernetes changes before applying.
---

# Kubernetes Change Review

## Goal

Review Kubernetes YAML or intended mutations before production impact. Valid YAML is not enough; check whether the change will select the right resources, roll out safely, preserve security posture, and remain reversible.

## Workflow

1. Identify intent, resource kinds, namespace scope, ownership, and whether this is create, update, delete, or rollback.
2. Check cross-resource consistency: labels/selectors, Service ports/targetPorts, probes/container ports, NetworkPolicy selectors, RBAC subjects, volumes/PVCs, and config/secret references.
3. Estimate blast radius: selected pods, namespaces, users/service accounts, traffic paths, persistent data, and cluster-wide objects.
4. Review rollout and rollback: strategy, PDB, readiness/startup probes, minReadySeconds, image tag immutability, and previous revision availability.

## Red Flags

- Selector changes on Deployments, Services, NetworkPolicies, or PDBs without explicit migration plan.
- Service targetPort/name mismatch or selector that matches no pods.
- Liveness probe that checks external dependencies or is stricter than readiness.
- Missing requests on production workloads, risky limits, or memory limit lower than observed working set.
- Privileged pods, hostPath, hostNetwork, added capabilities, root user, broad ClusterRole/ClusterRoleBinding, or wildcard verbs/resources.
- Destructive deletes, finalizer changes, ownerReference changes, PVC/storageClass changes, or StatefulSet identity assumptions.
- Deprecated API versions, CRD schema changes, webhooks/admission changes, or GitOps sync behavior that can amplify the change.

## Output

Return: safe to apply or not, blocking concerns, non-blocking risks, evidence, recommended edits, pre-checks, rollout strategy, rollback plan, and confidence. Do not suggest applying, deleting, or force-replacing resources unless the user explicitly asks.
