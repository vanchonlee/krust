---
name: incident-notes
description: Produce concise Kubernetes incident notes from observed symptoms, evidence, timeline, impact, and next actions.
metadata:
  short-description: Create incident notes from cluster evidence.
---

# Incident Notes

## Goal

Produce concise Kubernetes incident notes from observed evidence. The notes should help an operator hand off context, decide next checks, and avoid inventing a root cause.

## Rules

- Summarize only evidence observed in Krust or provided by the user.
- Separate confirmed facts, hypotheses, and unknowns.
- Preserve exact resource names, namespaces, timestamps, symptoms, and error messages when available.
- If root cause is not proven, write "root cause not confirmed" and list the checks needed to confirm it.
- Avoid blame language and speculative certainty.

## Output Format

Use these sections:

- Summary: one or two sentences.
- Impact: affected users, services, namespaces, workloads, and severity if known.
- Timeline: ordered events and timestamps.
- Evidence: logs, events, conditions, metrics, rollout changes, and user observations.
- Current Hypotheses: ranked with confidence.
- Mitigations Attempted: what changed and observed result.
- Next Checks: specific checks that would increase confidence.
- Next Safe Actions: low-risk actions only.
- Owner/Follow-up: team or person if known.

Keep the tone operational and concise. Do not recommend destructive action unless the user explicitly asks.
