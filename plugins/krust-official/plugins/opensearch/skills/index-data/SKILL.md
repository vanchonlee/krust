---
name: index-data
description: Explore indexed data in a self-hosted OpenSearch cluster. Use to list indices, inspect an index mapping, understand available fields, or search documents. Do not use this skill for cluster health, shard allocation, node pressure, pending tasks, or other SRE diagnostics.
---

# OpenSearch Indexed Data

Use the OpenSearch native MCP tools for read-only indexed-data exploration.

When multiple OpenSearch connections are configured, choose the MCP tool namespace whose server name corresponds to the user-requested connection. Never combine index or mapping evidence from different connections. If the intended connection is unclear, ask the user to name it.

## Workflow

1. Call `ListIndexTool` to discover candidate indices when the target index is not already known.
2. Call `IndexMappingTool` before constructing a search when field names or types are uncertain.
3. Call `SearchIndexTool` with the narrowest relevant index selection and query.
4. Report the index, query scope, matching evidence, and any mapping assumptions.

## Tool boundaries

- Use only indices and fields confirmed by tool output or explicitly supplied by the user.
- Narrow time ranges and result sizes before broadening a search.
- Treat missing fields and empty results as evidence about the query, not proof that an event never occurred.
- If the task concerns cluster health, allocation, nodes, JVM, disk, tasks, or hot threads, use the OpenSearch Operations skill instead.

## Safety

- Keep all OpenSearch activity read-only.
- Do not modify documents, mappings, settings, aliases, templates, shards, or cluster configuration.
