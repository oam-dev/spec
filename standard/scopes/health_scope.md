# Health scope

The health scope aggregates health states for components. Parameters of the health scope can be set to determine the percentage of components that must be unhealthy to consider the entire scope unhealthy.

The health scope on its own does not take any action based on health status. It is only a group health aggregator that can be queried and used by other processes and parts of an application, such as:

Application upgrade traits can monitor the aggregate health of a health scope and decide when to initiate an automatic rollback.
Monitoring applications can monitor the aggregate health of a health scope to issue alerts.

## Properties

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `probe-timeout` | `int32` | Y | | ProbeTimeout is the amount of time in seconds to wait when receiving a response before marked failure.|
| `probe-interval` | `int32` | Y | | ProbeInterval is the amount of time in seconds between probing tries. |

## Instance Example

For example, a health scope instance can be like below:

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: HealthScope
metadata:
  name: example-health-scope
spec:
  probe-timeout: 5
  probe-interval: 5
```

## Usage Example

An application could reference above scope instance as below:

```yaml
kind: Application
metadata:
  name: example-appconfig
spec:
  components:
    - componentName: example-component
      traits:
        ...
      scopes:
        healthscopes.core.oam.dev: example-health-scope
```
