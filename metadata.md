### Metadata

This metadata section is made up of several top-level keys.

Metadata provides information about the contents of the object.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `name` | `string` | Y | | A name for the schematic. `name` is subject to the restrictions listed beneath this table. |
| `labels` | `map[string]string` | N | | A set of string key/value pairs used as arbitrary labels on this component. See the "Label format" section immediately below. |
| `annotations` | `map[string]string`| N || A set of string key/value pairs used as arbitrary descriptive text associated with this object. See the "Annotations format" section immediately below. |

#### The `name` Field

The combination of group, kind, name must be unique. Two different kinds (for example, a Component and a Trait) may have the same name and not cause conflicts. Version is not a distinguishing factor.

*Okay:* Each kind allows the name `foo`.
```yaml
apiVersion: core.oam.dev/v1alpha2
kind: Component
metadata:
    name: foo
---
apiVersion: core.oam.dev/v1alpha2
kind: Trait
metadata:
    name: foo
```

*Okay:* Each group (`one.dev` and `other.dev`) allows the kind `Component` with name `foo`.
```yaml
apiVersion: one.dev/v1alpha2
kind: Component
metadata:
    name: foo
---
apiVersion: other.dev/v1alpha2
kind: Component
metadata:
    name: foo
```

*NOT Okay:* Version is not a namespace qualifier.

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: Component
metadata:
    name: foo
---
apiVersion: core.oam.dev/v1
kind: Component
metadata:
    name: foo
```

The `name` field must be formatted as follows:

> The name segment is required and must be 63 characters or less, beginning and ending with an alphanumeric character ([a-z0-9A-Z]) with dashes (-), underscores (_), dots (.), and alphanumerics between.

Unless otherwise noted, names must be unique within the Group/Version/Kind.

#### Label Format

Labels follow the [Kubernetes specification](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) for labeling:
> Valid label keys have two segments: an optional prefix and name, separated by a slash (/). The name segment is required and must be 63 characters or less, beginning and ending with an alphanumeric character ([a-z0-9A-Z]) with dashes (-), underscores (_), dots (.), and alphanumerics between. The prefix is optional. If specified, the prefix must be a DNS subdomain: a series of DNS labels separated by dots (.), not longer than 253 characters in total, followed by a slash (/).

#### Annotations Format

Annotations provide a mechanism for attaching arbitrary text within the metadata of an object. The annotations object follows the [Kubernetes specification](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/#syntax-and-character-set):

> Annotations are key/value pairs. Valid annotation keys have two segments: an optional prefix and name, separated by a slash (/). The name segment is required and must be 63 characters or less, beginning and ending with an alphanumeric character ([a-z0-9A-Z]) with dashes (-), underscores (_), dots (.), and alphanumerics between. The prefix is optional. If specified, the prefix must be a DNS subdomain: a series of DNS labels separated by dots (.), not longer than 253 characters in total, followed by a slash (/).

The following annotation labels are _predefined_ and RECOMMENDED.

| Attribute | Type | Required | Default Value | Description |
|-----------|------|----------|---------------|-------------|
| `description` | `string` | N | | A short description of the component. |
| `version` | `string` | N | | A user provided string defining the [semantic version](https://semver.org/) of the component, e.g. the release version of this software|

If `version` is not supplied, the default version is assumed to be `0.1.0`, per the [SemVer](https://semver.org/) specification.

Example:

```yaml
metadata:
  name: alpine-task
  labels:
    app: my-app-v1  # Non-normative example
  annotations:
    version: "1.0.1"
    description: A task that is backed by an Alpine Linux filesystem
```

The metadata section is used in all schematics. It is also compatible with the Kubernetes metadata section. Note, however, that the Kubernetes metadata is a superset of the OAM metadata, and contains attributes that OAM does not recognize.

## Group, Version, and Kind

Many of the API objects described in this document use a naming scheme called "Group, Version, Kind." This scheme, popularized by Kubernetes, provides a consistent way of namespacing and versioning API objects, and it is used here for OAM API objects versioning.

This section describes the scheme.

### Group

Group is a namespace for collecting several related kinds. Groups use a DNS naming convention. Example groups are:

- components.oam.dev
- functions.azure.com
- my.dev

All of the groups under the `oam.dev` domain are considered reserved for the specification. And all of the objects specified herein belong to groups in that domain.

Group MUST be globally unique.

### Version

The version string is an API version. Following the common paradigm, APIs are versioned by major number only. Minor and patch numbers are omitted from API versions. The actual minor and patch versions of the underlying engines may iterate, but they MUST iterate this version for any breaking change. In other words, the major number serves as a guarantee of compatibility, and minor and patch numbers should not change that guarantee. Therefore, consumers of the API cannot specify a finer granularity than the major version.

An API version number is always prefixed by a `v`, followed by one or more digits.

Examples:

- `v1`
- `v2`
- `v973`

There are two additional modifiers that are part of the major version number:

- `alphaN` (where `N` is one or more digits) indicates that this feature is experimental, and may be removed, but that its current compatibility marker is `1`
- `betaN` (where `N` is one or more digits) indicates that this feature is not yet stable. The `N` is a compatibility marker.

API versions that are marked either `alpha` or `beta` are considered unstable and susceptible to breaking changes.

Only one of the two modifiers may be applied at a time:

- `v1alpha1`
- `v973beta231`

Compatibility is established by _exact match only_. `v1` is not considered compatible with `v2`, `v1alpha1`, or `v1beta2`. 

Versions do not have a uniqueness requirement.

### Kind

The _kind_ is the name of a type. For example, a component's kind is `Component`, while a trait's kind is `Trait`. Kinds are always composed of words where the initial letter is capitalized, and the first letter of every word is capitalized. Kinds _should_ capitalize every letter of an acronym (e.g., `HTTP`, not `Http`).

Kinds MUST be unique within a group.

### Representations of Group/Version/Kind

The fully qualified representation of Group/Version/Kind is `GROUP/VERSION.KIND`. Here are some examples:

- `local.dev/v7alpha2.Proxy`
- `cache.example.com/v1.Redis`
- `azure.com/v2.Functions`

In schematics, the group and version are presented on one field, and the kind is presented on another:

```yaml
apiVersion: local.dev/v7alpha2
kind: Proxy
```

In rare cases, it is necessary to link a group and a kind, but without specifying a version. This is done, for example, when declaring default workload. As a general rule, this behavior is NOT RECOMMENDED, but when necessary, this specification follows the Kubernetes pattern of construct a DNS name out of the plural kind name and the group:

```
Proxies.local.dev # allowed but discouraged
```

This form is not accepted as an alternative for the fully qualified version. It is only accepted in cases where it is explicitly stated by the specification that this form is accepted.
