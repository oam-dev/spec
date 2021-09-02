
# Open Application Model

## Version

This is Open Application Model **draft proposal** version **v0.3.1**.
Learn more about versioning [below](#versioning).

## Proposed Changes

- [Policy](https://github.com/oam-dev/kubevela/blob/master/design/vela-core/workflow_policy.md#proposal) system to replace Application Scope.
- [Workflow](https://github.com/oam-dev/kubevela/blob/master/design/vela-core/workflow_policy.md#cue-based-workflow-task) system to model modern deployment procedure (blue-green deployment, traffic splitting, multi-cloud rollout etc).

## Versioning

For the model objects, Open Application Model specification convention adopts [Kubernetes API resource convention](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/resource-management.md).

Releases of the specification are versioned according to [Semantic Versioning 2.0](https://semver.org/spec/v2.0.0.html) and described in [OAM release page](https://github.com/oam-dev/spec/releases). Implementations of the specification are required to specify which version they implement.

### Changes to the API objects

Changes to the API objects in this specification follows [on compatibility](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#on-compatibility) and [incompatible api changes](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md#incompatible-api-changes) defined in Kubernetes API resource convention. It doesn't couple with release version of the specification.

Changes to the change process (e.g. governance model, review/approve process etc) itself are not currently versioned but may be independently versioned in the future.
