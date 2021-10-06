# Open Application Model

[![Gitter](https://badges.gitter.im/oam-dev/community.svg)](https://gitter.im/oam-devcommunity?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![License: MIT](https://img.shields.io/badge/License-OWF-yellow)](https://github.com/oam-dev/spec/blob/master/LICENSE)
[![TODOs](https://badgen.net/https/api.tickgit.com/badgen/github.com/oam-dev/spec)](https://www.tickgit.com/browse?repo=github.com/oam-dev/spec)
[![Follow on Twitter](https://img.shields.io/twitter/follow/oam_dev.svg?style=social&logo=twitter)](https://twitter.com/intent/follow?screen_name=oam_dev)

Open Application Model (OAM) is a set of standard yet higher level abstractions for modeling cloud native applications on top of today's hybrid and multi-cloud environments.

Focused on **application** rather than container or orchestrator, Open Application Model brings modular, extensible, and portable design for defining application deployment with higher level API. This is the key to enable simple, consistent yet robust application delivery across hybrid environments including Kubernetes, cloud, or even IoT devices.

## Introduction

_"Developers think in terms of application architecture, not of infrastructure."_

![How it works](assets/how-it-works.png)

### Why Open Application Model?

In today's hybrid deployment environments, delivering applications without application context is hard:

- Developers spend time on infrastructure details instead of apps - clusters, ingresses, labels, DNS, etc, and learning how the infrastructure is implemented in different environments
- Inextensible - upper layer platform may be introduced, but it's almost certain that the needs of your app will outgrow the capabilities of that platform soon.
- Vendor lock-in - app deployment is tightly coupled with service provider and infrastructure, which heavily impact on how you configure, develop and operate the app across hybrid environments.

In Open Application Model, we propose an app-centric approach instead:

- Application first - define the app deployment with a self-contained model, where operational behaviors as part of app definition, free of infrastructure, simply deploy.
- Clarity and extensibility - an open standard to modularize app delivery into reusable pieces, assemble them into a deployment plan per your own needs, fully self-service.
- Vendor agnostic - a consistent yet higher level abstraction to model app delivery across on-prem clusters, cloud providers or even edge devices. Zero lock-in.

The design of Open Application Model is driven by [KubeVela](https://github.com/oam-dev/kubevela) project - a modern application deployment platform that intends to make delivering and managing applications across today's hybrid, multi-cloud environments easier and faster. 

## Learn the Model

The model is maintained as a set of versioned API documentations as shown below.

|                                | Previous Releases | Latest Release |    Working Draft                  |
| :----------------------------: | :-----------------: | :------------: |:--------------------------------: |
| OAM Release Versions              | [v0.2.1](https://github.com/oam-dev/spec/releases/tag/v0.2.1) (KubeVela v0.3.x) | [v0.3.0](SPEC.md) (KubeVela v1.x) |  [v0.3.1](SPEC_DRAFT.md) (KubeVela v1.x)   |

For OAM release [v0.1.0](https://github.com/oam-dev/spec/releases/tag/v0.1.0),  it is only supported in [Rudr](https://github.com/oam-dev/rudr) and now archived.

## Community

### Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) guide for detailed information.

### Meeting Hours

| Item        | Value  |
|---------------------|---|
| Mailing List | https://groups.google.com/forum/#!forum/oam-dev |
| Bi-weekly OAM Community Call (English) | [Zoom](https://us02web.zoom.us/j/88638962723?pwd=MVhCZnNub2t0R3BmMUNEWE9vendLUT09), [Meeting Notes](https://docs.google.com/document/d/1nqdFEyULekyksFHtFvgvFAYE-0AMHKoS3RMnaKsarjs), [Records (YouTube)](https://www.youtube.com/channel/UCSCTHhGI5XJ0SEhDHVakPAA/)  |
| Bi-weekly APAC Friendly Meeting (Chinese)| [Zoom](https://us02web.zoom.us/j/2804785490?pwd=ZTN4ZU03UTlBZzlmVHIwTndINGM3UT09), [Meeting Notes](https://shimo.im/docs/w8CgdyYGWjtYJ3XP), [Records (BiliBili)](https://space.bilibili.com/180074935?spm_id_from=333.788.b_765f7570696e666f.2) |
| IM Channel      | https://gitter.im/oam-dev/ |
| Twitter      | [@oam_dev](https://twitter.com/oam_dev) |


## Copyrights

Both Open Application Model and KubeVela projects are hosted in [Cloud Native Computing Foundation (CNCF)](https://cncf.io), all copyrights belong to CNCF.
