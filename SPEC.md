
# Open Application Model

## Version

This is Open Application Model **documentation** version **v0.3.0**.
Learn more about versioning [below](#versioning).

## The Documentations

[Notational Conventions](notational_convention.md)

  1. [Purpose and Goals](1.purpose_and_goals.md)
  1. [Overview and Terminology](2.overview_and_terminology.md)
  1. [Component Model](3.component_model.md)
  1. [Workload Types](4.workload_types.md)
  1. [Application Scopes](5.application_scopes.md)
  1. [Traits](6.traits.md)
  1. [Application](7.application.md)
  1. [Practical Considerations](8.practical_considerations.md)
  1. [Design Principles](9.design_principles.md)

## The Model Entities

|                                | Category      |         API Version            |
| :----------------------------: | :-----------: | :----------------------------: |
| ComponentDefiniton | core | v1beta1 |
| WorkloadDefiniton | core | v1beta1 |
| TraitDefinition | core | v1beta1 |
| Application Scope | core | v1beta1 |
| Application  | core | v1beta1   |

## Versioning

For the model objects, Open Application Model adopts [Kubernetes API resource convention](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/resource-management.md).

Releases of the model are versioned according to [Semantic Versioning 2.0](https://semver.org/spec/v2.0.0.html) and described in [OAM release page](https://github.com/oam-dev/spec/releases). Implementations of the model are required to specify which version they implement.

### Changes to the API objects

Changes to the API objects in this model follows [on compatibility](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#on-compatibility) and [incompatible api changes](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#incompatible-api-changes) defined in Kubernetes API resource convention. It doesn't couple with release version of the model.

Changes to the change process (e.g. governance model, review/approve process etc) itself are not currently versioned but may be independently versioned in the future.
