# AppScope Configuration

This section describes how application scope can be set up using app-scope configuration.

An app scope needs to be set up before used in the deployment of components. To set up an app scope, we just deploy an app-scope configuration.

An app-scope configuration (abbr. `scope config`) is managed by the _app operator_ role, and provides information specific to the current set-up of an app scope.

## Defining an AppScope Configuration

### Top-Level Attributes

The top-level attributes of an app-scope config define its apiVersion, kind, metadata, and spec.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `apiVersion` | `string` | Y || The API version of the specification in use. At present only `core.hydra.io/v1alpha1` is defined. |
| `kind` | `string` | Y || The string `AppScopeConfiguration` |
| `metadata` | [`Metadata`](#metadata) | Y | | AppScope configuration metadata. |
| `spec`| [`Spec`](#spec) | Y || A container for exposing configuration attributes. |

### Metadata

Metadata describes this trait. The metadata section is defined in [Section 3](3.component_model.md#metadata).


### Spec
The specification follows the structure as below:

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `appScopeType` | `string` | Y | | The type of the app-scope to create an instance of. |
| `properties` | [`Properties`](#properties) | N | | The properties attached to this scope. |

### Properties

`properties` is an object whose structure is determined by the scope property schema. It may be a simple value, or it may be a complex object. Properties are validated against the appropriate schema  for the scope. For example, if a scope defines a schema for properties that requires an array of integers with at least one member, the `properties` object is expected to be an array of integers with at least one member. During validation of the `properties` object, the scope's property schema will be applied.

If no schema is supplied, a property will be passed to the scope runtime without structural validation. As long as it parses as YAML or JSON, it will be considered valid.

## Example
The following is an example of a complete YAML file that illustrates the configurational elements above:

```yaml
apiVersion: core.hydra.io/v1alpha1
kind: AppScopeConfiguration
metadata:
  name: public-vpc
spec:
  appScopeType: core.hydra.io/v1alpha1.Network
  properties:
  - name: network-id
    value: myNetwork
  - name: subnet-id
    value: public-subnet
  - name: gateway-type
    value: public

---
apiVersion: core.hydra.io/v1alpha1
kind: AppScopeConfiguration
metadata:
  name: private-vpc
spec:
  appScopeType: core.hydra.io/v1alpha1.Network
  properties:
  - name: network-id
    value: myNetwork
  - name: subnet-id
    value: private-subnet
  - name: gateway-type
    value: nat
```
