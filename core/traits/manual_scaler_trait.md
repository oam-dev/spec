# Manual Scaler

This trait definition allows operators to manually scale the number of replicas for workload types that allow scaling operations.

## Applicable Workload Types

 - Web Service
 - Server
 - Worker
 - `workload.oam.dev/replicable=true`

## Properties

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `replicas` | `integer` | Y | `0` | The target number of replicas to create for a given component instance. |

## Spec

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: TraitDefinition
metadata:
  annotations:
    definition.oam.dev/description: "Manually scale the app"
  name: scaler
spec:
  appliesToWorkloads:
    - webservice
    - worker
  extension:
    template: |-
      patch: {
         spec: replicas: parameter.replicas
      }
      parameter: {
        //+short=r
        replicas: *1 | int
      }
```

This `TraitDefinition` uses [CUE](https://github.com/cuelang/cue) as templating tool and exposes one configurable properties (i.e. `replicas` and `cmd`) to end users. Note that OAM doesn't have enforcement on the specific tool for templating.

## Usage examples

The following snippet from an application configuration shows how the manual scaler trait is applied and configured for a component.

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: ApplicationConfiguration
metadata:
  name: custom-single-app
spec:
  components:
    - componentName: frontend
      traits:
        - name: scaler
          properties:
            replicas: 10
```
