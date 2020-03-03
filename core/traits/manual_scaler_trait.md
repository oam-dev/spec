# Manual Scaler

This trait definition allows operators to manually scale the number of replicas for workload types that allow scaling operations.

## Applicable Workloads

 - `core.oam.dev/v1alpha2.ContainerizedWorkload`

## Schematic

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `replicaCount` | `integer` | Y | `0` | The target number of replicas to create for a given component instance. |

## Spec

As a core trait, the Manual Scaler trait should be part of OAM implementation, so we don't need trait definition for it.

But assuming manual scaler trait is not a core trait, we could define as the following workflow.

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

Then OAM runtime should have a version of this schema, the schema SHOULD be able to validate. For example using OpenAPIv3 schema like this:

```yaml
openAPIV3Schema:
  type: object
  description: Open Application Model ManualScaler Trait OpenAPIv3 schema.
  required:
    - apiVersion
    - kind
    - spec
  properties:
    apiVersion:
      description: APIVersion defines the versioned schema of this representation of an object.
      type: string
      enmu:
        - core.oam.dev/v1alpha2
    kind:
      description: Must be ManualScalerTrait.
      type: string
      enum:
        - ManualScalerTrait
    spec:
      description: Defines the desired state of the ManualScaler trait.
      type: object
      required:
        - replicaCount
      properties:
        replicaCount:
          description: the target number of replicas to scale a component to.
          type: integer
```

Or using JSON Schema:

```json
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "title": "Open Application Model ManualScaler Trait JSON Schema",
    "type": "object",
    "properties": {
        "apiVersion": {
            "type": "string",
            "description": "APIVersion defines the versioned schema of this representation of an object.",
            "enum": ["core.oam.dev/v1alpha2"]
        },
        "kind": {
            "type": "string",
            "description": "must be ManualScalerTrait.",
            "enum": ["ManualScalerTrait"]
        },
        "spec": {
            "type": "object",
            "description": "Defines the desired state of the ManualScaler trait.",
            "required": ["replicaCount"],
            "properties": {
              "replicaCount": {
                "type": "integer",
                "description": "the target number of replicas to scale a component to.",
                "minimum": 0
              }
            }
        }
    },
    "required": ["apiVersion", "kind", "spec"]
}
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