# Containerized Workload

The `ContainerizedWorkload` is a generic workload definition which could be referenced as the schema for long-running containerized workload types. 

> 
Note: it's by design that `ContainerizedWorkload` schema is **NOT** equivalent to Kubernetes `PodSpec`. `ContainerizedWorkload` focuses only on developer facing primitives and exposes ports of containers **by default**.

Below is the schematic specification for `ContainerizedWorkload`. 

### Top-Level Attributes of a containerized workload 

These attributes provide top-level information about the containerized workload.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `apiVersion` | `string` | Y ||  `core.oam.dev/v1alpha2` |
| `kind` | `string` | Y ||  `ContainerizedWorkload` |
| `metadata` | [`Metadata`](#metadata) | Y | | containerized workload metadata. |
| `spec`| [`Spec`](#spec-1) | Y | | A container for the containerized workload spec. |

## Spec

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `osType` | `string` | N |  | The OS required to host (all of) the component's containers (since containers share a kernel with the underlying host). Possible values include:<ul><li>linux</li><li>windows</li></ul> For extended runtimes, this is passed in unaltered. Default can be none and let the runtime decide where to place the component. |
| `arch` | `string` | N |  | The CPU architecture required to host (all of) the component's containers (since containers share physical hardware with the underlying host). Possible values include:<ul><li>i386</li><li>amd64</li><li>arm</li><li>arm64</li></ul> Default can be none and let the runtime chose architecture. |
| `containers` | [`[]Container`](#container) | Y | | The OCI container(s) that implement the component. |

### Container

This section describes the runtime configuration necessary to run a containerized workload for this component.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `name` | `string` | Y | | The container's name. Must be unique per component. |
| `image` | `string` | Y | | A path-like or URI-like representation of the location of an OCI image. Where applicable, this MAY be prefixed with a registry address, SHOULD be suffixed with a tag. |
| `resources` | [`Resources`](#resources) | Y | | Resources required by the container. |
| `cmd` | `[]string`| N | | Entrypoint array. |
| `args` | `[]string`| N | | Arguments to the entrypoint. The container image's CMD is used if this is not provided. |
| `env` | [`[]Env`](#env) | N | | Environment variables for the container. |
| `config` | [`[]ConfigFile`](#configfile) | N | | Locations to write configuration as files accessible within the container |
| `ports` | [`[]Port`](#port) | N | | Ports exposed by the container. |
| `livenessProbe` | [`HealthProbe`](#healthprobe) | N | | Instructions for assessing whether the container is alive. |
| `readinessProbe` | [`HealthProbe`](#healthprobe) | N | | Instructions for assessing whether the container is in a suitable state to serve traffic. |
| `imagePullSecret` | `string` | N | | Key that can be used to retrieve the credentials for pulling this secret. |

The details of the way that a runtime takes `imagePullSecret` and loads credentials is left to the OAM runtime implementation. For example, a Kubernetes implementation may treat this as a key that can be [loaded from a secret](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/).
While it is not required, it is RECOMMENDED that `image` names be suffixed with a digest in [OCI format](https://github.com/opencontainers/image-spec/blob/master/descriptor.md#digests). The digest may be used to compute the integrity of the image. `example/foobar@sha256:72e996751fe42b2a0c1e6355730dc2751ccda50564fec929f76804a6365ef5ef`.

> The name field is required and must be 63 characters or less, beginning and ending with an alphanumeric character ([a-z0-9A-Z]) with dashes (-), underscores (_), dots (.), and alphanumerics between.

#### Resources

Resources describe compute resources attached to a runtime.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `cpu` | [`CPU`](#cpu) | Y |  | Specifies the attributes of the cpu resource required for the container. |
| `memory` | [`Memory`](#memory) | Y | | Specifies the attributes of the memory resource required for the container. |
| `gpu` | [`GPU`](#gpu) | N |  | Specifies the attributes of the gpu resources required for the container. |
| `volumes` | [`[]Volume`](#volume) | N | | Specifies the attributes of the volumes that the container uses. |
| `extended` | [`[]ExtendedResource`](#extendedresource) | N | | Implementation-specific extended resource requirements |

For any resource that cannot be satisfied by the underlying platform, the platform MUST return an error and cease deployment. A resource is considered a requirement, and failure to meet that requirement means the runtime MUST NOT deploy the application. For example, if an application requests `1P` of memory, and that amount of memory is not available, the application deployment must fail. Likewise, if an application requires `1` gpu, and the runtime does not provide gpus, the application deployment MUST fail.

##### CPU

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `required` | `double` | Y |  | The minimum number of logical cpus required for running this container. |

See the Units section above for valid values.

##### Memory

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `required` | `string` | Y | | The minimum amount of memory required for running this container. The value should be a positive integer with/without unit suffix: P, T, G, M, K. If no unit is given, it defaults to 'bytes'. |

See the Units section above for valid values.

##### GPU

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `required` | `double` | Y |  | The minimum number of gpus  required for running this container. |

See the Units section above for valid values.

##### Volume

Volume describes name, a location to mount the volume, along with access mode (such as read/write or read-only) and sharing policy for the mount. It also describes the underneath disk attributes needed by the volume.
The format of the path is specific to the operating system of the consuming component, though implementations SHOULD provide support for UNIX-like path representations.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `name` | `string` | Y |  | Specifies the name used to reference the path. |
| `mountPath` | `string` | Y |  | Specifies the actual mount path in the filesystem. |
| `accessMode` | `string` | N | `RW` | Specifies the access mode. Allowed values are `RW` (read/write) and `RO` (read-only). |
| `sharingPolicy` | `string` | N | `Exclusive` | The sharing policy for the mount, indicating if it is expected to be shared or not. Allowed values are `Exclusive` and `Shared`. |
| `disk` | [`Disk`](#disk) | N |  | Specifies the attributes of the underneath disk resources required by the volume. |

See the Units section above for valid values.

Example:

```yaml
name: "configuration"
mountPath: /etc/config
accessMode: RO
sharingPolicy: Shared
disk:
  required: "2G"
  ephemeral: n
```

The above requires that a read-only volume be mounted at the path `/etc/config`, backed by a volume that provides at least 2G of non-ephemeral storage.

##### Disk

The disk specifies the attributes of the disk used by the volume. It describes information such as minimum disk size and the disk is ephemeral or not. Ephemeral disk indicates the component requires minimum disk size on the node to run it. For example, image processing component may require a larger cache on the node to run could use ephemeral disk. When ephemeral disk is set to false, it indicates external disk will be used.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `required` | `string` | Y |  | The minimum disk size required for running this container. The value should be a positive value, greater than zero. |
| `ephemeral` | `boolean` | N | | Specifies whether external disk needs to be mounted or not. |

See the Units section above for valid values.

##### ExtendedResource

An extended resource is a declaration of a resource requirement for an implementation-specific resource. For example, OAM-compliant platforms may expose special hardware. This field allows containers to indicate that such special offerings are required in order for the containers to operate.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `name` | `string` | Y | | The name of the resource, as a Group/Version/Kind |
| `required` | `string` | Y | | The required condition. |

The `name` field MUST be a group/version/kind identifying the specific resource.

Example:

```yaml
extended:
- name: ext.example.com/v1.MotionSensor
  required: "1"
- name: ext.example.com/v2beta4.ServoModel
  required: z141155-t100
```

If the named extended resource is not available for any reason, implementations MUST return an error when a component instance is created.

#### Env

Env describes an environment variable as a name/value pair of strings.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `name` | `string` | Y | | The environment variable name. Must be unique per container. |
| `value` | `string` | N | | The environment variable value. |

The `name` field must be composed of valid Unicode letter and number characters, as well as `_` and `-`.

Example:

```yaml
env:
  - name: "ADMIN_USER"
    value: "admin"  # This is a literal value
 ```

#### ConfigFile

ConfigFile describes a path to a file available within the container, as well as the data that will be written into that file. This provides a way to inject configuration files into a container.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `path` | `string` | Y | | An absolute path within the container. |
| `value` | `string` | N | | The data to be written into the file at the specified path. If this is not supplied, `fromParam` must be supplied |
| `fromParam` | `string` | N | | The parameter whose value should be written into this file as a value |

The `path` field must contain a path that abides by the pathing rules of the underlying operating system. If a relative path is given, implementations MUST assume the path is relative to the root directory of the container. Implementations MAY produce an error if using such a path would violate security measures or path layout requirements.

Example:

```yaml
config:
  - path: "/etc/access/default_user.txt"
    value: "admin"  # This is a literal value
  - path: "/var/run/db-data"
    fromParam: "sourceData" # This will cause the value to be read from the parameter whose name is `sourceData`
```

If both `fromParam` and `value` are specified, `fromParam` MUST take precedence, even if the parameter value is an empty value. If neither is specified, the runtime MUST produce an error.

#### Port

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `name` | `string` | Y || A descriptive name for the port. Must be unique per container. |
| `containerPort` | `integer` | Y || The port number. Must be unique per container. |
| `protocol` | `string` | N | `TCP` | Indicates the transport layer protocol used by the server listening on the port. Valid values are `TCP` and `UDP`. |

The `name` field must be lowercase alphabetical characters as present in the ASCII character set (0061-007A).

#### HealthProbe

Health Probe describes how a probing operation is to be executed as a way of determining the health of a component.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `exec` | [`Exec`](#exec) | N || Instructions for assessing container health by executing a command. Either this attribute or the `httpGet` attribute or the `tcpSocket` attribute MUST be specified. This attribute is mutually exclusive with both the `httpGet` attribute and the `tcpSocket` attribute. |
| `httpGet` | [`HTTPGet`](#httpget) | N || Instructions for assessing container health by executing an HTTP GET request. Either this attribute or the `exec` attribute or the `tcpSocket` attribute MUST be specified. This attribute is mutually exclusive with both the `exec` attribute and the `tcpSocket` attribute. |
| `tcpSocket` | [`TCPSocket`](#tcpsocket) | N || Instructions for assessing container health by probing a TCP socket. Either this attribute or the `exec` attribute or the `httpGet` attribute MUST be specified. This attribute is mutually exclusive with both the `exec` attribute and the `httpGet` attribute. |
| `initialDelaySeconds` | `integer` | N | `0` | Number of seconds after the container is started before the first probe is initiated. |
| `periodSeconds` | `integer` | N | `10` | How often, in seconds, to execute the probe. |
| `timeoutSeconds` | `integer` | N | `1` | Number of seconds after which the probe times out. |
| `successThreshold` | `integer` | N | `1` | Minimum consecutive successes for the probe to be considered successful after having failed. |
| `failureThreshold` | `integer` | N | `3` | Number of consecutive failures required to determine the container is not alive (liveness probe) or not ready (readiness probe). |

See the Units section above for valid time values.

##### Exec

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `command` | `[]string` | Y || A command to be executed inside the container to assess its health. Each space delimited token of the command is a separate array element. Commands exiting `0` are considered to be successful probes, whilst all other exit codes are considered failures. |

##### HTTPGet

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `path` | `string` | Y || The endpoint, relative to the `port`, to which the HTTP GET request should be directed. |
| `port` | `integer` | Y || The TCP socket within the container to which the HTTP GET request should be directed. |
| `httpHeaders` | [`[]HTTPHeader`](#httpheader) | N || Optional HTTP headers. |

##### HTTPHeader

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `name` | `string` | Y || An HTTP header name. This must be unique per HTTP GET-based probe. |
| `value` | `string` | Y || An HTTP header value. |

Both `name` and `value` must abide by the HTTP/1.1 specification for valid [header values](https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html#sec4.2)

##### TCPSocket

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `port` | `integer` | Y || The TCP socket within the container that should be probed to assess container health. |

Port must be an integer value greater than `0`.

### Units for Time, CPU, Memory, and Disk

In places in this specification, certain units of measure are applied. This section describes those units of measure.

### Timing (Intervals)

For timing, the default unit of time is seconds, represented as an integer.

### CPU count

CPU count is represented as a floating point number, where `1` means one CPU, `2` means 2 CPUs, and `0.5` means half of a CPU.

The exact meaning of this unit varies from platform to platform.  Implementors should consider that logical cpu is equivalent to one AWS vCPU or one Azure vCore or one GCP Core or one IBM vCPU. Fractional values are allowed. If a runtime does not support fractional units, it MUST round up (ceiling function) to the next integer value.

### Memory and Disk

Memory and disk space use the notation for bytes/kilo/mega/giga/tera/peta by just using the major unit:

- `1024` is 1024 bytes
- `88K` is 88 kilobytes
- `5M` is 5 megabytes
- `7G` is 7 gigabytes
- `100T` is 100 terabytes
- `9999P` is 9999 petabytes

If a `B` is appended after the unit letter, it MUST be ignored. Thus, `5M` and `5MB` are treated as identical. Case is unimportant. `15k` and `15K` MUST be treated as the same value.

## Containerized Workload JSON schema
One can also use a [containerized workload schema](containerized_workload_schema.json) to generate a valid component workload.
