# Server

A Server is a OAM core workload type to define long-running, scalable workloads that have a network endpoint with a stable name to receive network traffic for the component as a whole. Common use cases include web applications and services that expose APIs. The Server workload type has the following characteristics:

- Defines a container runtime where zero or more replicas of the same container may be run simultaneously.
- An application operator can increase or decrease the number of component replicas by applying and configuring traits when available.
- A Server is daemonized. A runtime MUST attempt to restart replicas that exit regardless of error code.
- A Server has a network endpoint with an automatically assigned virtual IP address (VIP) and DNS name addressable within the network scope to which the component belongs.

> If a Server does not provide at least one port on at least one container, implementations SHOULD emit an error.

## How to use?

1. Install below workload definition in your OAM based platform:

    ```yaml
    apiVersion: core.oam.dev/v1beta1
    kind: WorkloadDefinition
    metadata:
      name: Server
    spec:
      definitionRef:
        name: containerizedworkloads.core.oam.dev # the reference of schema for this workload type. In Kubernetes it should be a full name of API resource
    ```

    > Note: this workload references [ContainerizedWorkload](schema/containerized_workload.md) as schema.

2. Define a component references `Server` as workloads:

    ```yaml
    apiVersion: core.oam.dev/v1beta1
    kind: ComponentDefinition
    metadata:
      name: webserver
      annotations:
        definition.oam.dev/description: "webserver is a combo of Deployment + Service"
    spec:
      workload:
        type: Server # reference above workload definition by name.
      schematic:
        ... # CUE code to define user schematic.
    ```
