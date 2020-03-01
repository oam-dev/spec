# Health scope

The health scope aggregates health states for components. Parameters of the health scope can be set to determine the percentage of components that must be unhealthy to consider the entire scope unhealthy.

The health scope on its own does not take any action based on health status. It is only a group health aggregator that can be queried and used by other processes and parts of an application, such as:

 - Application upgrade traits can monitor the aggregate health of a health scope and decide when to initiate an automatic rollback.
 - Monitoring applications can monitor the aggregate health of a health scope to issue alerts.

## Schematic

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `probe-method` | `string` | Y | | The method to probe the components, e.g. 'httpGet'. |
| `probe-endpoint` | `string` | Y | | The endpoint to probe from the components, e.g. '/v1/health'. |
| `probe-timeout` | `integer` | N | | The amount of time in seconds to wait when receiving a response before marked failure. |
| `probe-interval` | `integer` | N | | The amount of time in seconds between probing tries. |
| `failure-rate-threshold` | `double` | N | | If the rate of failure of total probe results is above this threshold, declared 'failed'. |
| `healthy-rate-threshold` | `double` | N | | If the rate of healthy of total probe results is above this threshold, declared 'healthy'. |
| `health-threshold-percentage` | `double` | N | | The % of healthy components required to upgrade scope. |
| `required-healthy-components` | `array` | N | | Comma-separated list of names of the components required to be healthy for the scope to be health. |

## Instance Example

For example, a health scope instance can be like below:

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: HealthScope
metadata:
  name: example-health-scope
spec:
  probe-method: GET
  probe-endpoint: /health
```
