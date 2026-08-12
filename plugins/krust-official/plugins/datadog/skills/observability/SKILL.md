---
name: observability
description: Use for Datadog metrics, logs, monitors, dashboards, traces, events, and Datadog incident workflows.
metadata:
  short-description: Investigate systems with Datadog telemetry
---

# Datadog Observability

Use Datadog MCP tools for metrics, logs, monitors, dashboards, traces, events, and incidents explicitly associated with Datadog. Do not route PagerDuty-specific incident or on-call requests to Datadog.

Search for only the tools needed by the current investigation. Establish site, service, environment, and time range from the request or ask for the missing value when it materially changes the result. Start with read-only queries and summarize the evidence before proposing changes.

Never ask the user to paste credentials into chat. Authentication is managed by the enabled plugin.
