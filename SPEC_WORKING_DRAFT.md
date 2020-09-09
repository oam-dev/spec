
# Open Application Model

## Version

This is OAM **spec** working draft version **v0.2.2-WD**.
Learn more about versioning [below](#versioning).

## The Specification

  - [Notational Conventions](notational_convention.md)
  - [Purpose and Goals](1.purpose_and_goals.md)
  - [Overview and Terminology](2.overview_and_terminology.md)
  - API Resource Specification
    - Application Developers
      - [Components](3.component.md)
    - Application Operators
      - [Traits](6.traits.md)
      - [Application Scopes](5.application_scopes.md)
      - [Application Configuration](7.application_configuration.md)
    - Infrastructure Operators/Platform Builders
      - [Workload Definition](4.workload_definitions.md)
  - [Practical Considerations](8.practical_considerations.md)
  - [Design Principles](9.design_principles.md)

## Versioning

Open Application Model specification convention adopts [Kubernetes Resource Model](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/resource-management.md) which we believe will become the standard interface for the majority of platforms in the future.

Releases of the specification are versioned according to [Semantic Versioning 2.0](https://semver.org/spec/v2.0.0.html) and described in [OAM release page](https://github.com/oam-dev/spec/releases). Implementations of the specification are required to specify which version they implement.

### Changes to the API objects

Changes to the API objects in this specification follows [on compatibility](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#on-compatibility) and [incompatible api changes](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#incompatible-api-changes) defined in Kubernetes API convention. It doesn't couple with release version of the specification.

Changes to the change process (e.g. governance model, review/approve process etc) itself are not currently versioned but may be independently versioned in the future.