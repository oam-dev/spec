# Kubernetes Friendly OAM

* Authors: Nic Cope ([@negz], [negz@upbound.io]), Hongchao Deng ([@hongchaodeng], [hongchao.deng@alibaba-inc.com])
* Reviewers: [OAM Maintainers]

## Background

The [Open Application Model] (OAM) is a specification for building cloud native
applications. OAM aims to enable the [separation of concerns] between
application developers, application operators, and infrastructure operators.
[Crossplane] is an open source multicloud control plane that shares many of
OAM's goals around enabling the separation of concerns when modelling cloud
native applications.

While OAM and Crossplane share goals, they take different approaches. Crossplane
bets on the Kubernetes API server, leveraging it as its foundation. OAM is
_inspired_ by the Kubernetes API, but intends to be implementable by runtimes
that do not use Kubernetes at all. This runtime flexibility comes at a price to
Kubernetes-native implementations; the OAM spec is suggestively _Kubernetes
like_ but not particularly _idiomatic_ with regard to [Kubernetes API
conventions]. For example:

* Traits and Scopes, which define the schema for _parts of_ an ApplicationConfig
  resource. In Kubernetes a resource’s schema is defined by its
  [CustomResourceDefinition] (CRD).
* ComponentSchematics, which can be either a template for configuration values
  according to a fixed schema (the containers array), or define both schema and
  configuration values (the workloadSettings object).

Furthermore, the contemporary OAM specification cannot directly leverage the
many custom resources that are already defined in the Kubernetes ecosystem;
doing so requires that an OAM runtime understand how to parse an extended
workload corresponding to each custom resource.

Take, for example, a Crossplane _[resource claim]_:

```yaml
apiVersion: cache.crossplane.io/v1alpha1
kind: RedisCluster
metadata:
  name: app-redis
spec:
  classSelector:
    matchLabels:
      region: "us-east"
  writeConnectionSecretToRef:
    name: app-redis-connection-details
  engineVersion: "3.2"
```

The application of this RedisCluster resource claim to the API server causes
Crossplane to provision a Redis cluster using an appropriate infrastructure
provider. Resource claims are authored by application operators, like an OAM
ApplicationConfig. They model the need for infrastructure from an application’s
perspective; detailed configuration is defined by a _resource class_.

Ideally resource claims (and other Kubernetes custom resources like them) could
be first class citizens of OAM. Conversely, the contemporary OAM specification
imposes a layer of abstraction; an extended workload type. Crossplane (as a
tentative OAM runtime) would need to define an extended workload type
corresponding to the above resource claim:

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: ComponentSchematic
metadata:
  name: rediscluster
spec:
  workloadType: cache.crossplane.io/v1alpha1.RedisCluster
  parameters:
  - name: region
    type: string
    required: true
  - name: engineVersion
    type: string
  workloadSettings:
    - name: engineVersion
      type: string
      fromParam: region
    - name: region
      type: string
      fromParam: region
```

In addition to defining this extended workload type, the OAM runtime would need
to know how to render it, i.e. how to produce a RedisCluster custom resource
given a ComponentSchematic of the RedisCluster workload type. This rendering
process would presumably require the OAM runtime to contain logic for each
custom resource.

## Goals

OAM as a specification is heavily inspired by the Kubernetes API, but is runtime
agnostic. The purpose of this document is to explore whether the OAM
specification could be made friendlier to - idiomatic to and compatible with -
Kubernetes powered runtimes while remaining flexible enough to be usefully
implemented by other runtimes.

Kubernetes powered runtimes are the focus of this document because the OAM
specification already looks and feels a lot like the Kubernetes API. When it is
implemented using the _actual_ Kubernetes API any divergences from said API’s
conventions are an order of magnitude more likely to violate the [principle of
least astonishment] for users than for users of other runtimes.

Any proposal outlined by this document should ensure:

* The separation of concerns that is the core focus of the OAM specification.
* That the OAM specification “look and feel” idiomatic to the Kubernetes API
  model.
* That the OAM specification remains useful and compelling to other runtimes.

Note that this document frequently uses Crossplane as an example of a tentative
OAM runtime that would benefit from a Kubernetes friendly OAM specification, but
defining specifically how Crossplane will support OAM is out of scope.

## Proposal

This document proposes:

1. An ApplicationConfiguration _references_ distinct Component resources,
   grouping them into one logical application and providing inputs to their
   parameters.
1. Each Component embed an OAM “workload” under the `spec.workload` key.
   Available workloads are defined by WorkloadDefinitions. In Kubernetes
   runtimes any existing custom resource with a corresponding WorkloadDefinition
   can be used in a Component.
1. The set of available Traits and their schemas be defined via
   TraitDefinitions. Traits are applied to a component and configured by
   specifying their configuration inline in the ApplicationConfig as an
   "embedded resource".
1. The set of available Scopes and their schemas be defined via
   ScopeDefinitions. Scopes are referenced by an ApplicationConfiguration.

### Components and Workloads

Each Component is of a distinct workload _kind_. This is functionally equivalent
to the existing concept of workload _types_, but reuses the _kind_ term already
used within Kubernetes and OAM. The schema of a workload kind - including core
workload kinds like Server - are defined via a WorkloadDefinition. For example:

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: WorkloadDefinition
metadata:
  name: server.core.oam.dev
spec:
  definitionRef:
    name: server.core.oam.dev
```

The above WorkloadDefinition references a Kubernetes CustomResourceDefinition
named server.core.oam.dev, designating custom resources defined by said CRD to
be valid OAM workloads. OAM runtimes that are not powered by Kubernetes could
opt to either implement the CRD concept, or introduce a new field to the
WorkloadDefinition spec that defines a kind of workload and its schema.
Application operators can discover available OAM workload kinds by running
`kubectl get workloaddefinitions`. Workloads would most frequently be defined by
OAM runtimes or infrastructure operators.

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: Component
metadata:
  name: example-server
spec:
  workload:
    apiVersion: core.oam.dev/v1alpha1
    kind: Server
    spec:
      osType: linux
      containers:
      - name: my-cool-server
        image:
          name: example/very-cool-server:1.0.0
          digest: sha256:verytrustworthyhash
        resources:
          cpu:
            required: 1.0
          memory:
            required: 100MB
        ports:
        - name: http
          value: 8080
        env:
        - name: CACHE_SECRET
        livenessProbe:
          httpGet:
            port: 8080
            path: /healthz
        readinessProbe:
          httpGet:
            port: 8080
            path: /healthz
  parameters:
  - name: instanceName
    required: true
    fieldPaths:
    - "metadata.name"
  - name: cacheSecret
    required: true
    fieldPaths:
    - "spec.containers[0].env[0].value"
```

Components would most frequently be defined by application developers. A
Component has two responsibilities:

1. Embed a valid OAM workload kind under its `spec.workload` field.
2. Publish a set of parameters under its `spec.parameters` field. Parameters may
   be set by an ApplicationConfig. Their values overlay onto fields in the
   embedded OAM workload.

The embedded OAM workload is a template of a sorts - though it is important to
note that it is a first class citizen of the configuration language. It is
specified using YAML (or JSON, etc), not as an opaque string. OAM runtimes can
validate its schema against its WorkloadDefinition. Kubernetes runtimes may
consider the workload an [embedded object], like a polymorphic podTemplateSpec.

OAM runtimes would use the set of parameters published by a Component to mutate
the embedded workload based on inputs supplied by the ApplicationConfiguration.
Note that the relationship between parameters and workload fields is inverted
compared to the contemporary OAM spec. Each parameter includes one or more JSON
field paths specifying that its value should be injected at those paths. This
[existing] [Kubernetes paradigm] ensures the parameters supported by a component
are easily discoverable, and allows the OAM runtime to inject parameters into
any field of the embedded workload without introducing a [“fromParams()” inline
function DSL]. This approach also allows the published parameters to omit their
type - the type can be inferred from the type of the workload fields a parameter
corresponds to.

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: WorkloadDefinition
metadata:
  name: rediscluster.cache.crossplane.io
spec:
  definitionRef:
    name: rediscluster.cache.crossplane.io
```

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: Component
metadata:
  name: example-cache
spec:
  workload:
    apiVersion: cache.crossplane.io/v1alpha1
    kind: RedisCluster
    spec:
      engineVersion: "3.2"
  parameters:
  - name: instanceName
    required: true
    fieldPaths:
    - ".metadata.name"
  - name: secret
    required: true
    fieldPaths:
    - "spec.writeConnectionSecretToRef.name"
  - name: engineVersion
    required: false
    fieldPaths:
    - "spec.engineVersion"
```

The above WorkloadDefinition and Component leverages the existing Crossplane
RedisCluster resource claim. Crossplane as a tentative OAM runtime need not
define an extended workload type; it need only create a WorkloadDefinition
pointing to [the existing CustomResourceDefinition] of the RedisCluster resource
claim.

The Component design enables OAM runtimes to break the task of reconciling the
desired state of an application with the actual state of the world into two
distinct stages:

1. Render workloads. The OAM runtime iterates over the Components referenced by
   an ApplicationConfiguration, instantiating a distinct workload resource for
   each component. In the above examples this would involve creating a distinct
   Server resource, and a distinct RedisCluster resource.
2. Reconcile workloads. By producing valid instances of existing Kubernetes
   custom resources OAM is able to leverage existing controllers. For example
   the existing Crossplane RedisClaim controller(s) would be responsible for
   reconciling the RedisCluster custom resource produced during the render
   phase.

Note that this two phase reconciliation and the production of distinct workload
resources would not be required by the OAM specification. Runtimes could choose
to iterate over the referenced components, render them in memory, and act upon
the rendered components directly.

### Traits

Like workloads, each trait is of a distinct kind. A TraitDefinition defines the
schema of each kind of trait, and limits what workload kinds it applies to:

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: TraitDefinition
metadata:
  name: manualscalertrait.core.oam.dev
spec:
  appliesToWorkloads:
    - server.core.oam.dev/v1alpha1
    - worker.core.oam.dev/v1alpha1
    - task.core.oam.dev/v1alpha1
  definitionRef:
    name: manualscalertrait.core.oam.dev
```

The above TraitDefinition references a Kubernetes CustomResourceDefinition named
manualscalertrait.core.oam.dev, designating custom resources defined by said CRD
to be valid OAM traits. As with WorkloadDefinitions, OAM runtimes that are not
powered by Kubernetes could opt to either implement the CRD concept, or
introduce a new field to the TraitDefinition spec that defines a kind of trait
and its schema. Application operators can discover available OAM trait kinds by
running `kubectl get traitdefinitions`. Traits would most frequently be defined
by OAM runtimes or infrastructure operators.

This document proposes that trait configuration be applied by be embedding one
or more of a particular kind of trait under the `traits` array of the component
it should apply to. For example:

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: ApplicationConfiguration
metadata:
  name: cool-example
spec:
  components:
  - componentName: example-server
    traits:
    - trait:
        apiVersion: core.oam.dev/v1alpha1
        kind: ManualScalerTrait
        spec:
          replicaCount: 3
```

Note that the embedded trait resource supports Kubernetes-style object metadata,
but it may be omitted when authoring an ApplicationConfiguration. Metadata such
as names and labels may be filled automatically by the OAM runtime if a distinct
instance of a trait configuration is created as part of processing an
ApplicationConfiguration. As with workloads, Kubernetes runtimes may treat
traits as an [embedded object]. Each trait resource is embedded beneath the key
`trait`. This allows the spec to add other keys alongside `trait` in future
without breaking compatibility.

### Scopes

This document proposes that application scopes, while maintaining their current
purpose of grouping application components, function identically to traits from
an API perspective. Available kinds of scope are defined via a ScopeDefinition,
which also specifies whether a component may exist in multiple scopes of a
particular kind:

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: ScopeDefinition
metadata:
  name: networkscope.core.oam.dev
spec:
  allowComponentOverlap: true
  definitionRef:
    name: networkscope.core.oam.dev
```

The above ScopeDefinition references a Kubernetes CustomResourceDefinition named
networkscope.core.oam.dev, designating custom resources defined by said CRD to
be valid OAM scopes. As with the other OAM definition resources OAM runtimes
that are not powered by Kubernetes could opt to either implement the CRD
concept, or introduce a new field to the ScopeDefinition spec that defines a
kind of scope and its schema. Application operators can discover available OAM
scope kinds by running `kubectl get scopedefinitions`. Scopes would most
frequently be defined by OAM runtimes or infrastructure operators.

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: NetworkScope
metadata:
  name: example-vpc-network
  labels:
    region: us-west
    environment: production
spec:
  networkId: cool-vpc-network
  subnetIds:
  - cool-subnetwork
  - cooler-subnetwork
  - coolest-subnetwork
  internetGatewayType: nat
```

Scopes would be configured by creating an instance of configuration - a scope
resource that may be referenced by ApplicationConfigurations. This is not
functionally dissimilar to the contemporary OAM specification, which uses an
ApplicationConfiguration to create an instance of scope configuration that is
referenced by other ApplicationConfigurations.

### Application Configuration

Finally, the ApplicationConfiguration resource ties together a set of
components, traits, and scopes as it does today:

```yaml
apiVersion: core.oam.dev/v1alpha1
kind: ApplicationConfiguration
metadata:
  name: cool-example
spec:
  components:
  - componentName: example-server
    parameterValues:
    - name: instanceName
      value: cool-example
    - name: cacheSecret
      value: cache-connection
    traits:
    - trait:
        apiVersion: core.oam.dev/v1alpha1
        kind: ManualScalerTrait
        spec:
          replicaCount: 3
    scopes:
    - scopeRef:
        apiVersion: core.oam.dev/v1alpha1
        kind: NetworkScope
        name: example-vpc-network
  - componentName: example-cache
    parameterValues:
    - name: instanceName
      value: cool-example
    - name: engineVersion
      value: "4.0"
    - name: secret
      value: cache-connection
```

The above ApplicationConfiguration creates an application consisting of the
Server and RedisCluster components from the [previous examples]. The Server is
modified by the ManualScalerTrait, also [described previously]. This document
proposes the following changes to the contemporary ApplicationConfiguration:

* Trait configuration is specified via an inline, embedded resource.
* Scopes are referenced by a full object reference (with kind and version).
* Scope configuration is instantiated in a Scope, not in an ApplicationConfig.
* “instanceName” is demoted from a first class component field to a parameter.

It may be preferable for instanceName to remain a first class field of component
(it will likely always be required). It is demoted to a parameter in this
proposal to allow it to be overlaid upon a component’s embedded workload using
the same patterns and language as any other parameter, for example to allow the
instance name to be used in additional fields besides the embedded workload’s
`.metadata.name`.

## Alternatives Considered

The following related proposals preceded this document:

* [Crossplane as an OAM runtime]
* [Resource Composition Proposal in OAM]
* [OAM-K8s Solution]

[@negz]: https://github.com/negz
[negz@upbound.io]: mailto:negz@upbound.io
[@hongchaodeng]: https://github.com/hongchaodeng
[hongchao.deng@alibaba-inc.com]: mailto:hongchao.deng@alibaba-inc.com
[OAM Maintainers]: https://github.com/oam-dev/spec/blob/d16d5add5a725d28c587ba5d624350feb6713218/OWNERS.md
[Open Application Model]: https://oam.dev/
[separation of concerns]: https://github.com/oam-dev/spec/blob/d16d5add/introduction.md
[Crossplane]: https://crossplane.io
[Kubernetes API conventions]: https://github.com/kubernetes/community/blob/6850d0/contributors/devel/sig-architecture/api-conventions.md
[CustomResourceDefinition]: https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/
[resource claim]: https://crossplane.io/docs/v0.7/concepts.html#resource-claims-and-resource-classes
[principle of least astonishment]: https://en.wikipedia.org/wiki/Principle_of_least_astonishment
[embedded object]: https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/#rawextension
[existing]: https://github.com/kubernetes-sigs/kustomize/blob/03cc4e/docs/fields.md#vars
[Kubernetes paradigm]: https://kubernetes.io/docs/tasks/inject-data-application/environment-variable-expose-pod-information/#use-pod-fields-as-values-for-environment-variables
[“fromParams” inline function DSL]: https://docs.google.com/document/d/1ylo-ZwjCRWw-vesMt4wueDHojiDQyf-p51RkdRU4m_o/edit#heading=h.gp0go28b7a85
[the existing CustomResourceDefinition]: https://github.com/crossplaneio/crossplane/blob/ce8aced3d/cluster/charts/crossplane/templates/crds/cache.crossplane.io_redisclusters.yaml
[previous examples]: #components-and-workloads.
[described previously]: #traits
[Crossplane as an OAM runtime]: https://github.com/crossplaneio/crossplane/pull/1132
[Resource Composition Proposal in OAM]: https://docs.google.com/document/d/1ylo-ZwjCRWw-vesMt4wueDHojiDQyf-p51RkdRU4m_o/edit#heading=h.hh69i79bk4p3
[OAM-K8s Solution]: https://docs.google.com/document/d/1HFa22qWfMgt8j80cYD3ULYt6JV5KEA9TkSTn3n-96Dc/edit
