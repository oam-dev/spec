# An Example Workflow

This section is non-normative. OAM compliant tooling need not implement this section.

This section uses a fictional tool called `oamctl` to illustrate a workflow pattern for OAM app operations.

### Deploying two components with parameter overrides
The following example shows two separate components:
 - a front-end web app in a container, run as a core containerized workload.
 - a back-end Cassandra database, represented by an extended workload.

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: Component
metadata:
  name: frontend
  annotations:
    version: v1.0.0
    description: "A simple webserver"
spec:
  workload:
    apiVersion: core.oam.dev/v1alpha2
    kind: ContainerizedWorkload
    metadata:
      name: sample-workload
    spec:
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
          value: default
  parameters:
  - name: message
    description: The message to display in the web app.  
    required: true
    type: string
    fieldPaths:
    - "spec.containers[0].env[0].value"
```

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: Component
metadata:
  name: backend
  annotations:
    version: v1.0.0
    description: "Cassandra database"
spec:
  workload:
    apiVersion: data.oam.dev/v1
    kind: Cassandra
    spec:
      maxStalenessPrefix: 100000
      defaultConsistencyLevel: Eventual
  parameters:
    - name: maxStalenessPrefix
      description: Max stale requests.
      type: int
      fieldPaths:
      - "spec.maxStalenessPrefix"
    - name: defaultConsistencyLevel
      description: The default consistency level
      type: string
      fieldPaths:
      - "spec.defaultConsistencyLevel"
```

Note that each component allows certain parameters to be overridden. For example, the `message` parameter is exposed for configuration in the frontend component. Within the component config, the parameter is piped to an environment variable where the component code can read the value.

Components are designed for reuse by exposing parameters to be configured in different deployment scenarios.

An application configuration combines any number of components and sets operational characteristics and configuration for a deployed _instance_ of each component.

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
      parameterValues:
        - name: message
          value: "Well hello there"

    - componentName: backend
```

In this example, the ops config allows a user to override only one of the parameters in the `frontend` component, and it does not allow the user to change any of the `backend` parameters. This allows the author of this ops config to control configuration options for the components therein.

The operator can now deploy the components together with this configuration to create running instances of the components:

```
$ oamctl install -c ./config.yaml ./frontend.yaml ./backend.yaml -p "message=overridden message!"
```

### Adding traits to the components
Traits are attached by application operators. In the next part of this example, traits are added to the two components defined previously.

Our system supports a few different traits:

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
kind: ApplicationConfiguration
metadata:
  name: custom-single-app
  annotations:
    version: v1.0.0
    description: "Customized version of single-app"
spec:
  components:
    - componentName: frontend
      parameterValues:
        - name: message
          value: "Well hello there"
      traits:
        - trait:
            apiVersion: standard.oam.dev/v1alpha1
            kind: Ingress
            spec:
              host: "www.example.com"
              path: "/"
```

This example attaches an `ingress` to route traffic from another network (public, perhaps) to the internal service. This configuration shows how the trait is both attached and configured.

An implementation of this would then direct inbound traffic bound for `www.example.com` to the `front-end` component.

### Placing components into application scopes

Up to this point, the frontend component is deployed into the default "root" network and health scopes, as all core workload types are required to be in all core application scope types.

The backend component is an extended workload type and is therefore not automatically deployed into the default "root' application scopes.

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

Both components are then added to the same network scope for direct network connectivity and a shared set of network policies that can be defined by the infrastructure operator on the SDN itself. The SDN name is parameterized in the application configuration to allow an application operator to deploy this configuration into any SDN.


```yaml
apiVersion: core.oam.dev/v1alpha2
kind: ApplicationConfiguration
metadata:
  name: custom-single-app
  annotations:
    version: v1.0.0
    description: "Customized version of single-app"
spec:
  scopes:
    - scopeRef:
        apiVersion: core.oam.dev/v1alpha2
        kind: NetworkScope
        name: my-vpc-network
  components:
    - componentName: frontend
      parameterValues:
        - name: message
          value: "Well hello there"
      traits:
        - trait:
            apiVersion: standard.oam.dev/v1alpha1
            kind: Ingress
            spec:
              host: "www.example.com"
              path: "/"
      scopes:
        - my-vpc-network

    - componentName: backend
      scopes:
        - my-vpc-network
```

This example now shows a complete installation of the application configuration composed of two components, an ingress trait, and a network scope with parameters exposed to an application operator to customize the domain name, the network to deploy it to, and a custom message for the web front end to display.
