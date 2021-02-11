# An Example Workflow

This section is non-normative. OAM compliant tooling need not implement this section.

This section uses a fictional tool called `oamctl` to illustrate a workflow pattern for OAM app operations.

## Install two workload types

The following example shows two separate components:
 - a `webserver` web app in a container, run as a core containerized workload.
 - a `cassandra` Cassandra database, represented by an extended workload.

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: WorkloadDefinition
metadata:
  name: webserver
  annotations:
    version: v1.0.0
    description: "A simple webserver"
spec:
  template:
    ...
```

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: WorkloadDefinition
metadata:
  name: cassandra
  annotations:
    version: v1.0.0
    description: "Cassandra database"
spec:
  template:
    ...
```

An application combines any number of components and sets operational characteristics and configuration for a deployed _instance_ of each component.

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: Application
metadata:
  name: custom-single-app
  annotations:
    version: v1.0.0
    description: "Customized version of single-app"
spec:
  components:
    - name: frontend
      type: webserver
      settings:
        osType: linux
        containers:
        - name: web
          image: example/charybdis-single:latest@@sha256:verytrustworthyhash
          resources:
            cpu:
              required: 1.0
            memory:
              required: 100MB
          env:
          - name: MESSAGE
            value: overridden message
    - name: backend
      type: cassandra
      settings:
        maxStalenessPrefix: 100000
        defaultConsistencyLevel: Eventual
```

The operator can now deploy the workload definitions together with this application to create running instances of the components:

```output
$ oamctl install -c ./app.yaml ./webserver.yaml ./cassandra.yaml
```

## Adding traits to the components

Traits are attached by application operators. In the next part of this example, traits are added to the two components defined previously.

Our system supports a few different traits (pre-installed by `TraitDefinition`):

```console
$ oamctl trait-list
╔════════════╤═════════╤═════════════════════════════════════════════╗
║ NAME       │ VERSION │ PRIMITIVES                                  ║
╟────────────┼─────────┼─────────────────────────────────────────────╢
║ autoscaler │ 0.1.0   │ Server, Worker                              ║
╟────────────┼─────────┼─────────────────────────────────────────────╢
║ ingress    │ 0.1.0   │ SingletonServer, Server                     ║
╚════════════╧═════════╧═════════════════════════════════════════════╝
```

Here is how the previous application configuration looks with an `Ingress` trait attached to the front-end component:

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: Application
metadata:
  name: custom-single-app
  annotations:
    version: v1.0.0
    description: "Customized version of single-app"
spec:
  components:
    - component
      - name: frontend
        type: webserver
        settings:
          osType: linux
          containers:
          - name: web
            image: example/charybdis-single:latest@@sha256:verytrustworthyhash
            resources:
              cpu:
                required: 1.0
              memory:
                required: 100MB
            env:
            - name: MESSAGE
              value: overridden message
        traits:
          - name: ingress
            properties:
              host: "www.example.com"
              path: "/"
```

This example attaches an `ingress` to route traffic from another network (public, perhaps) to the internal service. This configuration shows how the trait is both attached and configured.

An implementation of this would then direct inbound traffic bound for `www.example.com` to the `frontend` component.

### Placing components into application scopes

Up to this point, no application scopes group the frontend component and backend component.

In this example, a [network scope](5.application_scopes.md#network-scope) is defined and attached to an SDN. Application Scope instance is created following it's [`ScopeDefinition`](5.application_scopes.md):

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: NetworkScope
metadata:
  name: my-vpc-network
  labels:
    region: us-west
    environment: production
spec:
  networkId:  my-vpc
  subnetIds:
  - my-subnet1
  - my-subnet2
  internetGatewayType: nat
```

Both components are then added to the same network scope for direct network connectivity and a shared set of network policies that can be defined by the infrastructure operator on the SDN itself.


```yaml
apiVersion: core.oam.dev/v1alpha2
kind: Application
metadata:
  name: custom-single-app
  annotations:
    version: v1.0.0
    description: "Customized version of single-app"
spec:
  components:
    - component
      - name: frontend
        type: webserver
        settings:
          osType: linux
          containers:
          - name: web
            image: example/charybdis-single:latest@@sha256:verytrustworthyhash
            resources:
              cpu:
                required: 1.0
              memory:
                required: 100MB
            env:
            - name: MESSAGE
              value: overridden message
      traits:
        - name: ingress
          properties:
            host: "www.example.com"
            path: "/"
      scopes:
        - scopeRef:
            apiVersion: core.oam.dev/v1alpha2
            kind: NetworkScope
            name: my-vpc-network
    - name: backend
      type: cassandra
      settings:
        maxStalenessPrefix: 100000
        defaultConsistencyLevel: Eventual
```

This example now shows a complete installation of the application configuration composed of two components, an ingress trait, and a network scope.
