# Data Placement scope

In responding to application specific latency requirement, an application can specify the data placement policy for components. Parameters of the data placement scope can be set to: 'in-'cloud' or 'in-edge' for placement policy, 'RW' (read/write) or 'RO' (read only) for access mode, and 'shared' or 'exclusive' for sharing policy.

For those data defined 'in-cloud', the data will stay in Cloud and will not migrate to the Edge where is near users.

For those data defined 'in-edge', it can be further defined for the attributes of access mode and sharing policy. For the data defined as 'RW' (read/write) and 'shared', the data has multiple instances among multiple edge sites, state should be maintained consistence among these instances.

For those data defined 'in-edge', 'RW' (read/write) and 'exclusive', the data will have an instance in Edge near users. It is also possible to have multiple instances for the same data entity spread among multiple edge sites. We need to define the behavior of data migration according to storage usage and data expiration policy. It will be further defined whether these multiple instances should be synchronized back and saved to Cloud or not.

For those data defined 'in-edge', 'RO' (read only), the data will have an instance in Edge near users if exclusive. It is also possible to have multiple instances for the same data entity spread among multiple edge sites if shared. We need to define the behavior of data migration according to storage usage and data expiration policy.

In Open Application Model, we can define it as 'DataPlacement' scope. Users can bind 'DataPlacement' to different Components in their application. The implementation of DataPlacement will handle the data migration detail based on the Scope kind.

## Sample User Scenario

In mixed reality application, users usually touch and manipulate the holograms of targeted objects rendered in the vision of real world. The digital twins of the targeted objects can be specified as components bound with 'in-edge' attribute of 'DataPlacement' scope in consideration of the requirement of low latency. The template part of digital twins can be further specified with 'RO' attribute to reflect the fact of templates cannot be modified. On the other hand, the configuration part of digital twins can be specified with 'RW' and 'exclusive' in single user mode. In multi-user mode, The configuration part of digital twins can be specified with 'RW' and 'shared'.

## Schematic

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `placement-policy` | `string` | Y | | Possible values include:<ul><li>in-cloud</li><li>in-edge</li></ui> |
| `accessMode` | `string` |	N	| `RW` | Specifies the access mode. Allowed values are RW (read/write) and RO (read-only).
| `sharingPolicy` |	`string` | N | `Exclusive` | The sharing policy for the data, indicating if it is expected to be shared or not. Allowed values are Exclusive and Shared.

## Instance Example

For example, a data placement scope instance can be like below:

```yaml
apiVersion: extend.oam.dev/v1alpha2
kind: DataPlacementScope
metadata:
  name: example-data-placement-scope
spec:
  placement-policy: 'in-edge'
```
