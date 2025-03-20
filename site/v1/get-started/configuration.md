---
title: Get started
date: 2014-04-18 11:30:00 
permalink: /v1/get-started/configuration/index.html
eleventyNavigation:
  version: v1
  root: true
  order: 15
---

<h1 class="title">Configuration</h1>

[[TOC]]

# Flamingock Configuration

## Introduction
Flamingock provides flexibility in configuration to cater to various use cases. This section guides you through setting up and customizing Flamingock to suit your needs.

## Configuration Options
Flamingock's configuration is divided into the following categories:

- **Core Configuration**: Shared across all setups.
- **Cloud Configuration**: For using Flamingock's managed cloud service.
- **Local Configuration**: For storing metadata in a user-managed database (e.g., MongoDB or DynamoDB).
- **Runner Configuration**: Specific to the application type (Standalone or Spring Boot).

### Core Configuration
Core configuration defines global settings like locking, transaction strategies, metadata, and more.

#### Key Settings
- **Lock Configuration**:
    - `lockAcquiredForMillis`: Duration the lock remains valid (default: 1 minute).
    - `lockQuitTryingAfterMillis`: Timeout for acquiring a lock (default: 3 minutes).
    - `lockTryFrequencyMillis`: Frequency of lock retry attempts (default: 1 second).
    - `throwExceptionIfCannotObtainLock`: Fail if unable to acquire a lock (default: true).
    - `enableRefreshDaemon`: Enable a daemon thread to refresh the lock (default: true).
- **Transaction Strategy**:
    - `CHANGE_UNIT`: Each change unit runs in an independent transaction.
    - `EXECUTION`: The entire execution is wrapped in a single transaction.
- **Metadata**: Custom user-defined data attached to migrations.
- **MongockImporterConfiguration**:
    - `source`: Specifies the source for legacy migrations.
    - `enabled`: Indicates whether the legacy migration importer is active.
- **Stage Configuration**:
    - `stages`: List of stages defining execution order and code packages.

#### Additional Properties
- **Service Settings**:
    - `serviceIdentifier`: Identifier for the service running Flamingock.
- **Version Control**:
    - `startSystemVersion`: System version to start migration (default: 0).
    - `endSystemVersion`: System version to stop migration (default: Integer.MAX_VALUE).
- **Feature Toggles**:
    - `trackIgnored`: Whether to track ignored changes (default: false).
    - `enabled`: Enables or disables Flamingock (default: true).

### Cloud Configuration
The recommended approach for Flamingock is to use its cloud-managed service. This setup provides advanced features, scalability, and simplified operations.

#### Key Fields
- `apiToken`: Token for authenticating with the Flamingock cloud.
- `host`: Flamingock cloud endpoint (default: `https://runner.flamingock.io`).
- `service`: Unique service identifier.
- `environment`: Deployment environment (e.g., `staging`, `production`).

### Local Configuration
For scenarios where cloud connectivity isn’t viable, Flamingock supports using a local database (MongoDB or DynamoDB) to store metadata. This setup lacks advanced features like distributed locking and audit querying.

#### Key Fields
- `transactionDisabled`: Disable transactions for local storage (default: false).

## Examples for Common Configurations

### Standalone 
**YAML Configuration**:
```yaml
flamingock:
  apiToken: "your-api-token"
  service: "your-service-name"
  environment: "production"
  lockAcquiredForMillis: 60000
  lockQuitTryingAfterMillis: 180000
  lockTryFrequencyMillis: 1000
  stages:
    - name: "stage1"
      codePackages:
        - "io.flamingock.examples"
```

**Builder Configuration**:
```java
public class MysqlStandaloneApplication {
    public void run() {
        FlamingockStandalone
            .cloud()
            .setApiToken("your-api-token")
            .setEnvironment("production")
            .setService("your-service-name")
            .setLockAcquiredForMillis(60000L)
            .setLockQuitTryingAfterMillis(180000L)
            .setLockTryFrequencyMillis(1000L)
            .addStage(new Stage("stage1").addCodePackage("io.flamingock.examples"))
            .build()
            .run();
    }
}
```

### Spring Boot 
**YAML Configuration**:
```yaml
flamingock:
  apiToken: "your-api-token"
  service: "your-service-name"
  environment: "production"
  lockAcquiredForMillis: 60000
  lockQuitTryingAfterMillis: 180000
  lockTryFrequencyMillis: 1000
  runnerType: ApplicationRunner
  stages:
    - name: "stage1"
      codePackages:
        - "io.flamingock.examples"
```

**Builder Configuration**:
```java
public class SpringBootApp {
    public void run(ApplicationContext context) {
        FlamingockSpringboot
            .cloud()
            .setApiToken("your-api-token")
            .setEnvironment("production")
            .setService("your-service-name")
            .setSpringContext(context)
            .setLockAcquiredForMillis(60000L)
            .setLockQuitTryingAfterMillis(180000L)
            .setLockTryFrequencyMillis(1000L)
            .addStage(new Stage("stage1").addCodePackage("io.flamingock.examples"))
            .build()
            .run();
    }
}
```
