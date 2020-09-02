# Open Application Model Specification

[![Gitter](https://badges.gitter.im/oam-dev/community.svg)](https://gitter.im/oam-devcommunity?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![License: MIT](https://img.shields.io/badge/License-OWF-yellow)](https://github.com/oam-dev/spec/blob/master/LICENSE)
[![TODOs](https://badgen.net/https/api.tickgit.com/badgen/github.com/oam-dev/spec)](https://www.tickgit.com/browse?repo=github.com/oam-dev/spec)
[![Follow on Twitter](https://img.shields.io/twitter/follow/oam_dev.svg?style=social&logo=twitter)](https://twitter.com/intent/follow?screen_name=oam_dev)

Open Application Model is a runtime-agnostic specification for modeling cloud native applications and building app-centric platforms.

Focused on application development and operation, _Open Application Model_ brings modular, extensible, and portable design to building application-first platforms on any underlying runtime system such as Kubernetes, cloud, or IoT devices.

> **NOTICE:** The current working draft of OAM specification (0.2.x release) is under pre-beta release, which means the specification is still under development but will keep backward compatibility in any further change. Interested in contributing? Take a look at the issues! We're looking for more great ideas on how to model cloud native applications.

## Introduction

Open Application Model provides a series of standard but extensible abstractions to model application in a self-contained approach, which means both the components and corresponding operational capabilities (named "traits") are considered as parts of a given application. This enables platform builders to create platforms around this model, with an app-centric mindset by natural, and essentially changes building platforms into developing components and traits for the application to run. 

In addition, categorizing runtime capabilities into modularized components and traits will also provide better discoverability, manageability and interoperability to these capabilities regardless the runtime systems they rely on. 

![How it works][how-it-works]

In one word, Open Application Model empowers application platforms to provide standardized application centric primitives (e.g. Components and Traits) instead of exposing infrastructure details to end users, while retaining the high flexibility and extensibility for platform builders. This model won't lock you into any specific abstractions -- on the contrary, its primitives are designed to allow you to define the right level of abstraction depend on your own use case.

Open Application Model specification convention adopts [Kubernetes Resource Model](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/resource-management.md) which we believe will become the standard interface for the majority of platforms in the future.

### Team-centric personas

When it comes to the application centric primitives, we think it is important to distinguish between the parts that developers are responsible for, and the parts that operators (or the platform itself) are responsible for. 

This is because in practice different levels of abstraction would apply to different personas. Also, if these primitives get muddled in the platform, a careless operational modification on wrong primitive would easily result in bugs, mishaps or even service outages.

Thus _Open Application Model_ defines different abstractions for different roles responsible for building and operating apps and maintaining infrastructure.

* _Developers_ are responsible for writing business logic, describing what a microservice or component does,
  and _how_ it can be configured. They are the domain experts on the code.
* _Application Operators_ are responsible for configuring the runtime aspects of
  one or more of these microservices. They are the domain experts on the
  platform. In fully automatic use cases (e.g. Serverless), application operator could be the platform itself.
* _Infrastructure Operators_ also known as _Platform Builders_, are responsible for setting up and maintaining the
  infrastructure within which applications run. They are the domain
  experts on developing platform capabilities and infrastructure-level details.

For more details and user stories, see [introduction.md](./introduction.md).

## See it in action

[OAM Kubernetes Runtime](https://github.com/crossplane/oam-kubernetes-runtime) is the officially maintained OAM plugin for Kubernetes, which is a [joint effort](https://cloudblogs.microsoft.com/opensource/2020/05/27/open-application-model-oam-v1alpha2-crossplane/) with [Crossplane](https://github.com/crossplane/crossplane) community. 

## The Specification

  - [Notational Conventions](notational_convention.md)
  - [Purpose and Goals](1.purpose_and_goals.md)
  - [Overview and Terminology](2.overview_and_terminology.md)
  - API Resource Specification
    - Application Developers
      - [Components](4.component.md)
    - Application Operators
      - [Application Scopes](5.application_scopes.md)
      - [Traits](6.traits.md)
      - [Application Configuration](7.application_configuration.md)
    - Infrastructure Operators/Platform Builders
      - [Workload Definition](3.workload.md)
  - [Practical Considerations](8.practical_considerations.md)
  - [Design Principles](9.design_principles.md)

## Versioning

Releases of the specification are versioned according to [Semantic Versioning 2.0](https://semver.org/spec/v2.0.0.html) and described in [OAM release page](https://github.com/oam-dev/spec/releases). Implementations of the specification are required to specify which version they implement.

### Changes to the API objects

Changes to the API objects in this specification follows [on compatibility](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#on-compatibility) and [incompatible api changes](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#incompatible-api-changes) defined in Kubernetes API convention. It doesn't couple with release version of the specification.

Changes to the change process (e.g. governance model, review/approve process etc) itself are not currently versioned but may be independently versioned in the future.


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
| Meeting link | https://zoom.us/j/271516061 |
| APAC Friendly Community meeting | [Bi-weekly APAC (Starting May 19, 2020), Tuesdays 19:00PM GMT+8](https://calendar.google.com/calendar?cid=OGFhaDBxbjBqZDM0c25jamM5bmQ1OXZxajBAZ3JvdXAuY2FsZW5kYXIuZ29vZ2xlLmNvbQ) |
| Meeting link APAC Friendly meeting | https://zoom.com.cn/j/2847572020 |
| Meeting notes| [Notes and agenda](https://docs.google.com/document/d/1nqdFEyULekyksFHtFvgvFAYE-0AMHKoS3RMnaKsarjs) |
| Meeting recordings| [Recordings](https://drive.google.com/drive/folders/1yr5LSB8NkEYxzBL-R9D-z-UwVYx4luLe) |
| IM Channel      | https://gitter.im/oam-dev/ |
| Twitter      | [@oam_dev](https://twitter.com/oam_dev) |

### Resources

Come find community blogs and conference talks about OAM in [community/talks_and_blogs.md](./community/talks_and_blogs.md).
