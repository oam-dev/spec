# Personas Introduction

This doc gives an introduction of the personas of Open Application Model in a story-based format. Note that these are extracted from a combination of user stories, research, and user input. While this is not an exhaustive list of possible roles, these are the roles identified as the primary targets for this specification.

- __Software Owner__ deliver business value in the form of software. While they should understand the operational characteristics of the software they deliver, they are unconcerned with _how_ operational requirements are fulfilled. For instance, a software owner may be aware that its code writes data to a specific path on a file system, but need not concern themselves with what kind of volume (disk) is mounted to that path or how that dependency is fulfilled. Software owner will register softwares as supported component types in the platform.

- __Application operators__ deliver business value by configuring, installing, and managing components and/or applications such as updating, scaling, auto recovery, etc. Unlike software owners, operators are concerned with _how_ a component or application's operational requirements are fulfilled. For instance, if a software owner has declared that a component writes data to a specific path on a file system, an operator may concern themselves with mounting an appropriate volume to that path.

- __Infrastructure operators__ deliver value by building the application centric platform and managing low-level infrastructural components. This may range from managing the physical hardware in an on-premises network to directly managing cloud service offerings in a public cloud. Infrastructure operators are less concerned with the particular configuration needs of _an application_, focusing instead on the big picture of how an enterprise's overall infrastructure is managed. For example, an infrastructure operator may manage the underlying storage offerings that are used for provisioning persistent storage.

We will go through a story that describes an application delivery lifecycle with a OAM based platform. The storyline looks like this:

1. The _infrastructure operator_ creates a platform with supported component types and operational features to handle the deployment and operations;
2. The _software owner_ creates a web application, register it as a new component type in the platform;
3. The _application operator_ assemble that component into an application, configures it with operational traits, such as autoscaling, and deploys the application to target runtime.

## Infrastructure Operator: Maintain Platform Capabilities

The most power of the Open Application Model comes from the underlying platforms that implements the model, which can surface the capabilities that make the underlying platforms unique and useful through OAM in a way that is consistent across any platform that supports the model.

Infrastructure operators are responsible for declaring, installing, and maintaining the underlying capabilities that are available on the platform. For example, an infrastructure operator might choose a specific load balancer technology when deploying to a cloud provider to expose the service.

The following diagram demonstrates the platform architecture:

![alt](./assets/platform_arch.png)

With OAM, infrastructure operator (or platform builders) can benefit from reusable modules in the format of _Component Types_, _Traits_, and _Scopes_. This allows platforms to do things like build an application centric platform that expose these higher level abstractions as application level APIs, or assemble them in predefined application templates. In later case, software owners could then choose how to run their applications by selecting component types, for example, microservice apps with high SLO requirements, stateful apps with persistent volumes, or event driven functions with horizontally autoscaling. This brings a serverless experience to end users in a cloud native way, all due to the modular design.

At the meantime, OAM essentially enables portable application definitions across runtimes. If an app can be deployed and used on one provider, it should be able to run on any other providers. We define must-implement, recommended, and candidate types in _core_, _standard_, and _extended_ APIs. This will ensure portability and provide extensibility in the same spec. Of course, not everything is portable and the primary concern of such an interface is that it is a "lowest-common denominator". Our aspiration is to build a vendor-neutral, community-owned spec and the most popular APIs will be embraced and added to the specification over time. In this way, the evolution will ensure that most users will be successful in defining cloud native applications via the Open Application Model.

## Software Owner: Define and Register Software as Component

The infrastructure operator have so far build a OAM based platform with certain runtime system. The story begins with the software owns who owns an application, such as an online shopping application. The application program takes a few parameters such as log level and HTTP port. To let the software owner focus defining the application itself, we have application operators take care of operational tasks. This provides a "serverless" experience to application owners: they only need to define and package the application as component types, and then leave to application operators to instantiate these components and operate.

To deliver their applications, software owners need to define _Component Type_ YAML files. In the _Open Application Model_, each individual component type is described as a _WorkloadDefinition_ YAML by the software owner. This file encapsulates a workload and the information needed to run it. For example, it can contain the container image packaging the program, whether it needs an endpoint, if its designed to run as a task to completion or as a server, environment variables, and any parameters the software owners want to expose to an operator to override at deployment time. 

## Application Operator: Deploy and Operate Applications

To run and operate an application, the application operator sets parameter values for the components and applies operational characteristics, such as replica size, autoscaling policy, ingress points, and traffic routing rules in an _Application_ yaml. In OAM, these operational characteristics are called _traits_. Writing and deploying a _Application_ yaml is equivalent to deploying an application. The underlying platform will create live instances of defined workloads and attach operational traits to workloads according to the _Application_ spec.

Note that application operator could be the platform itself, i.e. the automated operation platforms will assign traits to components based on intention of application owners.

