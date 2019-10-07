# Introduction

This doc gives an introduction of the specification in a story-based format. It help end users and developers to quickly understand and implement the protocol.

In the following, we are going through a story of application delivery lifecycle. The storyline goes as: 1. The _developer_ creates a web application; 2. The _application operator_ deploys instances of that application, and configures it with operation traits, e.g. autoscaling; 3. The _infrastructure operator_ decides which underlying technology is used to handle the deployment and operations.

## App Developer: Develop an App

First of all, we have a developer who creates an online shopping app. He knows how to write and test the code. The program takes a few parameters: log level, http port, metrics port. But he doesn't know how to deploy and operate the app -- he only needs to deliver the code to the app operator who will handle the rest.

How does the app developer deliver the code? The answer is that he will define a _ComponentSchematic_ yaml. In _Open Application Model_, each piece of program is described as a _ComponentSchematic_ yaml by the app developer. Any information that app operator needs to know about the program will be defined within. For example, the container image packaging the program and the program parameters will be written in a _ComponentSchematic_ yaml. 

The below diagram demonstrates the workflow:

![alt](./assets/dev2ops.png)

## App Operator: Deploy an App

Deploying an application requires runtime traits configuration and live instances deployment. The app operator defines runtime configuration like replica size, autoscaling policy, which cluster to deploy in a _ApplicationConfiguration_ yaml. Writing and deploying a_ApplicationConfiguration_ yaml is equivalent to deploying an app. The underlying platform will create live instances of _ComponentSchematic_ and attach operational traits to them according to the _ApplicationConfiguration_ spec.

The below diagram demonstrates the workflow:

![alt](./assets/ops-deploy-app.png)

## Infra Operator: Configure Platform Capabilities

Now that the app operator deploys a _ComponentSchematic_ yaml. How does it happen in real? The power comes from the underlying platforms.

Each platform provides a group of application deployment and operational capabilities. Infrastructure operators are responsible for declaring, installing, and maintaining the underlying services that are available on the platform. For example, infra operator might choose the Load Balancer technology of a specific cloud provider to expose the service.

The below diagram demonstrates the platform architecture:

![alt](./assets/platform_arch.png)

[Scylla](https://github.com/microsoft/scylla) is a reference implementation of the platform based on Kubernetes. Give it a try to gain hands-on experience.

## The Benefits? Portable Application!

Focused on the separation of development concerns from operational considerations through a platform-agnostic application model, Open Application Model provides a set of tools for building portable containerized applications on systems like Kubernetes.

For end users, they enjoy portable apps across platforms. If an app could be deployed and used on one provider, it should be able to on all other providers. We define must-implement, recommended, and candidate types in _core_, _standard_, and _extended_ APIs. This will ensure portability and provide extensibility in the same spec. Of course, not everything is portable and the primary concern of such an interface is that it is a "lowest-common denominator". Our aspiration is to build a vendor-neutral, community-owned spec and the most popular APIs will be embraced and added to the specification over time. In this way, the evolution will ensure that most users will be successful in building cloud native applications via the Open Application Model.
