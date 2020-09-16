
# Open Application Model

## Version

This is OAM **spec** working draft version **v0.2.2-WD**.
Learn more about versioning [below](#versioning).

## The Specification

[Notational Conventions](notational_convention.md)

  1. [Purpose and Goals](1.purpose_and_goals.md)
  1. [Overview and Terminology](2.overview_and_terminology.md)
  1. [Components](3.component.md)
  1. [Workload Definition](4.workload_definitions.md)
  1. [Application Scopes](5.application_scopes.md)
  1. [Traits](6.traits.md)
  1. [Application Configuration](7.application_configuration.md)
  1. [Practical Considerations](8.practical_considerations.md)
  1. [Design Principles](9.design_principles.md)

## The Specification Components

|                                | Category      |         API Version            |
| :----------------------------: | :-----------: | :----------------------------: |
| **Workload Types**  |
| Server | core | [v1alpha2](core/workloads/server.md) |
| Task  | core | [v1alpha1-WD](core/workloads/task.md)   |
| CronJob  | core |   v1alpha1-WD      |
| WebService | standard |  v1alpha1-WD |
|                               |
| **Traits**  |
| Manual Scaler  | core |  [v1alpha2](core/traits/manual_scaler_trait.md)          |
| Route  | standard |  v1alpha1-WD      |
| Domain  | standard |  v1alpha1-WD       |
| Rollout  | standard |   v1alpha1-WD        |
| Auto Scaler  | standard | v1alpha1-WD        |
| Monitoring | standard | v1alpha1-WD        |
| Logging | standard | v1alpha1-WD        |
| Cert | standard | v1alpha1-WD        |
|                               |
| **Scopes**  |
| Network Scope  | standard |  [v1alpha2](standard/scopes/network_scope.md)          |
| Health Scope  | core |  [v1alpha2](core/scopes/health_scope.md)          |

## Versioning

Open Application Model specification convention adopts [Kubernetes API resource convention](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/resource-management.md).

Releases of the specification are versioned according to [Semantic Versioning 2.0](https://semver.org/spec/v2.0.0.html) and described in [OAM release page](https://github.com/oam-dev/spec/releases). Implementations of the specification are required to specify which version they implement.

### Changes to the API objects

Changes to the API objects in this specification follows [on compatibility](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#on-compatibility) and [incompatible api changes](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#incompatible-api-changes) defined in Kubernetes API resource convention. It doesn't couple with release version of the specification.

Changes to the change process (e.g. governance model, review/approve process etc) itself are not currently versioned but may be independently versioned in the future.
