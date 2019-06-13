# Hydra Specifications

This repository holds the specifications for Hydra-compliant runtimes.

Hydra is the code name for a broad initiative to define open
specifications for an _ops-centric_ "serverless" compute platform as well as an
open source reference implementation thereof.

The Hydra Specifications project is the definitive source for the
open specifications.

*This repository is unstable.* The specification is under development, and breaking changes are made regularly.

## Table of Contents

* The Hydra Application Specification
  1. [Purpose and Goals](1.purpose_and_goals.md)
  2. [Overview and Terminology](2.overview_and_terminology.md)
  3. [The Component Model](3.component_model.md)
  4. [Application Scopes](4.application_scopes.md)
  5. [Traits](5.traits.md)
  6. [Operational Configuration](6.operational_configuration.md)
  7. [Workload Types](7.workload_types.md)
  8. [Practical Considerations](8.practical_considerations.md)


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
