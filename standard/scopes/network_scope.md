# Network scope

The network scope groups components together and links them to a network or SDN. The network itself must be defined and operated by the application operator.

The network scope may also be queried by traffic management traits to determine discoverability boundaries for a service mesh or API boundaries for API gateways.

If no network scope is assigned, the platform MUST join the application to a default network. Within that default network, all components in the application configuration MUST be able to communicate to each other, and health probes MUST be able to contact any components who define health checking rules. The network joined, however, is platform-dependent. For example, cluster-based environments (such as Kubernetes) declare cluster-wide networks. Conversely, a truly serverless implementation may join the components to a network that only includes the components and the health checking probes.

## Schematic

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `networkId` | `string` | Y | | The id of the network, e.g. vpc-id, VNet name. |
| `subnetIds` | `array` | Y | | A comma separated list of IDs of the subnets within the network. For example, 'vsw-123' or 'vsw-123','vsw-456'. There could be more than one subnet because there is a limit in the number of IPs in a subnet. If IPs are taken up, operators need to add another subnet into this network. |
| `internetGatewayType` | `string` | Y | | The type of the gateway, options are 'public', 'nat'. Empty string means no gateway. |

## Spec

As a core scope, the spec of network scope has already defined. So we don't need any scope definition for this.

## Instance Example

For example, a network scope instance can be like below:

```yaml
apiVersion: standard.oam.dev/v1alpha2
kind: NetworkScope
metadata:
  name: example-vpc-network
  labels:
    region: us-west
    environment: production
spec:
  networkId: cool-vpc-network
  subnetIds:
  - cool-subnetwork
  - cooler-subnetwork
  - coolest-subnetwork
  internetGatewayType: nat
```
