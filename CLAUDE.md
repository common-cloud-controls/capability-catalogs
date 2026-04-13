# CLAUDE.md

## Overview

This repository contains capability catalogs for the [FINOS Common Cloud Controls](https://commoncloudcontrols.com) project. Each `capabilities.yaml` file conforms to the Gemara `CapabilityCatalog` schema.

## Gemara MCP Server

This repo includes a `.mcp.json` that configures the Gemara MCP server. Use it to **validate catalog files** against the schema during editing:

- `validate_gemara_artifact` — validate a capabilities.yaml file against the Gemara schema
- `migrate_gemara_artifact` — migrate a file to a newer schema version
- `gemara://schema/definitions` — browse the schema type definitions
- `gemara://lexicon` — look up Gemara terminology

**Always validate after making changes** to a capabilities.yaml file.

## File Structure

```
<category>/<service>/capabilities.yaml
```

Each file has:
- `imports:` — references to core capabilities (`CCC.Core.Capabilities`)
- `capabilities:` — flat list of service-specific capability entries

## Entry Structure

```yaml
capabilities:
  - id: CCC.<Service>.CP<nn>
    title: Human-readable title
    group: <GroupID>
    description: |
      What this capability does.
```

## Groups

Every capability must have a `group:` field. The group describes what operational domain the entry belongs to, not what service it's part of.

| Group ID | Use When The Entry Is About... |
|---|---|
| `Encryption` | Cryptographic protection — encryption, key management, certificates |
| `Access` | Authentication, authorization, trust perimeters, least privilege |
| `Observability` | Logging, metrics, alerting, tracing, audit trails |
| `Data` | Replication, backup, recovery, data retention, storage, queries |
| `Resource` | Resource lifecycle, scaling, cost management, tagging |
| `Compute` | CPU, memory, storage allocation, runtime, execution |
| `Ingestion` | Active/passive data ingestion, CDC, connectors |
| `Networking` | VPCs, subnets, routing, DNS, load balancing |
| `Orchestration` | Container orchestration, CI/CD, job scheduling |
| `Processing` | ETL, stream/batch processing, data lineage |
| `Messaging` | Pub/sub, topics, message ordering, delivery guarantees |
| `MachineLearning` | ML environments, model registries, GenAI, inference |

**Decision rule:** Ask "what breaks if this capability is missing?" and pick the group that matches the answer. Service-specific capabilities that describe what a service *does* should use the group matching the service's primary function (e.g., all IAM capabilities → `Access`).
