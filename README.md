# Open Application Model Specification

From the creators of 
[Helm](https://helm.sh),
[OpenKruise](https://openkruise.io/en-us/),
the Open Application Model is a specification for building cloud native applications.

Focused on separating concerns of development and operation needs, _Open Application Model_ brings modular, extensible, and portable design to building and delivering applications on platforms like Kubernetes.

*This repository is unstable, and open to contributions.*
The specification is under development, and breaking changes could be made.
Interested in contributing? Take a look at the issue queue! We're looking for more
 ideas on how to model cloud native applications.

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
  3. [The Component Model](3.component_model.md)
  4. [Application Scopes](4.application_scopes.md)
  5. [Traits](5.traits.md)
  6. [Application Configuration](6.application_configuration.md)
  7. [Workload Types](7.workload_types.md)
  8. [Practical Considerations](8.practical_considerations.md)
  9. [Design Principles](9.design_principles.md)


## Community

### Milestones

To get an overview of the milestones and their description please visit the [Milestones](https://github.com/microsoft/hydra-spec/milestones) page. 

### Triaging 

Triaging of items into milestones will occur during the bi-weekly community call. During this call, issues might be brought into milestones, removed from milestones or moved between milestones. 

### Contributing

See the [CONTRIBUTING](contributing.md) guide for more information about submitting changes to the spec.

One of the easiest ways to contribute is to participate in discussions. There are several ways to get involved.

| Item        | Value  |
|---------------------|---|
| Mailing List | https://groups.google.com/forum/#!forum/oam-dev |
| Community meeting info | Bi-weekly (Starting Oct 22, 2019), Tuesdays 10:30AM PST  |
| Community meeting link | https://zoom.us/j/271516061 |
| IM Channel      | https://gitter.im/oam-dev/ |

[how-it-works]: assets/how-it-works.png
