# Core Application Scopes

The following core application scopes are defined by this specification:

| Name | Type | Description |
|-----------|------|----------|
|[Network](#network)|core.hydra.io/v1alpha1.Network | This scope groups components into a network subnet boundaries and defines the general runtime networking model. Network definitions, rules, and policies are described by the infrastructure's network or SDN.
|[Health](#health)|core.hydra.io/v1alpha1.Health | This scope groups components into an aggregate health group. The aggregate health of the constituent components within the group supplies information to upgrade and rollback mechanisms.  |

## Network

Application scope name: `core.hydra.io/v1alpha1.Network`

The network scope groups components together and links them to a network or SDN. The network itself must be defined and operated by the infrastructure.

The network scope may also be queried by traffic management traits to determine discoverability boundaries for a service mesh or API boundaries for API gateways.

If no network scope is assigned, the platform MUST join the application to a default network. Within that default network, all components in the application configuration MUST be able to communicate to each other, and health probes MUST be able to contact any components who define health checking rules. The network joined, however, is platform-dependent. For example, cluster-based environments (such as Kubernetes) declare cluster-wide networks. Conversely, a truly serverless implementation may join the components to a network that only includes the components and the health checking probes.

The network scope is defined by the following spec:

```yaml
apiVersion: core.hydra.io/v1alpha1
kind: ApplicationScope
metadata:
  name: network
  annotations:
    version: v1.0.0
    description: "network boundary that a group components reside in"
spec:
  type: core.hydra.io/v1.NetworkScope
  allowComponentOverlap: false
  parameters:
    - name: network-id
      description: The id of the network, e.g. vpc-id, VNet name.
      type: string
      required: Y
    - name: subnet-id
      description: The id of the subnet within the network.
      type: string
      required: Y
    - name: internet-gateway-type
      description: The type of the gateway, options are 'public', 'nat'. Empty string means no gateway.
      type: string
      required: N
```

## Health

Application scope name: `core.hydra.io/v1alpha1.Health`

The health scope aggregates health states for components. Parameters of the health scope can be set to determine the percentage of components that must be unhealthy to consider the entire scope unhealthy.

The health scope on its own does not take any action based on health status. It is only a group health aggregator that can be queried and used by other processes and parts of an application, such as:

- Application upgrade traits can monitor the aggregate health of a health scope and decide when to initiate an automatic rollback.
- Monitoring applications can monitor the aggregate health of a health scope to issue alerts.

The health scope is defined by the following spec:

```yaml
apiVersion: core.hydra.io/v1alpha1
kind: ApplicationScope
metadata:
  name: health
  annotations:
    version: v1.0.0
    description: "aggregated health state for a group of components."
spec:
  type: core.hydra.io/v1alpha1.HealthScope
  allowComponentOverlap: true
  parameters:
    - name: probe-method
      description: The method to probe the components, e.g. 'httpGet'.
      type: string
      required: true
    - name: probe-endpoint
      description: The endpoint to probe from the components, e.g. '/v1/health'.
      type: string
      required: true
    - name: probe-timeout
      description: The amount of time in seconds to wait when receiving a response before marked failure.
      type: integer
      required: false
    - name: probe-interval
      description: The amount of time in seconds between probing tries.
      type: integer
      required: false
    - name: failure-rate-threshold
      description: If the rate of failure of total probe results is above this threshold, declared 'failed'.
      type: double
      required: false
    - name: healthy-rate-threshold
      description: If the rate of healthy of total probe results is above this threshold, declared 'healthy'.
      type: double
      required: false
    - name: healthThresholdPercentage
      description: The % of healthy components required to upgrade scope
      type: double
      required: false
    - name: requiredHealthyComponents
      description: Comma-separated list of names of the components required to be healthy for the scope to be health.
      type: string
      required: false
```
