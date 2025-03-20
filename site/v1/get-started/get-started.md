---
title: Get started
date: 2014-04-18 11:30:00 
permalink: /v1/get-started/index.html
eleventyNavigation:
  version: v1
  root: true
  order: 10
---

<h1 class="title">Get Started</h1>

[[TOC]]

## Overview

Flamingock provides various runners, from the standalone runner (vanilla version) to Spring Boot and other frameworks.
This section explains how to get started with Flamingock using the standalone with the builder approach, continuing from the **client-service** example in [What is Flamingock?](/v1/what-is-mongock).

Let's dive in and start working with Flamingock!

### Prerequisites

Before getting started, ensure you have the following:

- **Java 8+**
- **Maven** or **Gradle** (recent versions)
- A **JVM project** based on either Maven or Gradle

### 1. Create a Flamingock account
If you don't have an account, [create a free account here](/v1/cloud/signup). **It takes just one minute!**  
After registration, you'll receive:
- An api token
- A service name
- An environment name

### 2. Add Flamingock's Core Dependency

To get started, include Flamingock's core dependency in your `build.gradle` or `pom.xml`.

For **Gradle**:

```kotlin
implementation("io.flamingock:flamingock-core:$flamingockLatestVersion")
```

### 3. Create Your First ChangeUnit
In this step, you'll define a `ChangeUnit` for MySQL. A `ChangeUnit` encapsulates a set of changes (like creating a table) and provides rollback logic.
This demonstrates the code approach for creating a `ChangeUnit`, which is always available.
However, for many technologies like MySQL, a declarative approach is also provided.

For declarative approach, visit the [[Templates section](/v1/templates)].

Example programmatic `ChangeUnit`:

```java
package io.flamingock.examples.mysql.standalone.changes;

import io.flamingock.core.api.annotations.ChangeUnit;
import io.flamingock.core.api.annotations.Execution;
import io.flamingock.core.api.annotations.RollbackExecution;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

@ChangeUnit(id = "create-collection", order = "1", transactional = false)
public class _1_createClientsTable {

    @Execution
    public void execution(Connection connection) throws SQLException {
        try (Statement statement = connection.createStatement()) {
            statement.executeUpdate("CREATE TABLE Clients (\n" +
                    "            ClientID int,\n" +
                    "            LastName varchar(255),\n" +
                    "            FirstName varchar(255),\n" +
                    "            Address varchar(255),\n" +
                    "            City varchar(255)\n" +
                    "        )");
        }
    }
    
    /**
     This method is optional, even when the changeUnit is transactional. 
     It is primarily used during the undo operation and in scenarios where transactions are unavailable. 
     However, if the changeUnit is transactional and Flamingock needs to perform a rollback (not to be confused with the undo operation), 
     this method will be ignored.
     **/
    @RollbackExecution
    public void rollback(Connection connection) throws SQLException {
        try (Statement statement = connection.createStatement()) {
            statement.executeUpdate("DROP TABLE IF EXISTS Clients");
        }
    }
}
```

For declarative approach, visit the [[Templates section](/v1/templates)].

### 4. Build the Runner

Flamingock provides two main approaches for building the runner: the **builder approach** and the **autoconfiguration approach**.

This example uses the **builder approach** to configure the runner with the necessary settings.

Example runner configuration:

```java
public class MysqlStandaloneApplication {
    public static void main(String[] args) throws SQLException {
        try (Connection connection = getConnection();
             CloudTransactioner cloudTransactioner = new SqlCloudTransactioner(connection)
                     .setDialect(SqlDialect.MYSQL)) {
            Runner runner = FlamingockStandalone.cloud()
                    .setApiToken(APITOKEN_FROM_STEP_1)
                    .setEnvironment(ENVIRONMENT_FROM_STEP_1)
                    .setService(SERVICE_FROM_STEP_1)
                    .setCloudTransactioner(cloudTransactioner)
                    .addStage(new Stage("mysql_stage").addFileDirectory("flamingock/stage1"))
                    .addDependency(Connection.class, getConnection())
                    .build();
        }
        // application's code here
    }
}
```

For more details, see the [Runner Builder section](/v1/runner#build).

### 5. Execute the Runner

Once your runner is built, simply execute it by calling the `run()` method.

```java
runner.run();
```

### Logs Output

When you successfully run Flamingock, the following logs confirm the changes were applied:

```
[main] INFO io.flamingock.core.runner.PipelineRunner - Finished Flamingock process successfully
Stage: mysql_stage
	1) id: create-collection 
		[class: io.flamingock.examples.mysql.standalone.changes._1_createClientsTable]
		Started		        ✅ - OK
		Executed	        ✅ - OK
		Audited[execution]	✅ - OK
...
```

### Local driver
Flamingock provides local drivers as an alternative to the cloud solution. These drivers allow you to store migration states directly in your own database. While this approach may fit specific scenarios, it lacks the scalability, advanced transaction handling, and centralized control offered by the cloud solution.

For more information on setting up local drivers, see the [Local Drivers docummentation](/v1/local_drivers)

---
### Next Steps

Congratulations! You've successfully set up Flamingock.  
To further explore features and advanced usage, visit the [Resources page](/v1/resources).
