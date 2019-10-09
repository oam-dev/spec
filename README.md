# Open Application Model Specification

From the creators of 
[Helm](https://helm.sh),
[OpenKruise](https://openkruise.io/en-us/),
and [Service Fabric](https://github.com/Microsoft/service-fabric),
the Open Application Model is a specification for building cloud native applications.

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

[Scylla](https://github.com/microsoft/scylla) is a reference implementation of the Open Application Model specification for Kubernetes. To get started with an example on the Open Application Model, go to the [Scylla getting started](https://github.com/microsoft/scylla/blob/master/docs/quickstart/quickstart.md) guide.

## Specification

  1. [Purpose and Goals](1.purpose_and_goals.md)
  2. [Overview and Terminology](2.overview_and_terminology.md)
  3. [The Component Model](3.component_model.md)
  4. [Application Scopes](4.application_scopes.md)
  5. [Traits](5.traits.md)
  6. [Application Configuration](6.application_configuration.md)
  7. [Workload Types](7.workload_types.md)
  8. [Practical Considerations](8.practical_considerations.md)
  9. [Design Principles](9.design_principles.md)
  10. [Appendix A: Extended Workload Types](10.appendix-extended-workload-types.md)


## Community

### Milestones

To get an overview of the milestones and their description please visit the [Milestones](https://github.com/microsoft/hydra-spec/milestones) page. 

### Triaging 

Triaging of items into milestones will occur during the bi-weekly community call. During this call, issues might be brought into milestones, removed from milestones or moved between milestones. 

### Contributing

Hydra Specifications follow the [CNCF Code of Conduct][cncf-coc]. See the [CONTRIBUTING](contributing.md) guide for more information about submitting changes to the spec.

Below are links to join the bi-weekly community meetings and our meeting notes. Community Slack channels & mailing lists will be added shortly (~ 10/1). 

| Item        | Value  |
|---------------------|---|
| Mailing List | TBD |
| Meeting Information | Bi-weekly (Starting Sept 24th), Tuesdays 10:30AM PST  |
| Meeting Link | https://zoom.us/j/623691799?pwd=ZWc4SHFNdWpRUVVNYkdJWE9zVHpjZz09   |
| Slack Channel       | TBD  |
| Meeting Notes       | https://docs.google.com/document/d/1nqdFEyULekyksFHtFvgvFAYE-0AMHKoS3RMnaKsarjs/edit?usp=sharing |

[cncf-coc]: https://github.com/cncf/foundation/blob/master/code-of-conduct.md

[how-it-works]: assets/how-it-works.png