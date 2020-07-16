# Open Application Model Specification

|![notification](assets/bell-outline-badge.svg) What is NEW!|
|------------------|
|May 18th, 2020. [Crossplane](https://github.com/crossplane/crossplane) becomes the standard Kubernetes implementation of OAM spec!|
|Mar 27th, 2020. OAM v1.0.0-alpha.2 is **RELEASED**! The new spec is highly extensible and native to Kubernetes runtime. Check the spec and [What's new in OAM v1alpha2](https://speakerdeck.com/resouer/whats-new-in-oam-v1alpha2-spec) for more detail!|
|Mar 26th, 2020. A proof-of-concept project named AWS ECS for OAM is published! Check [the AWS Labs repo ](https://github.com/awslabs/amazon-ecs-for-open-application-model) and have fun with developer centric experience with OAM + Fargate!|

Open Application Model is a platform-agnostic specification for building cloud native applications.

Focused on separating concerns of development and operation needs, _Open Application Model_ brings modular, extensible, and portable design to building and delivering applications on different platforms.

> **NOTICE:** *This repository is unstable and open to contributions.* The specification is under development and could adopt breaking changes in the future. Interested in contributing? Take a look at the issues! We're looking for more great ideas on how to model cloud native applications.

## Introduction

Open Application Model provides a standard and extensible framework for platform builders to create application centric platforms on top of lower level runtime systems such as Kubernetes with additions of discoverability, manageability and interoperability. 

![How it works][how-it-works]

Open Application Model empowers application platforms to provide standardized application centric primitives (e.g. Workloads and Traits) instead of exposing infrastructure details to end users, while retaining the extensibility of the underlying runtime system. With the idea of simplify creating upper layer platforms, the model won't lock you into specific abstractions -- on the contrary, its primitives allow you to define the right level of abstraction depend on your own use case.

### A team-centric model

When it comes to the application centric primitives, we think it is important to distinguish between the parts that developers are responsible for, and the parts that operators (or the platform itself) is responsible for. Otherwise, if these roles get muddled, it would result in communications mishaps, bugs, or even service outages.

_Open Application Model_ attempts to solve this problem by modeling the application according to the
roles responsible for building and running apps and operating infrastructure.

* _Developers_ are responsible for describing what a microservice or component does,
  and _how_ it can be configured. They are the domain experts on the code.
* _Application Operators_ are responsible for configuring the runtime aspects of
  one or more of these microservices. They are the domain experts on the
  platform.
* _Infrastructure Operators_ also known as _Platform Builders_, are responsible for setting up and maintaining the
  infrastructure within which applications run. They are the domain
  experts on the low-level details.

For more details and user stories, see [introduction.md](./introduction.md).

## See it in action

[OAM Kubernetes Runtime](https://github.com/crossplane/oam-kubernetes-runtime) is the officially maintained OAM plugin for Kubernetes. 

OAM Kubernetes Runtime is a [joint effort](https://cloudblogs.microsoft.com/opensource/2020/05/27/open-application-model-oam-v1alpha2-crossplane/) with [the Crossplane community](https://github.com/crossplane/crossplane). Furthermore, to get started with an example of using OAM to deliver both applications and cloud resources in unified approach, please follow the end-to-end [getting started doc in Crossplane](https://crossplane.io/docs/v0.12/getting-started/run-applications.html).

## The Specification

The specification convention adopts [Kubernetes Resource Model](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/resource-management.md) which we believe will become the standard interface for the majority of platforms in the future.

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


## Community

### Versioning

Since July 2020, changes to the specification are versioned according to [Semantic Versioning 2.0](https://semver.org/spec/v2.0.0.html) and described in [its release page](https://github.com/oam-dev/spec/releases). Layout (e.g. repo structure, doc format etc) changes are not versioned. Specific implementations of the specification (e.g. OAM Kubernetes Runtime ) should specify which version they implement.

Changes to the change process (e.g. governance model, review/approve process etc) itself are not currently versioned but may be independently versioned in the future.

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

[how-it-works]: assets/how-it-works.png

### Resources

Come find community blogs and conference talks about OAM in [community/talks_and_blogs.md](./community/talks_and_blogs.md).
