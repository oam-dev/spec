# Core Workload Types

The following core workload types are defined by this specification:

|Name|Type|Service endpoint|Replicable|Daemonized|
|-|-|-|-|-|
|[Service](#service)|core.hydra.io/v1alpha1.Service|Yes|Yes|Yes
|[Singleton Service](#singleton-service)|core.hydra.io/v1alpha1.SingletonService|Yes|No|Yes
|[Worker](#worker)|core.hydra.io/v1alpha1.Worker|No|Yes|Yes
|[Singleton Worker](#singleton-worker)|core.hydra.io/v1alpha1.SingletonWorker|No|No|Yes
|[Task](#task)|core.hydra.io/v1alpha1.Task|No|Yes|No
|[Singleton Task](#singleton-task)|core.hydra.io/v1alpha1.SingletonTask|No|No|No

## Service

Workload type name: `core.hydra.io/v1alpha1.Service`

A service is used for long-running, scalable workloads that have a network endpoint with a stable name to receive network traffic for the component as a whole. Common use cases include web applications and services that expose APIs. The Service workload type has the following characteristics:

- Defines a container runtime where zero or more replicas of the same container may be run simultaneously.
- An application operator can increase or decrease the number of service replicas by applying and configuring traits when available.
- A service is daemonized. A runtime MUST attempt to restart replicas that exit regardless of error code.
- A service has a network endpoint with an automatically assigned virtual IP address (VIP) and DNS name addressable within the network scope to which the component belongs.

## Singleton Service

Workload type name: `core.hydra.io/v1alpha1.SingletonService`

A singleton service is a special case of the Service workload type that is limited to at most one replica. The singleton service workload type as the following characteristics:

- Defines a container runtime where at most one replica of the same container may be run at a time.  
- A singleton service is daemonized. A runtime MUST attempt to restart the singleton replica if it exits regardless of error code.
- A singleton service has a network endpoint with an automatically assigned virtual IP address (VIP) and DNS name addressable within the network scope to which the component belongs.

## Worker

Workload type name: `core.hydra.io/v1alpha1.Worker`

A worker is used for long-running, scalable workloads that do not have a service endpoint for network requests, aside from optional liveliness and readiness probe endpoints on individual replicas. Workers are typically used to pull from queues or provide other offline processing. The worker workload type has the following characteristics:

- Defines a container runtime where zero or more replicas of the same container may be run simultaneously.
- An application operator can increase or decrease the number of worker replicas by applying and configuring traits when available.
- A worker is daemonized. A runtime MUST attempt to restart replicas that exit regardless of error code.

## Singleton Worker

Workload type name: `core.hydra.io/v1alpha1.SingletonWorker`

A singleton worker is a special case of the worker workload type that is limited to at most one replica. The singleton worker workload type as the following characteristics:

- Defines a container runtime where at most one replica of the same container may be run at a time.  
- A singleton worker is daemonized. A runtime MUST attempt to restart the singleton replica if it exits regardless of error code.

## Task

Workload type name: `core.hydra.io/v1alpha1.Task`

A task is used to run code or a script to completion. Commonly used to run cron jobs or one-time highly parallelizable tasks that exit and free up resources upon completion. The task workload type has the following characteristics:

- Defines a container runtime where zero or more replicas of the same container may be run simultaneously.
- An application operator can increase or decrease the number of worker replicas by applying and configuring traits when available
- A task is non-daemonized. A runtime MUST NOT attempt to restart replicas that exit without an error code.

## Singleton Task

Workload type name: `core.hydra.io/v1alpha1.SingletonTask`

A singleton task is a special case of the task workload type that is limited to at most one replica. The singleton task workload type as the following characteristics:

- Defines a container runtime where at most one replica of the same container may be run at a time.  
- A singleton task is non-daemonized. A runtime MUST NOT attempt to restart replicas that exit without an error code.

Core workload descriptions MUST include a `container` section in the component schematic. Implementations of the core workload types MUST NOT require that a `workloadSetting` list be present in the component description (though it may allow that some settings are passed that way only if their default values are sufficient for running a workload).
