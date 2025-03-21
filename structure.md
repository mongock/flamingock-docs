> **NOTE 1**: Instead of cloud-native focus, I think we should introduce the concept of 'Flamingock Server' instead. This clarifies that the biggest difference from Mongock is that Flamingock requires the installation (self-hosted) or usage (Cloud) of new component that is Flamingock Server rather than the traditional, more feature-limited Mongock that relied solely on an existing DB instance.

> **NOTE 2**: We should tag the sections that are Flamingock Server dependant vs self-provisioned. Given the traction / number of requests for the Mongock PRO license, there were many request originating from the technical documentation. It is important we keep this channel as a source of marketing.

> **NOTE 3**: For features that we know that are in the pipeline and scheduled for a release, we can add a new "Upcoming" tag. However, if this tag is added, is because we believe it will 100% make it to production in a relatively short-medium term. We should also add a section that links to GitHub for people to make new feature requests. 

------
_**Structure**_

# Introduction
- Overview: Brief introduction to Flamingock, its evolution from Mongock, and the introduction of Flamingock Server (with a Cloud and self-hosted options). Highlight it can still support a self-provisioned backend.
- Introduce Change-as-code as a formal concept
- Key Features: Highlight the most important features (Auditing & Rollback, Cloud-Native, Extended System Support, etc.).
- Use Cases: Practical examples of where Flamingock can be applied (e.g., database versioning, configuration management, etc.).
> This section should be very consistent / in tune  with the Website and not differ too much.

# Getting Started
- Installation: Guide for setting up Flamingock cloud and client, including both cloud and fallback local driver options.
- Quick Start Guide: A hands-on tutorial for quickly integrating Flamingock into an existing project or system.
- Configuration: Configuration options for setting up Flamingock, including environment variables and default settings.
> Note that I've swapped the order and put this as a start. We'd want to encourage people to get hands on asap rather than going via lenghty explanations (specially useful for experienced users). 

# Technical overview
- Provide the architectural overview and an explanation with hyperlinks to the core concepts. Introduce the deployment flavours:
  - With the Flamingock Server (Cloud and self-hosted)
  - With self-provisioned backend (DB)

## Core Concepts
- Change Units & Auditing: Explanation of how Flamingock handles changes and versions across systems.
- Runner: Explain the runner.
- Driver: Explain Driver
- Cloud: Explain cloud mechanism
- Transaction Handling: How transactions are managed to ensure data integrity in Flamingock Server and self-provisioned backend.
- Integration with local driver as an alternative
- Rollback: Detailed explanation of auditing features and rollback mechanisms, including how to query and restore previous states.
- Workflows and stages: Overview of Flamingock's advanced workflow management, including sequential, parallel, and combined workflows, and  stages
- Template: Explanation of templates.
- Distributed Locking: How Flamingock handles synchronization and locking in distributed environments.
Etc.
> Note: Merged Concepts and Architecture sections into a 'Technical Overview' one. 

# Features
- Add table matrix with functionality supported with Flamingock Server vs self-provisioned backend
- GraalVM: Explanation of how Flamingock handles changes and versions across systems.
- Workflows: Explanation workflows and stages
- Custom injections
- Transactions
- Events
- Multitenant
- Etc

# Templates
- Explain

# Testing
- Explain

# CLI
- Explain

# Flamingock client
## Configuration
- Setting up client with Flamingock-server
- Setting up client with self-provisioned backend

## Runners 
- Standalone: Explain standalone runner
- Spring Boot v3: Explain Spring Boot v3 runner
- Spring Boot v3: Explain Spring Boot v4 runner

# Flamingock Server
## Cloud 
- Set up Orgs, Users, Access, etc
- Security: Create tokens
- Audit
- Dashboard configuration
- Setting up alerts and metrics
- etc

## Self-hosted (Upcoming)

# Self-provisioned compatible drivers
- MongoDB V3
- MongoDB Sync4
- MongoDB Spring Data V2
- …

# Resources ---> What is this?
- Explains

# Migration from Mongock
- from v3 to Flamingock
- from v4 to Flamingock
- id+ author potential issue when importing if changed annotation in changeUnit
> Note: Regarding the last point, there is some room to discussion

# FAQ  --> Separate file
- Explains

# Contributing --> Separate file
- Explains

# Changelog  --> Separate file
- History of changes or links to github releases. We need to keep also the updated about new features and changes in the server
