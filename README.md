# Open Application Model Specification

[![Gitter](https://badges.gitter.im/oam-dev/community.svg)](https://gitter.im/oam-devcommunity?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![License: MIT](https://img.shields.io/badge/License-OWF-yellow)](https://github.com/oam-dev/spec/blob/master/LICENSE)
[![TODOs](https://badgen.net/https/api.tickgit.com/badgen/github.com/oam-dev/spec)](https://www.tickgit.com/browse?repo=github.com/oam-dev/spec)
[![Follow on Twitter](https://img.shields.io/twitter/follow/oam_dev.svg?style=social&logo=twitter)](https://twitter.com/intent/follow?screen_name=oam_dev)

Open Application Model (OAM) is a runtime-agnostic specification for defining cloud native applications and enable building application-centric platforms by natural.

Focused on **application** rather than container or orchestrator, _Open Application Model_ brings modular, extensible, and portable design for modeling cloud native applications and enable building application-centric platforms on any runtime infrastructure like Kubernetes, cloud, or IoT devices.

### Why Open Application Model?

Platforms without application context is hard:

- Developers spend time on infrastructure details instead of applications - ingress, labels, iptables rules, DNS, etc, and learning how the platform is implemented.
- Restricted platform capabilities - in-house APIs with opinionated abstractions and implementations, lack of interoperability.
- Runtime lock-in - platform is tightly coupled with execution runtime, which heavily impact on how you configure, develop and operate your application.

In Open Application Model, we propose an app-centric approach instead:

- Application first - build the platform around a self-contained app model, where operational features as part of app definition, free of infrastructure here.
- Clarity and extensibility - an open standard to modularize your platform capabilities into reusable pieces, with freedom to bring your own abstractions and implementations.
- Runtime agnostic - a consistent experience to deploy and operate your apps across on-prem clusters, cloud providers or even edge devices.

> **NOTICE:** The current working draft of OAM specification (0.2.x release) is under pre-beta release, it's still under development but will keep backward compatibility for any further change.

## Introduction

_"Developers think in terms of application architecture, not of infrastructure."_

Open Application Model defines a series of standard but extensible abstractions to model micro-service applications, with operation features as part of the application definition. This enables platform builders to create runtime-agnostic systems around a unified model, by developing modularized components and traits, and essentially serve their customers (e.g. developers) with app-centric mindset by default.

![How it works](assets/how-it-works.png)

### Team-centric and separation of concerns

Open Application Model proposed a clear separation of concerns between the parts that developers are responsible for, and the parts that operators and platform engineers are responsible for. For more details, see [introduction.md](./introduction.md).

## Read the specification

|                                | Latest Release |    Working Draft                  |
| :----------------------------: | :------------: |:--------------------------------: |
| **Core Specification:**        |
| OAM Specification              | [v0.2.1](https://github.com/oam-dev/spec/blob/v0.2.1/SPEC_LATEST_STABLE.md) |  [v0.2.2-WD](SPEC.md)  |

## See it in action

- [KubeVela](https://github.com/oam-dev/kubevela): the highly extensible application platform based on Kubernetes and OAM.

## Community

### Milestones

To get an overview of the milestones and their description please visit the [Milestones](https://github.com/oam-dev/spec/milestones) page. 

### Triaging 

Triaging of items into milestones will occur during the bi-weekly community call. During this call, issues might be brought into milestones, removed from milestones or moved between milestones. 

### Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) guide for more information about submitting changes to the spec.

One of the easiest ways to contribute is to participate in discussions. There are several ways to get involved.

| Item        | Value  |
|---------------------|---|
| Mailing List | https://groups.google.com/forum/#!forum/oam-dev |
| Community meeting info | [Bi-weekly (Starting Oct 22, 2019), Tuesdays 10:30AM PST](https://calendar.google.com/calendar?cid=dDk5YThyNGIwOWJyYTJxajNlbWI0a2FvdGtAZ3JvdXAuY2FsZW5kYXIuZ29vZ2xlLmNvbQ)  |
| Meeting link | https://zoom.us/j/2847572020 |
| APAC Friendly Community meeting | [Bi-weekly APAC (Starting May 19, 2020), Tuesdays 19:00PM GMT+8](https://calendar.google.com/calendar?cid=OGFhaDBxbjBqZDM0c25jamM5bmQ1OXZxajBAZ3JvdXAuY2FsZW5kYXIuZ29vZ2xlLmNvbQ) |
| Meeting link APAC Friendly meeting | https://zoom.com.cn/j/2847572020 |
| Meeting notes| [Notes and agenda](https://docs.google.com/document/d/1nqdFEyULekyksFHtFvgvFAYE-0AMHKoS3RMnaKsarjs) |
| Meeting recordings| [Recordings](https://drive.google.com/drive/folders/1yr5LSB8NkEYxzBL-R9D-z-UwVYx4luLe) |
| IM Channel      | https://gitter.im/oam-dev/ |
| Twitter      | [@oam_dev](https://twitter.com/oam_dev) |

### Resources

Come find community blogs and conference talks about OAM in [community talks and blogs](./community/talks_and_blogs.md).
