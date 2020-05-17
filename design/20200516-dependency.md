# Resource Dependency in OAM

- Owner: Hongchao Deng (@hongchaodeng)
- Date: 05/17/2020

## Background

Creating one application service or resource often relies on other services or resources to be ready.
This is a well-known pattern called dependency.

In OAM, we talk about dependency in two different contexts:

1. **Inter ApplicationConfiguration**:
  There is dependency workflow between multiple ApplicationConfigurations. This usually can be achieved
  via external workflow engine such as Tekton/Argo Workflow.
2. **Intra ApplicationConfiguration**: 
  There is dependency between Components and Traits within the same ApplicationConfiguration.
  Since they are applied at the same time, external workflow engine couldn't do anything to control their ordering.
  It is the OAM runtime that needs to satisfy this requirement.

In this proposal, we are handling the second case, *Intra ApplicationConfiguration* dependency.

## Goals

This proposal includes the following goals:

1. **Describe ordering for k8s-style resources**.
  One key part of depedency is ordering, i.e. creating resources one after another.
  For example, resource A could not be created unless resource B has been ready (we will describe the meaning of ready below).
  We want to apply this to k8s-style resource.
  Since both components and traits are k8s-style resources, as long as we achieve this goal it should satisfy OAM requirements too.
2. **Describe field data passing between resources**.
  If we think dependency as ordering, it is still not enough. We also need to handle field data passing.
  For example, resource A's field "spec.connSecret" relies on resource B's field "status.connSecret". 
  This means the field data of one resource is passed from another field of other resource and needs to wait the field data to be ready.
  In fact, ordering in the first goal can be modeled as data dependency as well except that the receiver doesn't use the data.
3. **OAM native experience**.
  OAM has application concepts like Components, Traits, etc.
  The solution will provide OAM native experience built on top of the generic solution. For example, modeling as traits.

In the following we will first go through the use cases and then propose a solution to satisfy them.


## Use Cases

### 1. Web app and database

An applicationConfiguration consists of two components: web and db (database).
Web component should not be created until db component is created,
including public endpoint and connection secret to be made ready.

### 2. ML app workflow

An ML applicationConfiguration from [Hypercycle ML](http://www.4paradigm.com/product/hypercycleml)
has three components:

- preprocess
- training
- serving

They have strict ordering:

```
preprocess -> training -> serving
```

Each component has to wait for data to be finished processing by the previous stage.
Today, ML frameworks including Tensorflow do not handle such dependency issue.
They rely on external tools (e.g. Argo Workflow) to handle it.
If we want to model them as Components in the same ApplicationConfiguration, the external tools could not help in this case.

### 3. Service-binding Trait

Service binding is an OAM trait that we develop to bind user specified input such as env or file path
to specific data sources such as secrets or events.
More background introduction can be found in this [slides](https://docs.google.com/presentation/d/1PseN_8_zZH8clWZJzP8tuRMpz51SxQb-mKeG1f4QVZQ/edit?usp=sharing).

A service binding trait needs to be applied before creating the component.
Moreover, this kind of trait ordering is quite general and applies to many other traits we have seen.

### 4. NSQ deployment

An [NSQ](https://github.com/nsqio/nsq) cluster is composed with three components: nsqd, nsqlookup and nsqadmin.
To set up a `nsq` cluster, one needs to first make sure that the `nsqd` component is up, then start and make sure that the `nsqlookup` component is up, and run the `nsqadmin` component at last.
nsqlookup consumes the result of nsqd component instance, and nsqadmin consumes the result of nsqlookup component instance.
If we couldn't describe the dependencies order between the components, the component couldn't run properly.

This is similar to [kubernetes#65502](https://github.com/kubernetes/kubernetes/issues/65502) , the issue describes dependencies between containers on the same Pod, while we are discussing dependencies between components.


## Proposal

The overall idea is to have each resource object specify data inputs coming from other resources' fields.
The OAM runtime will build the dependency graph accordingly and only create resources once the data inputs are ready.
To achieve this, we propose:

1. Add the following `DataInputs` into each resource.

    ```go
    type DataInputs []DataInput

    type DataInput struct {
      // FromDataOutput specifies the name of the DataOutput object in the same namespace.
      FromDataOutput string `json:"fromDataOutput"`

      // ToFieldPaths specifies an array of fields within this resource object
      // that will be overwritten by the value of given DataOutput. The type of the
      // parameter (e.g. int, string) is inferred from the type of these fields;
      // All fields must be of the same type. Fields are specified as JSON field
      // paths without a leading dot, for example 'spec.replicas'.
      // If empty, it means this input is not used at all.
      // This is allowed for resource ordering purpose.
      ToFieldPaths []string `json:"toFieldPaths"`
    }
    ```

    For example, a web application may specify input of mysql secret in yaml style:

    ```yaml
    datainputs:
    - toFieldPaths: ["spec.connSecret"]
      fromDataOutput: mysql-conn-secret # check below DataOutput of the same name
    ```

   The above DataInputs will be json-marshaled and put into an annotation key `core.oam.dev/datainputs` of my-app Deployment:

    ```yaml
    kind: Deployment
    metadata:
      name: my-app
      annotations:
        "core.oam.dev/datainputs": "[{\"toFieldPaths\": [\"spec.connSecret\"],\"fromDataOutput\": \"mysql-conn-secret\"}]"
    ```

    Note that my-app won't be created since it has DataInputs dependency to satisfy.

2. Add a new CRD `DataOutput` with the following definition:

    ```go
    type DataOutput struct {
      metav1.TypeMeta   `json:",inline"`
      metav1.ObjectMeta `json:"metadata,omitempty"`

      Spec   DataOutputSpec   `json:"spec,omitempty"`
      Status DataOutputStatus `json:"status,omitempty"`
    }

    type DataOutputSpec struct {
      // Resource specifies the reference of specific resource, which points to a unique resource.
      Resource ResourceReference `json:"resource"`

      // FieldPath specifies the field path of a given k8s-style resource.
      FieldPath string `json:"fieldPath"`
    }

    type ResourceReference struct {
      APIVersion string `json:"apiVersion"`

      Kind string `json:"kind"`

      // Name indicates the name of the object in the same namespace.
      Name string `json:"name,omitempty"`
    }

    type DataOutputStatus struct {
      // Ready indicates whether the field data of the source resource is ready.
      Ready bool `json:"ready"`
    }
    ```

    For example, a mysql connection secret DataOutput coming from RDSInstance my-rds would look like:

    ```yaml
    apiVersion: core.oam.dev/v1alpha2
    kind: DataOutput
    metadata:
      name: mysql-conn-secret
    spec:
      resource:
        apiVersion: alibaba.oam.crossplane.io/v1alpha1
        kind: RDSInstance
        name: my-rds
      fieldPath: "status.connSecret"
    ```

    In this case, the OAM runtime will automatically build a dependency between the `my-app` and `my-rds`.

    It will wait until `my-rds` object's field `status.connSecret` has value, and marks the DataOutput status ready:

    ```yaml
    apiVersion: alibaba.oam.crossplane.io/v1alpha1
    kind: RDSInstance
    metadata:
      name: my-rds
    spec:
      ...
    status:
      connSecret: mysql-conn # OAM runtime will wait for this field to have value
    ---
    apiVersion: core.oam.dev/v1alpha2
    kind: DataOutput
    metadata:
      name: mysql-conn-secret
    ...
    status:
      ready: true # once the above connSecret has value this will be marked true
    ```

    At this point, the dependency has been satisfied.
    The OAM runtime will create `my-app` object with corresponding values passed to its fields.
    
    ```yaml
    kind: Deployment
    metadata:
      name: my-app
      ..
    spec:
      connSecret: mysql-conn
    ```

3. In OAM Component we put a special syntax sugar in `Parameters` fields:
  as long asthe name of the Parameter matches the name of the DataOutput, it becomes a DataInput.

    For example, the following parameter mysql-conn-secret in Component matches the name of DataOutput trait
    in the same ApplicationConfiguration.

    ```yaml
    kind: Component
    metadata:
      name: my-app
    spec:
      workload:
        kind: Deployment
        metadata:
          name: my-app
      parameters:
      - name: mysql-conn-secret # this matches the name of DataOutput
        fieldPaths: ["spec.connSecret"]
    
    ---
    # In ApplicationConfiguration
    components:
    - componentName: my-app
      traits:
      - trait:
          apiVersion: core.oam.dev/v1alpha2
          kind: DataOutput
          metadata:
            name: mysql-conn-secret # this matches the name of a component's parameter
          spec:
            resource:
              apiVersion: alibaba.oam.crossplane.io/v1alpha1
              kind: RDSInstance
              name: my-rds
            fieldPath: "status.connSecret"
    ```

    In this case, the OAM runtime will build a dependency from my-app to my-rds,
    wait until the field data ready, and create the workload patched with DataInputs annotation.

    ```yaml
    kind: Deployment
    metadata:
      name: my-app
      annotations:
        "core.oam.dev/datainputs": "[{\"toFieldPaths\": [\"spec.connSecret\"],\"fromDataOutput\": \"mysql-conn-secret\"}]"
    ```

### Limitations

All the resources in the dependency graph should be created and managed by OAM runtime.

## Solution examples

In this section we are providing more solution examples for dependency use cases.

1. Service binding trait:

    ApplicationConfiguration (partial):

    ```yaml
    components:
    - componentName: my-app
      traits:
      - trait:
          apiVersion: core.oam.dev/v1alpha1
          kind: ServiceBinding
          metadata:
            name: my-secret-binding
          spec:
            from:
              secret:
                name: my-secret
            to:
              env: true
      - trait:
          kind: DataOutput
            resource:
              apiVersion: core.oam.dev/v1alpha1
              kind: ServiceBinding
              name: my-secret-binding
            fieldPath: "status.ready"
    ```

    Component:

    ```yaml
    kind: Component
    metadata:
      name: my-app
    spec:
      parameters:
      - name: my-secret-binding
        fieldPaths: [] # This parameter is for ordering purpose only
    ```
