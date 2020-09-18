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
	apiVersion: core.oam.dev/v1alpha2
	kind: WorkloadDefinition
	metadata:
	  name: Server # the workload type
	spec:
	  definitionRef:
	    name: containerizedworkloads.core.oam.dev # the reference of schema for this workload type. In Kubernetes it should be a full name of API resource
	```

	As shown on the example above, [ContainerizedWorkload](schema/containerized_workload.md) is used as the schema for Server workload type.

2. Define a component reference `Server` as workload:

	```yaml
	apiVersion: core.oam.dev/v1alpha2
	kind: Component
	metadata:
	  name: frontend
	spec:
	  workload:
	    type: Server # <--- type of this workload
	    spec:        # <--- spec template of this workload
	      osType: linux
	      containers:
	      - name: my-cool-workload
	        image: example/very-cool-workload:0.1.2@sha256:verytrustworthyhash
	        resources:
	          cpu:
	            required: 1.0
	          memory:
	            required: 100MB
	        cmd:
	        - "bash lscpu"
	        ports:
	        - name: http
	          value: 8080
	```
