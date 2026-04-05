# CCC Capability Catalogs

This repository contains the capability catalogs for the [FINOS Common Cloud Controls](https://www.finos.org/common-cloud-controls-project) project — machine-readable definitions of the observable features and behaviours provided by cloud service types.

Capabilities describe what a service *can do*, forming the foundation from which threats and controls are derived in the corresponding [threat-catalogs](https://github.com/common-cloud-controls/threat-catalogs) and [control-catalogs](https://github.com/common-cloud-controls/control-catalogs) repositories.

## Repository Structure

Catalogs are organised by service domain and type:

```
<domain>/<service-type>/capabilities.yaml
```

For example:

```
storage/object/capabilities.yaml
compute/virtual-machines/capabilities.yaml
crypto/key/capabilities.yaml
```

Cross-cutting capabilities that apply to all service types are maintained separately in [core-catalog](https://github.com/common-cloud-controls/core-catalog) and referenced here via the `imports` key.

## Delivery

At release time, the [CCC delivery toolkit](https://github.com/finos/common-cloud-controls/tree/main/delivery-toolkit) ingests these files and produces versioned Markdown artifacts for publication.

## License

[Community Specification License 1.0](LICENSE)
