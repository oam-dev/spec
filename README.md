# Open Application Model Specification

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

|![notification](assets/bell-outline-badge.svg) What is NEW!|
|------------------|
|Mar 27th, 2020. OAM v1.0.0-alpha.2 is **RELEASED**! The new spec is highly extensible and native to Kubernetes runtime. Check the spec and [What's new in OAM v1alpha2](https://speakerdeck.com/resouer/whats-new-in-oam-v1alpha2-spec) for more detail!|
|Mar 26th, 2020. A proof-of-concept project named AWS ECS for OAM is published! Check [the AWS Labs repo ](https://github.com/awslabs/amazon-ecs-for-open-application-model) and have fun with developer centric experience with OAM + Fargate!|

Open Application Model is a specification for building cloud native applications.

Focused on separating concerns of development and operation needs, _Open Application Model_ brings modular, extensible, and portable design to building and delivering applications on platforms like Kubernetes.

*This repository is unstable, and open to contributions.*
The specification is under development, and breaking changes could be made.
Interested in contributing? Take a look at the issue queue! We're looking for more
great ideas on how to model cloud native applications.

## Introduction

![How it works][how-it-works]

When it comes to application development and deployment, we think it is important to distinguish between the parts that developers are responsible for, and the parts that operations is responsible for. Otherwise, if these roles get muddled, it would result in communications mishaps, bugs, or even
service outages.

_Open Application Model_ attempts to solve this problem by modeling the application according to the
roles responsible for building and running apps and operating infrastructure.

* _Developers_ are responsible for describing what a microservice or component does,
  and _how_ it can be configured. They are the domain experts on the code.
* _Application Operators_ are responsible for configuring the runtime aspects of
  one or more of these microservices. They are the domain experts on the
  platform.
* _Infrastructure Operators_ are responsible for setting up and maintaining the
  infrastructure within which applications run. They are the domain
  experts on the low-level details.

For more details and user stories, see [introduction.md](./introduction.md).

## See it in action

[Rudr](https://github.com/oam-dev/rudr) is a reference implementation of the Open Application Model specification for Kubernetes. To get started with an example on the Open Application Model, go to the [Rudr Tutorial](https://github.com/oam-dev/rudr/blob/master/docs/tutorials/deploy_and_update.md) guide.

## The Specification

[Notational Conventions](notational_convention.md)

  1. [Purpose and Goals](1.purpose_and_goals.md)
  2. [Overview and Terminology](2.overview_and_terminology.md)
  3. [Workload Types](3.workload.md)
  4. [The Component Model](4.component.md)
  5. [Application Scopes](5.application_scopes.md)
  6. [Traits](6.traits.md)
  7. [Application Configuration](7.application_configuration.md)
  8. [Practical Considerations](8.practical_considerations.md)
  9. [Design Principles](9.design_principles.md)


## Community

### Milestones

To get an overview of the milestones and their description please visit the [Milestones](https://github.com/microsoft/hydra-spec/milestones) page. 

### Triaging 

Triaging of items into milestones will occur during the bi-weekly community call. During this call, issues might be brought into milestones, removed from milestones or moved between milestones. 

### Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) guide for more information about submitting changes to the spec.

One of the easiest ways to contribute is to participate in discussions. There are several ways to get involved.

| Item        | Value  |
|---------------------|---|
| Mailing List | https://groups.google.com/forum/#!forum/oam-dev |
| Community meeting info | [Bi-weekly (Starting Oct 22, 2019), Tuesdays 10:30AM PST](https://calendar.google.com/calendar?cid=dDk5YThyNGIwOWJyYTJxajNlbWI0a2FvdGtAZ3JvdXAuY2FsZW5kYXIuZ29vZ2xlLmNvbQ)  |
| APAC Friendly Community meeting | [Bi-weekly APAC (Starting Dec 24, 2019), Tuesdays 1:00PM GMT+8](https://calendar.google.com/event?action=TEMPLATE&tmeid=MzJnbHR2b3R1bHYxMG0wc2YybDJjZmhuc2pfMjAxOTEyMjRUMDUwMDAwWiBmZW5namluZ2NoYW9AbQ&tmsrc=fengjingchao%40gmail.com&scp=ALL) |
| Meeting link | https://zoom.us/j/271516061 |
| Meeting notes| [Notes and agenda](https://docs.google.com/document/d/1nqdFEyULekyksFHtFvgvFAYE-0AMHKoS3RMnaKsarjs) |
| Meeting recordings| [Recordings](https://drive.google.com/drive/folders/1yr5LSB8NkEYxzBL-R9D-z-UwVYx4luLe) |
| IM Channel      | https://gitter.im/oam-dev/ |
| Twitter      | [@oam_dev](https://twitter.com/oam_dev) |

[how-it-works]: assets/how-it-works.png

### Resources

Come find community blogs and conference talks about OAM in [community/talks_and_blogs.md](./community/talks_and_blogs.md).
