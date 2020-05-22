# Introduction

This doc gives an introduction of the Open Application Model in a story-based format. Its goal is to help end users and developers quickly understand and implement the specification.

In the following, we go through a story that describes an application delivery lifecycle. The storyline looks like this:

1. The _developer_ creates a web application;
2. The _application operator_ deploys instances of that application, and configures it with operational traits, such as autoscaling;
3. The _infrastructure operator_ decides which underlying technology is used to handle the deployment and operations.

## Application developer: write and test code

The story begins with the application developers who create an application, such as an online shopping application. They know how to write and test the code. The application program takes a few parameters such as log level and HTTP port. To let the developers focus on implementing the application's business logic, we have application operators (either human or automated operation platforms) take care of operational tasks. This provides a "serverless" experience to application developers: they only need to develop and package the application, and then deliver it to application operators to operate.

To deliver their applications, developers need to define _Component_ YAML files. In the _Open Application Model_, each individual component of a program is described as a _Component_ YAML by the application developer. This file encapsulates a workload and the information needed to run it. For example, it can contain the container image packaging the program, whether it needs an endpoint, if it's designed to run as a task to completion or as a server, environment variables, and any parameters the developers want to expose to an operator to override at deployment time.

The following diagram demonstrates this workflow:

![alt](./assets/dev2ops.jpg)

## Application operator: deploy and operate applications

To run and operate an application, the application operator sets parameter values for the developers' components and applies operational characteristics, such as replica size, autoscaling policy, ingress points, and traffic routing rules in an _ApplicationConfiguration_ yaml. In OAM, these operational characteristics are called _traits_. Writing and deploying a _ApplicationConfiguration_ yaml is equivalent to deploying an application. The underlying platform will create live instances of defined workloads and attach operational traits to workloads according to the _ApplicationConfiguration_ spec.

The following diagram demonstrates the workflow:

![alt](./assets/ops-deploy-app.jpg)

## Infrastructure operator: configure platform capabilities

The application developer and application operator have so far described an application and its operational characteristics in platform-neutral terms. The power of the Open Application Model comes from the underlying platforms that implements the model, which can surface the capabilities that make the underlying platforms unique and useful through OAM in a way that is consistent across any platform that supports the model.

Infrastructure operators are responsible for declaring, installing, and maintaining the underlying services that are available on the platform. For example, an infrastructure operator might choose a specific load balancer technology when deploying to a cloud provider to expose the service.

The following diagram demonstrates the platform architecture:

![alt](./assets/platform_arch.png)


## For platform builders

Focused on the separation of development concerns from operational considerations through a platform-agnostic specification, the _Open Application Model_ brings modular, extensible, and portable design to build and deliver applications on platforms like Kubernetes.

With OAM, platform builders can provide reusable modules in the format of _Components_, _Traits_, and _Scopes_. This allows platforms to do things like packaging them in predefined application profiles. Users choose how to run their applications by selecting profiles, for example, microservice apps with high SLO requirements, stateful apps with persistent volumes, or event driven functions with horizontally autoscaling. This brings a serverless experience to end users in a cloud native way, all due to the modular design.

Another benefit for end users is having portable apps across platforms. If an app can be deployed and used on one provider, it should be able to run on any other providers. We define must-implement, recommended, and candidate types in _core_, _standard_, and _extended_ APIs. This will ensure portability and provide extensibility in the same spec. Of course, not everything is portable and the primary concern of such an interface is that it is a "lowest-common denominator". Our aspiration is to build a vendor-neutral, community-owned spec and the most popular APIs will be embraced and added to the specification over time. In this way, the evolution will ensure that most users will be successful in building cloud native applications via the Open Application Model.
