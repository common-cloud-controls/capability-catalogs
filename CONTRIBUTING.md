# Contributing to CCC Capability Catalogs

## Overview

This repository contains the capability catalogs for the [FINOS Common Cloud Controls](https://www.finos.org/common-cloud-controls-project) project. Each `capabilities.yaml` file conforms to the Gemara [`CapabilityCatalog`](https://github.com/gemaraproj/gemara) schema — a standardized, machine-readable data model for GRC engineering.

At release time, the [CCC delivery toolkit](https://github.com/finos/common-cloud-controls/tree/main/delivery-toolkit) ingests these files using the [`go-gemara`](https://github.com/gemaraproj/go-gemara) Go SDK and converts them to Markdown and OSCAL artifacts for publication.

## Repository Structure

Capabilities are organized by service domain and type:

```
<domain>/<service-type>/capabilities.yaml
```

For example:
```
storage/object/capabilities.yaml
crypto/key/capabilities.yaml
compute/virtual-machines/capabilities.yaml
```

## Schema

Two top-level keys should be present in every artifact: `capabilities`, and `imports`.

### `capabilities`

A list of capabilities native to this service type. Each entry requires:

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique identifier in the form `CCC.<ServiceCode>.CP<NN>` |
| `title` | string | Short label for the capability |
| `description` | string | Detailed description of what the capability provides |

Example:

```yaml
capabilities:
  - id: CCC.ObjStor.CP01
    title: Storage Buckets
    description: |
      Provides uniquely identifiable segmentations in which data elements may
      be stored.
```

### `imports`

An optional list of references to capabilities defined in another catalog (typically `CCC.Core`) that this service type also supports. Each entry requires a `reference-id` pointing to the source catalog and a list of `entries`, each with a `reference-id` and a `remarks` field summarising applicability.

```yaml
imports:
  - reference-id: CCC
    entries:
      - reference-id: CCC.Core.CP01
        remarks: Encryption in Transit Enabled by Default
```

> **Note:** Catalog-level `metadata` (title, version, publication date, etc.) is not authored here — it is populated automatically at release time by the delivery toolkit.

## Adding a New Capability

1. Locate the relevant `capabilities.yaml` for the service type (e.g. `storage/object/capabilities.yaml`).
2. Append a new entry to the `capabilities` list. Assign the next available `CP` number for that service code.
3. Ensure `id`, `title`, and `description` are all present.
4. If the new capability is a standard platform feature already described in `core/ccc/capabilities.yaml`, add a reference under `imports` instead of duplicating the definition.

## Adding a New Service Type

1. Create a new directory under the appropriate domain (e.g. `networking/cdn/`).
2. Add a `capabilities.yaml` file with a `capabilities` list. Follow the ID convention `CCC.<ServiceCode>.CP<NN>`, choosing a unique service code.
3. Add any applicable core capability references under `imports`.
4. Open a pull request — the catalog title, metadata, and group assignments are resolved by the delivery toolkit and do not need to be authored manually.

## Validation

Capabilities can be validated locally against the Gemara CUE schema using the [CUE CLI](https://cuelang.org/docs/install/):

```sh
cue vet --schema '#CapabilityCatalog' github.com/gemaraproj/gemara <path>/capabilities.yaml
```

## References

- [Gemara schema](https://github.com/gemaraproj/gemara) — CUE schema definitions including `CapabilityCatalog`
- [go-gemara](https://github.com/gemaraproj/go-gemara) — Go SDK used by the delivery toolkit to parse and convert catalogs
- [gemara.openssf.org](https://gemara.openssf.org) — full model documentation
