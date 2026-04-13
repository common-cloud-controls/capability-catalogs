# Capability Catalog Style Guide

This guide defines the writing conventions for entries in CCC service-specific capability catalogs. These capabilities build on the [core catalog](../core-catalog/) and describe what a particular service type can do.

---

## General Conventions

### Voice and Tone

- Write in a professional, technical tone suitable for a security-engineering audience.
- Use present tense throughout.
- Be precise and specific — avoid vague language like "properly", "appropriately", or "as needed".
- Avoid marketing language or subjective qualifiers ("best-in-class", "robust", "comprehensive").

### Title Case

All titles use Title Case. Capitalize every word except:

- Articles: a, an, the
- Short prepositions (four letters or fewer): in, of, for, to, at, by, on, via, with
- Conjunctions: and, or, but, nor

Always capitalize the first and last word regardless of part of speech. Examples: "Encryption in Transit Enabled by Default", "Text-Based Model Selection".

### Formatting

- Use the YAML block literal (`|`) for any multi-line text field (`description`).
- End all sentences with a period.
- Do not end titles with a period or any trailing punctuation.
- Use one blank line between paragraphs within a field, not between sentences.
- Keep entries to the minimum words needed to describe the capability clearly.

---

## Capability Titles

- Use **Title Case** (see [Title Case](#title-case) above).
- Write as a short **noun phrase** describing what the service provides or supports.
- Keep to 2-8 words.
- Do not use imperative verbs (those belong in control titles).

Common title patterns:

| Pattern | Example |
|---|---|
| "[Feature] [State/Quality]" | "Encryption in Transit Enabled by Default" |
| "[Thing] [Noun]" | "Storage Buckets", "General Purpose Instances" |
| "[Algorithm/Standard]" | "RSA-2048", "AES-256-GCM" |
| "[Adjective] [Feature]" | "Configurable Data Retention Period" |
| "[Action]-Based [Feature]" | "Text-Based Model Selection" |

**Good:** "Storage Buckets"
**Good:** "Configurable Data Retention Period"
**Good:** "Text-Based Model Selection"
**Bad:** "Enforce Encryption" (imperative — that's a control title)
**Bad:** "the ability to store objects" (sentence fragment, not title case)

---

## Description

- Write 1-3 complete sentences in present tense.
- Describe what the capability provides, supports, or enables.
- Common opening patterns:
  - "Provides..." — for features the service offers
  - "Supports..." — for standards, protocols, or algorithms
  - "Ability to..." — for user-facing actions
  - "Offers..." — for optional or configurable features
  - "Ensures..." — for guarantees the service makes
- Stay factual and declarative — describe what _is_, not what _should be_.
- Do not describe threats or controls — a capability is a statement of what the service can do, not what risk it mitigates.

**Example:**

```yaml
description: |
  Provides uniquely identifiable segmentations in which data elements
  may be stored. Each segmentation supports independent access controls,
  lifecycle policies, and encryption configurations.
```

**Example for an algorithm:**

```yaml
description: |
  Supports the RSA algorithm with a key size of 2048 bits for
  encryption and digital signatures.
```

**Example for a model capability:**

```yaml
description: |
  Ability to select a foundation model that excels at natural language
  understanding and generation tasks such as summarization, translation,
  text generation, question answering, and sentiment analysis.
```

### Description vs. Title

The title names the capability; the description explains it. Do not repeat the title verbatim as the first words of the description. The description should add context — what it does, how it works, or what it enables.

---

## Group

Every capability must have a `group` field. The group describes the security or operational domain the capability belongs to — not which service it is part of.

Assign the group by asking: **"What domain does this capability primarily serve?"**

Service-specific capabilities that describe what a service _does_ should use the domain group matching the service's primary function:

- IAM capabilities → `Access`
- Logging, monitoring, tracing, audit capabilities → `Observability`
- Key management capabilities → `Encryption`
- GenAI model capabilities → `CCC.Core.MachineLearning`
- VPC and load balancer routing capabilities → `CCC.Core.Networking`
- Pub/sub capabilities → `CCC.Core.Messaging`
- Storage and query capabilities → `CCC.Core.Data`

When a capability spans two domains, pick the one that best describes its _primary function_. For example, "VPC Flow Logs" belongs in `Observability`, not `CCC.Core.Networking`.

See the [group assignment guide](../CLAUDE.md#group-assignment-guide) for the full list of available groups with descriptions.

---

## Imports

When referencing core capabilities in the `imports` section:

- The `remarks` field should match or closely paraphrase the core capability's title.
- Keep remarks concise — they are a label, not a description.

**Example:**

```yaml
imports:
  - reference-id: CCC.Core.Capabilities
    entries:
      - reference-id: CCC.Core.CP01
        remarks: Encryption in Transit Enabled by Default
```

---

## Service-Specific Language

Unlike the core catalog, service-specific capabilities should name the service domain directly. Use the canonical terminology for the service type:

| Service | Use terms like |
|---|---|
| Object Storage | buckets, objects, lifecycle policies, retention |
| Key Management | keys, key versions, key rings, rotation, wrapping |
| Load Balancer | listeners, backends, health checks, targets, routing |
| Virtual Machines | instances, images, boot disks, snapshots, machine types |
| IAM | principals, roles, policies, permissions, federation |
| GenAI | models, prompts, inference, fine-tuning, embeddings |

Avoid cloud-provider-specific product names (S3, GCS, ALB, CloudFront). Use generic service-type terms instead. Note that "KMS" is acceptable as a generic abbreviation for key management service.

---

## Capability vs. Control vs. Threat

If you are unsure whether an entry belongs in the capability catalog, use this test:

| Question | If yes, it's a... |
|---|---|
| Does it describe what the service _can do_? | **Capability** |
| Does it describe what _could go wrong_? | **Threat** (put it in the threat catalog) |
| Does it describe what _must be done_ to stay secure? | **Control** (put it in the control catalog) |
