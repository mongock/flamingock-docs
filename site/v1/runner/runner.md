---
title: Runner
date: 2014-04-18 11:30:00 
permalink: /v1/runner/index.html
eleventyNavigation:
  version: v1
  root: true
  key: runner 
  title: Runner
  order: 40
---
<h1 class="title">Flamingock Runner</h1>




[[TOC]]

## Introduction
This page explains the process of using the runner, configuration properties, etc. You can find a more practical guide in each runner page.

As mentioned in the [technical overview](/technical-overview#runner), the runner is the orchestator that handles the environment and it's responsible of the process.


The natural steps you should follow are:
- Decide the right runner for your project from the [available options](#runner-options).
- Import the maven dependencies(which can be found in each runner page).
- [Build the runner](#build).
- [Execute it](#execution).
______________________________________

## Runner options
There are specific runners for certain environments, like frameworks, etc.

Currently Flamingock provides: 
- [Flamingock standalone runner](/v1/runner/standalone/) 
- [Flamingock springboot runner](/v1/runner/springboot/) 
<!--- [Flamingock micronaut runner](/runner/micronaut/) -->
______________________________________

## Build
Flamingock offers two build approaches:

- **Builder approach:** The user manually configures and executes the runner by using the runner builder. Normally by setter methods. Regardless the type of runner, all of them provides an static method `builder()` which returns the builder.

- **Autoconfiguration:** Flamingock automatically configures and executes the runner by taking the configuration from properties file and taking advantage of the underlying framework. However, It still uses the builder behind the scenes, but it's transparent to the user.

<p class="noteAlt">Once the runner is built, it can only run once</p>

______________________________________

## Configuration

<p class="tipAlt">When using properties file, you need to add the prefix <b>`mongock.`.</b></p>
<p class="success">You can find more additional properties in each runner page.</p>

| Property                  | type | Description                                                                                  | Type                | Default value |
| :------------------------:|:---------------------------------------------------------------------------------------------|---------------------|:-----------:|:-------------:|
| **driver**                | component | The Flamingock driver. This parameter can only be passed programatically. When opting for **autoconfiguration**, Flamingock builds the driver and injects it to the runner. | ConnectionDriver | Mandatory |  
| **migrationScanPackage**  | property | The list of migration(changeUnits and changeLogs) classes and/or packages where they are stored. | List< String >      |Mandatory |  
| **metadata**              | property | Custom data attached to the migration. It will be added to change entry in the mongock table/collection. | Map<String, Object> | null |  
| **startSystemVersion**    | property | System version to start with.                                                                 | String              | `0` |  
| **endSystemVersions**     | property | System version to end with.                                                                   | String              | MAX_VALUE |  
| **trackIgnored**          | property | Specifies if an ignored changeUnit(already executed for example) should be track in the Flamingock table/collection with status IGNORED. | boolean | `false` |  
| **enabled**               | property | If false, will disable Flamingock execution.| boolean |NO          | `true` |  
| **serviceIdentifier**     | property | Application/service instance's indentifier. | String | null|
| **defaultMigrationAuthor**| property | Author field is not mandatory in ChangeUnit. The field `author` in this annotation is optional. However for backward compatibility it's still required. If it's provided in the ChangeUnit annotation, this value is taken. If not, Flamingock will look at this property. If not provided, the default value is provided. | String | `default_author` |
| **throwExceptionIfCannotObtainLock**| property | Flamingock will throw FlamingockException if lock can not be obtained. Builder method `dontFailIfCannotAcquireLock` to turn it to false. | boolean | long | `true` |  
| **transactional**              | property | Indicates the whether transaction is enabled. For backward compatibility, this property is not mandatory but it will in coming versions. It works together with the driver under the following agreement: Transactions are enabled only if the driver is transactionable and this field is `true` or not provided. If it's `false`, transactions are disabled and will throw an exception if this field is `true` and the driver is not transactionable. To understand what _transactionable_ means in the context of the driver and how to make a driver transactionable, visit the section [driver](/v1/driver/).      | boolean | null |  
| **transactionStrategy**   | property | Dictates the transaction strategy. `CHANGE_UNIT` means each changeUnit(applied to deprecated changeLog as well) is wrapped in an independent transaction.`EXECUTION` strategy means that Flamingock will wrap all the changeUnits in a single transaction. Note that Flamingock highly recommend the default value, `CHANGE_UNIT`, as the `EXECUTION` strategy is unnatural and, unless it's really designed for it, it can cause some troubles along the way. | String | `CHANGE_UNIT` |  
______________________________________

## Execution
Although each builder may provide additional options to build the runner, all of them share the basic method `buildRunner()`, which returns a `FlamingockRunner` instance. This interface provides multiple methods, one of them is `execute()`, which starts the migration process. This is the natural way to run Flamingock when using the standalone runner. 

On the other hand, when opting for autoconfiguration, the user doesn't need to worry about the execution, Flamingock, with the help of the framework, takes care of it.
