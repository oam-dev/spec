# Hydra Specifications

From the creators of 
[Helm](https://helm.sh),
[OpenKruise](https://openkruise.io/en-us/),
and [Service Fabric](https://github.com/Microsoft/service-fabric),
Hydra is a specification for building cloud native applications.

Focused on the separation of development concerns from operational considerations,
Hydra provides a set of tools for building high-grade containerized applications
on systems like Kubernetes.

*This repository is unstable, and open to contributions.*
The specification is under development, and breaking changes are made regularly.
Interested in contributing? Take a look at the issue queue! We're looking for more
great ideas on how to model cloud native applications.

## The Main Idea

When it comes to the practice of software development and deployment, we think it is
important to distinguish between the parts of the software that developers are
responsible for, and the parts that operations is responsible for. Too often,
these roles get muddled, resulting in communications mishaps, bugs, or even
service outages.

Hydra attempts to solve this problem by modeling the application according to the
roles responsible for building and running apps and infrastructure.

* _Developers_ are responsible for describing what a microservice or component does,
  and _how_ it can be configured. They are the domain experts on the code.
* _Application Operators_ are responsible for configuring the runtime aspects of
  one or more of these microservices. They are the domain experts on the
  platform.
* _Infrastructure Operators_ are responsible for setting up and maintaining the
  infrastructure within which applications run. They are the domain
  experts on the low-level details.

Hydra describes a model where developers are responsible for defining _components_,
application operators are responsible for creating instances of those components and
assigning them _application configurations_. And infrastructure operators are
responsible for declaring, installing, and maintaining the underlying services that
are available on the platform.

For example, a _developer_ creates web application. The _application operator_ creates
an instance of that application, and configures it to autoscale with load. The
_infrastructure operator_ decides which underlying autoscaling technology is
used to do the scaling.

## About This Repository

This repository holds the specifications for Hydra-compliant runtimes.

Hydra is the code name for a broad initiative to define open
specifications for composing and managing cloud native applications as well as an
open source reference implementation thereof.

The Hydra Specifications project is the definitive source for the
open specifications.

## Table of Contents

* The Hydra Application Specification
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

## Contributing

Hydra Specifications follow the [CNCF Code of Conduct][cncf-coc]. See the [CONTRIBUTING](contributing.md) guide for more information about submitting changes to the spec.

## Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" are to be
interpreted as described in [RFC 2119][rfc2119].

The key words "unspecified", "undefined", and "implementation-defined" are to be
interpreted as described in the [rationale for the C99
standard][c99-unspecified].

An implementation IS compliant if it satisfies all the MUST, REQUIRED, and SHALL
requirements.

An implementation IS NOT compliant if it fails to satisfy one or more of the
MUST, REQUIRED, or SHALL requirements.

[cncf-coc]: https://github.com/cncf/foundation/blob/master/code-of-conduct.md
[rfc2119]: http://tools.ietf.org/html/rfc2119
[c99-unspecified]: http://www.open-std.org/jtc1/sc22/wg14/www/C99RationaleV5.10.pdf#page=18
