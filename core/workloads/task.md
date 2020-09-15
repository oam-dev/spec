# Task

A Task is a OAM core workload type to run code or a script to completion. Commonly used to run one-time highly parallelizable tasks that exit and free up resources upon completion. The task workload type has the following characteristics:

- Defines a container runtime where zero or more replicas of the same container may be run simultaneously.
- An application operator can increase or decrease the number of worker replicas by applying and configuring traits when available
- A task is non-daemonized. A runtime MUST NOT attempt to restart replicas that exit without an error code.

## How to use?

TODO: the schema of Task is still under design.