# Data Placement scope

In responding to application specific latency requirement, an application can specify the data placement policy for components. Parameters of the data placement scope can be set to: 'in-'cloud' or 'in-edge' for placement policy, 'RW' (read/write) or 'RO' (read only) for access mode, and 'shared' or 'exclusive' for sharing policy.

For those data defined 'in-cloud', the data will stay in Cloud and will not migrate to the Edge where is near users.

For those data defined 'in-edge', it can be further defined for the attributes of access mode and sharing policy. For the data defined as 'RW' (read/write) and 'shared', the data has multiple instances among multiple edge sites, state should be maintained consistence among these instances.

For those data defined 'in-edge', 'RW' (read/write) and 'exclusive', the data will have an instance in Edge near users. It is also possible to have multiple instances for the same data entity spread among multiple edge sites. We need to define the behavior of data migration according to storage usage and data expiration policy. It will be further defined whether these multiple instances should be synchronized back and saved to Cloud or not.

For those data defined 'in-edge', 'RO' (read only), the data will have an instance in Edge near users if exclusive. It is also possible to have multiple instances for the same data entity spread among multiple edge sites if shared. We need to define the behavior of data migration according to storage usage and data expiration policy.

In Open Application Model, we can define it as 'DataPlacement' scope. Users can bind 'DataPlacement' to different Components in their application. The implementation of DataPlacement will handle the data migration detail based on the Scope kind.

## Sample User Scenario

An application may connected by a 5G mobile phone through 5G wireless connection by carrier A and a desktop PC through wired connection by carrier B. For those data components in this application without or with loose latency requirement, the components can be bound with 'in-cloud' attribute of 'DataPlacement' scope. For those data components in this application with low latency requirement, the components can be bound with 'in-edge' attribute of 'DataPlacement' scope. If the components are read-only, they can be bound with 'in-edge' & 'RO' attribute of 'DataPlacement' scope and duplicated among multiple edge sites without causing any inconsistent issue. If the components are not read-only, they can be bound with 'in-edge' & 'RW' attribute of 'DataPlacement' scope. 'exclusive' attribute of 'DataPlacement' scope can be set further to prevent multiple access the components simultaneously. The underlying application operation should solve the issue of data migration from another edge site if necessary. 'Shared' attribute of 'DataPlacement' scope can be set further if simultaneously multiple access of the components is necessary. The underlying operation platform should solve the issue of data sharing among multiple edge sites.

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
