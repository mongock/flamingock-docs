---
title: What is Flamingock?
date: 2024-12-16 11:30:00 
permalink: /index.html
eleventyNavigation:
  version: v1
  root: true
  order: 0
---

<p class="text-center">
    <img src="/images/flamingock-logo-with-title.png" width="65%" alt="Flamingock">
</p>

[//]: # (<div class="success">)

[//]: # (<b>Flamingock 5 released!!</b> Please visit the <a href="/v1/from-version-4-to-5">upgrade page</a> to follow easy process. )

[//]: # (</div>)

## Introduction

Flamingock is the evolution of Mongock, reimagined as a cloud-native tool for distributed environments. 
It seamlessly integrates with your application to manage versioning and audit changes in systems that evolve alongside it

Whilst Mongock focused on versioning NoSQL databases, Flamingock extends this concept to all technologies, systems,
and configurations, with built-in auditing and rollback capabilities. It ensures the application and its dependent 
components evolve together by managing configuration changes during startup to maintain version compatibility, 
streamline integration, and reduce conflicts at deployment time.

>Additionally, Flamingock offers multiple infrastructure setups for providing flexibility to users, as it introduces a Cloud offering whilst still retaining existing supported databases such as MongoDB, DynamoDB, or Couchbase.

------------------------------------------------

## Why Flamingock?
Our mission is to enable developers to deploy and manage stateful changes in distributed systems in a safe and reliable manner. 

In a nutshell:

- **Seamless System Evolution**: Ensure your application and its dependencies evolve together. Flamingock manages configuration and system changes during startup, maintaining compatibility across components and reducing conflicts.

- **Broad Applicability**: Move beyond NoSQL databases. Flamingock supports any system, database, or configuration, offering unmatched flexibility for various architectures and technologies.

- **Cloud-Native Convenience**: Leverage Flamingock's cloud offering for a fully managed solution. Focus on building your application while Flamingock handles audited change data management.

- **Built-In Auditing and Rollback**: Gain full control over changes with built-in auditing capabilities and rollback mechanisms, ensuring consistency across deployments.

- **Low-Code and Code-Based Options**: Define changes in a way that suits your workflow. Whether you prefer traditional code-based methods or intuitive no-code templates, Flamingock adapts to your needs.

- **Optimised for Distributed Environments**: With features like distributed locking and parallel synchronized execution, Flamingock is designed to handle the complexities of distributed deployments, ensuring consistent and efficient updates.

- **Future-Ready with GraalVM Support**: Compile Java applications into native executables for improved performance, making Flamingock a future-proof choice for modern applications.

- **Multi-Tenant Support**: Simplify operations for SaaS and multi-tenant applications by managing changes across multiple tenants with a single infrastructure.

- **Multi-ecosystem**: It provides support for multiple programming languages and ecosystems.

- **Multi-framework support**: Can be used together with most, if not all, frameworks, like Spring boot, Micronaut, Quarkus, etc.

- **Widely adopted**: Adopted by well-known frameworks such as JHipster as part of the scaffolding, as well as big corporations and tech companies in different industries.

- **Regularly maintained**: We maintain and update features regularly

- **Open source**: We are an open source tool, operating under the Apache License 2.0

- **Database ADN**:  While it supports a big range of databases, as any other system, tt is the most reliable production-grade solution for MongoDB migrations currently in the market, compatible with Mongo Atlas and different MongoDB versions.


Get more information about our support model at [support@mongock.io](mailto:support@mongock.io) and we can help you walking you to production. 

------------------------------------------------

## How it works

Imagine you're developing an application or service that relies on multiple services, a database, and external configurations. Every time you deploy a new version of the application, you also need to update:

Database schema to support new features.
Configuration settings for integrations with third-party APIs.
System-wide changes that depend on the new codebase.
In a traditional setup, coordinating these updates is tedious and error-prone. For example, deploying a database change ahead of the code might cause runtime errors, while applying the code first could break older configurations.

This is where Flamingock excels.

**With Flamingock**:

Change units ensure database updates, configurations, and other system changes are versioned alongside your application code.
The changes are applied at runtime during the application startup, ensuring compatibility between the code and the systems it depends on.
Built-in auditing and rollback mechanisms provide full control, reducing risks during deployment.
Flamingock is particularly effective in distributed environments, where multiple service instances or tenants require synchronized updates across systems to maintain consistency and prevent conflicts.

### ... other ways of running Flamingock
The explained way of running Flamingock is the common and traditional use case. However, Flamingock can be used in wider use cases and offers more operations to support these.

You can use the **Flamingock CLI** to apply your changes, but also other operations like undo, list and more supported operations. The purpose is to provide a flexible manner of executing migrations.

Please, visit the [CLI section](/v1/cli/) for more information.