# Core Traits

The following core traits are defined by this specification:

| Name | Type | Description |
|-----------|------|----------|
| [Manual Scaler](#manual-scaler) | core.hydra.io/v1alpha1.ManualScaler | This trait definition allows operators to manually scale the number of replicas for workload types that allow scaling operations. |

## Manual Scaler

This trait definition allows operators to manually scale the number of replicas for workload types that allow scaling operations.

### Applicable workload types

- `core.hydra.io/v1alpha1.Service`
- `core.hydra.io/v1alpha1.Worker`
- `core.hydra.io/v1alpha1.Task`

### Properties

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `replicaCount` | `integer` | Y | `0` | The target number of replicas to create for a given component instance. |

### Spec

```yaml
apiVersion: core.hydra.io/v1alpha1
kind: Trait
metadata:
  name: ManualScaler
  annotations:
    version: v1.0.0
    description: "Allow operators to manually scale a workloads that allow multiple replicas."
spec:
  appliesTo:
    - core.hydra.io/v1alpha1.Service
    - core.hydra.io/v1alpha1.Worker
    - core.hydra.io/v1alpha1.Task
  properties:
    type: object
    properties:
      replicaCount:
        description: the target number of replicas to scale a component to.
        type: integer
        minimum: 0
      required:
        - replicaCount
```

### Usage examples

The following snippet from an application configuration shows how the manual scaler trait is applied and configured for a component. In this example, it is assumed component `frontend` has a workload type of `core.hydra.io/v1alpha1.Service`.

```yaml
apiVersion: core.hydra.io/v1alpha1
kind: ApplicationConfiguration
metadata:
  name: custom-single-app
  annotations:
    version: v1.0.0
    description: "Customized version of single-app"
spec:
  variables:
  components:
    - componentName: frontend
      instanceName: web-front-end
      parameterValues:
      traits:
        - name: core.hydra.io/v1alpha1.ManualScaler
          properties:
            replicaCount: 5
```

This example illustrates using the manual scaler trait to manually scale a component by specifying the number of replicas the runtime should create. This trait has only one attribute in its properties: `replicaCount`. This takes an integer, and a value is required for this trait to be successfully applied. If, for example, an application configuration used this trait but did not provide the `replicaCount`, the system would reject the application configuration.
