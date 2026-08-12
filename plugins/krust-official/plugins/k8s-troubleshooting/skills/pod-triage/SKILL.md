---
name: pod-triage
description: Triage Kubernetes pods that are Pending, CrashLoopBackOff, Error, OOMKilled, or not Ready. Use when investigating failing pods or workload health.
metadata:
  short-description: Triage failing pods and readiness issues.
---

# Pod Triage

## Goal

Find the first failing layer for unhealthy pods without guessing. Use native Krust data first: selected workload, owner chain, pod list, pod status, container state, restart counts, events, logs, probes, images, env/config references, resources, nodes, and recent rollout changes.

## Workflow

1. Identify scope: one pod, all pods for a workload, or a namespace-wide pattern.
2. Classify the failure mode before drilling down: scheduling, image pull, init container, runtime crash, OOM, probe/readiness, node pressure, config/secret, or unknown.
3. Prefer controller-level evidence when multiple pods fail the same way; prefer pod/container-level evidence when only one replica is affected.
4. Correlate recent events, rollout revision, image, config, resource, and node changes before recommending action.

## Decision Tree

- If pod is `Pending`, inspect scheduling events, node selectors, affinity, taints/tolerations, PVC binding, resource requests, quota, and node pressure.
- If pod is `ImagePullBackOff` or `ErrImagePull`, inspect image name/tag, registry auth, pull policy, and recent image changes.
- If init containers fail, inspect init container state/logs before app containers; do not diagnose the main app until init is complete.
- If pod is `CrashLoopBackOff`, inspect previous logs, exit code, signal, command/args, env/config references, dependency startup assumptions, and probe-triggered restarts.
- If pod is `OOMKilled`, compare memory limit, working set trend if metrics exist, restart time, node pressure, and recent traffic/config changes.
- If pod is running but not ready, compare readiness probe path/port/scheme with container ports, service targetPort, app listen address, and dependency readiness.
- If only one replica fails, check node, volume, local resource pressure, and pod-specific event differences.

## Output

Return a concise incident-style summary with: failing layer, evidence, confidence, likely cause, missing checks if confidence is low, and safest next action. Do not suggest deletes, restarts, rollbacks, or spec changes unless the user explicitly asks.
