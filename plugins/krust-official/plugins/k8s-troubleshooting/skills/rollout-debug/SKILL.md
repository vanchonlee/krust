---
name: rollout-debug
description: Debug Kubernetes Deployment, StatefulSet, DaemonSet, Job, or CronJob rollout issues. Use when a rollout is stuck, unavailable, or newly failing.
metadata:
  short-description: Debug stuck or failing rollouts.
---

# Rollout Debug

## Goal

Explain why a rollout is not progressing, unavailable, or newly failing, then produce the lowest-risk recovery path.

## Workflow

1. Identify controller kind, namespace, generation, observed generation, desired/current/ready/available replicas, conditions, and latest events.
2. Compare current pod template with previous revisions when available: image, command/args, env, config/secret references, probes, resources, labels, selectors, and strategy.
3. Inspect new ReplicaSet/pods first, then old ReplicaSet/pods to separate rollout failure from pre-existing health issues.
4. Treat repeated pod failures as a template/controller issue until evidence proves a node-specific or pod-specific issue.

## Decision Tree

- If `observedGeneration` lags, the controller has not processed the latest spec; inspect controller/events and avoid assuming pod failure.
- If desired replicas are not created, inspect quota, admission/webhook rejection, selector/template mismatch, and controller events.
- If pods are created but unavailable, reuse pod triage: scheduling, image pull, init, crash, OOM, readiness, or probe failure.
- If new pods are ready but rollout remains blocked, inspect maxUnavailable/maxSurge, PodDisruptionBudget, minReadySeconds, readiness gates, and old replica termination.
- If a StatefulSet is involved, consider ordered startup, PVC binding, stable identity, and one-bad-ordinal blocking later pods.
- If a DaemonSet is involved, compare node selectors, taints, tolerations, OS/arch, and per-node failures.
- If a Job/CronJob is involved, inspect completions, parallelism, backoffLimit, activeDeadlineSeconds, failed pods, and schedule/suspend status.

## Output

Return rollout status, blocking condition, evidence, affected revision or template field, confidence, and recommended safe action. Prefer rollback, pause, or config correction guidance over direct mutation unless the user asks to apply a change.
