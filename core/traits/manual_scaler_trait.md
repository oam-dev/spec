# Manual Scaler

This trait definition allows operators to manually scale the number of replicas for workload types that allow scaling operations.

## Applicable Workloads

 - `core.oam.dev/v1alpha2.ContainerizedWorkload`

## Schematic

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `replicaCount` | `integer` | Y | `0` | The target number of replicas to create for a given component instance. |

## Spec

As a core trait, we don't need any trait definition here. But we could give this as an example.

Firstly, we need register trait definition like below:

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: TraitDefinition
metadata:
  name: manualscalertrait.core.oam.dev
spec:
  appliesToWorkloads:
    - core.oam.dev/v1alpha2.ContainerizedWorkload
  definitionRef:
    name: manualscalertrait.core.oam.dev
```

Then OAM runtime should have a version of this schema, the schema SHOULD be able to validate. For example using JSON Schema like this:

```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object",
    "required": [
        "replicaCount"
    ],
    "properties": {
       "replicaCount": {
        "type": "integer",
        "description": "the target number of replicas to scale a component to.",
        "minimum": 0
       }
    }
}
```

Or using OpenAPI V3 Schema:

```yaml
openAPIV3Schema:
  type: object
  properties:
    replicaCount:
      description: the target number of replicas to scale a component to.
      type: integer
  required:
    - replicaCount
```

## Usage examples

The following snippet from an application configuration shows how the manual scaler trait is applied and configured for a component. In this example, it is assumed component `frontend` following workload of `core.oam.dev/v1alpha2.ContainerizedWorkload`.

```yaml
apiVersion: core.oam.dev/v1alpha2
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
      traits:
        - trait:
            apiVersion: core.oam.dev/v1alpha2
            kind: ManualScalerTrait
            spec:
              replicaCount: 5
```

This example illustrates using the manual scaler trait to manually scale a component by specifying the number of replicas the runtime should create. This trait has only one attribute in its properties: `replicaCount`. This takes an integer, and a value is required for this trait to be successfully applied. If, for example, an application configuration used this trait but did not provide the `replicaCount`, the system would reject the application configuration.