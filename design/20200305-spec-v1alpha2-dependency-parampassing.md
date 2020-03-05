# Component and Trait Dependency with Parameter Passing

* Author: @wonderflow @hongchaodeng

## Background

In https://github.com/oam-dev/spec/issues/171, we have discussed our dependency stories and started to explore solutions.
Now that we have implemented and evolved the design to solve real world problems, we are proposing the design to OAM spec.

## Goals

In this design, we want to help applications reduce the burden of handling fault tolerance and dependency health.
By putting this work in the OAM platform layer, OAM users can define explicit dependency and parameter passing in a standard way
without carign about the implementation details.

## Use Cases

### Case 1: MySQL and Web

Two components, MySQL and Web exists in the same AppConfig and Web depends on MySQL that:

1. MySQL needs to start first;
2. Web depends on connection credentials from MySQL Component. Once MySQL has started, the credentials will be injected into Web component's environment variables via service binding.

### Case 2: NAS FileSystem and MountTarget

Two components, NAS_FileSystem and NAS_MountTarget exists in the same _ApplicationConfiguration_ and NAS_MountTarget depends on NAS_FileSystem that:

1. NAS_FileSystem needs to be created first;
2. NAS_MountTarget depends on filesystemID from NAS_FileSystem to fill in NAS_MountTarget's spec.

## Proposal

### Component Dependency

For use case 1 and 2, we are proposing to add `dependencies` in _ApplicationConfiguration_ which a component can describe its dependent components.

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: ApplicationConfiguration
spec:
- components:
  - componentName: mysql
  - componentName: web
    dependencies:
      - mysql
---
kind: ApplicationConfiguration
spec:
- components:
  - componentName: nas-file-system
  - componentName: nas-mount-target
    dependencies:
    - nas-file-system
```

### Trait Ordering

For use case 1, we are using a trait called [ServiceBinding](https://github.com/oam-dev/trait-injector) to auto-bind credentials. ServiceBinding trait should be applied before component starts. We propose to add `stage` in TraitDefinition to express ordering relationship with the workload:

```yaml
apiVersion: core.oam.dev/v1alpha2
kind: TraitDefinition
spec:
  appliesToWorkloads:
  - core.oam.dev/v1alpha2.ContainerizedWorkload
  stage: preStart # other options: postStart | preStop | postStop
```

### Parameter Passing

For use case 2, the filesystemID from NasFileSystem's status needs to be passed to NasMountTarget's spec. To solve this problem, we propose to extend _ParameterValue_ to accept value from another component's field:

```yaml
kind: ApplicationConfiguration
spec:
  components:
  - componentName: nas-file-system # The Workload of nas-file-system output a filesystemID at .status.filesystemID

  - componentName: nas-mount-target
    dependencies:
    - nas-file-system
    parameterValues:
    - name: filesystem-id
      from:
        compoent: nas-file-system
        fieldPath: ".status.filesystemID" # take the field's value from nas-file-system's workload
---
apiVersion: core.oam.dev/v1alpha2
kind: Component
metadata:
  name: nas-mount-target
spec:
  workload:
    apiVersion: ros.aliyun.com/v1alpha2
    kind: NASMountTarget
    spec:
      filesystemID: "" # to be filled
  parameters:
  - name: filesystem-id
    fieldPath: ".spec.filesystemID"
```
