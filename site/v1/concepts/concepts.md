---
title: Flamingock Concepts
date: 2014-04-18 11:30:00
permalink: /v1/concepts/index.html
eleventyNavigation:
  version: v1
  root: true
  order: 5
---
<h1 class="title">Main Concepts</h1>

[[TOC]]

## Change Units
A change unit is an individual unit of change that contains executable code along with its corresponding rollback mechanism. It can be defined using a code-based approach or through low-code options.

## Auditing
Flamingock tracks and stores detailed information about each change unit, including system state, versioning, execution duration, author, error details, and more, ensuring full transparency and traceability.

## Cloud

Flamingock's cloud offering is the default and recommended approach, providing a **maintenance-free experience**
with enhanced features, better audit history, and improved scalability. It’s designed to reduce operational
overhead while offering more advanced capabilities compared to local drivers. The cloud service also includes
a **free tier**, allowing users to experience core features with limited resources.

For a more detailed explanation of advanced cloud features, please refer to the [cloud page](/v1/cloud).

## Driver
In Flamingock, a driver is the mechanism responsible for storing essential operational data, such as audit
information, distributed locks, and system state. Drivers ensure Flamingock can track and manage
change executions reliably and consistently.

### Cloud Driver
It's the driver implementation for leveraging the cloud offering to store its operational data

### Local Drivers
For users who prefer not to use the cloud offering, it's the driver implementation for using a user-provided database to store Flamingock's operational data.

## Runner
The runner is the orchestrator component dealing with the process logic, configuration, dependencies, framework and any environmental aspect. It’s the glue that puts together all the components. It takes the workflow, driver and framework, and run the process.

Mongock provides different runner for multiple frameworks(standalone, Springboot, Micronaut...) and it can be combined with any driver

### Standalone Runner
Intended for vanilla Java applications or frameworks not directly supported by Flamingock. It offers a simple, flexible way to run Flamingock processes without relying on any specific framework.

### Spring Boot Runner
An extension of the standalone runner, designed to integrate seamlessly with Spring Boot applications. It provides additional features and integrations. However, the standalone runner can be used just as effectively in Spring Boot or non-Spring environments.

## Rollbacks
Triggered automatically when a change unit fails during execution. For transactional systems, Flamingock uses the system’s native rollback mechanism; otherwise, the user provides rollback code.

## Undo
Allows users to revert to a specific system state by rolling back changes from the most recent change unit up to a specified change unit, date or tag. This is useful when the user wants to undo a set of applied changes.

## Workflow
Flamingock organizes the execution of change units into workflows, which consist of multiple stages.

### Stages
Groups of change units that can run in sequence or in parallel, based on the configuration.

### Workflow
A collection of stages that defines the process flow for executing change units. Stages are executed in parallel by default but can include dependencies to enforce sequential execution.
Workflows provide a structured approach to manage changes efficiently and ensure scalability during execution.

## Templates
Templates provide a higher-level abstraction over change units, allowing users to define reusable change units
via YAML or other formats. Templates can handle various use cases, from database migrations to complex system configurations, and can be created and shared by users, contributors, or Flamingock itself.

## Locking
Flamingock uses distributed locking to ensure that only one process applies changes at a time, preventing concurrency issues in distributed systems. While there is a default locking configuration, more granular control can be configured for demanding scenarios.
