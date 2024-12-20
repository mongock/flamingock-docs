---
title: Get started
date: 2014-04-18 11:30:00 
permalink: /v1/get-started/index.html
eleventyNavigation:
  version: v1
  root: true
  order: 20
---
<h1 class="title">Get Started</h1>


[[TOC]]

## Steps

Flamingock provides different runners, from the standalone runner(vanilla version) to Springboot, and other frameworks. In this section will show how to use Flamingock with Springboot.

Carryng on with our **client-service** example in [what is Flamingock?](/v1/what-is-mongock), lets start working with Flamingock!

### 1- Add Flamingock bom to your Pom file 
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.mongock</groupId>
            <artifactId>mongock-bom</artifactId>
            <version>LAST_RELEASE_VERSION_5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
### 2- Add the maven dependency for the runner
[Runner options](/v1/runner/#runner-options)
```xml
<dependency>
  <groupId>io.mongock</groupId>
  <artifactId>mongock-springboot</artifactId>
</dependency>
```

### 3- Add the maven dependency for the driver
[Driver options](/v1/driver/#driver-options)
```xml
<dependency>
  <groupId>io.mongock</groupId>
  <artifactId>mongodb-springdata-v3-driver</artifactId>
</dependency>
```
Flamingock is not intrusive, relies the driver library's version on the developer. These libraries are injected with **scope provided**.


### 4- Create your migration script/class

Note that by default, a ChangeUnit is wrapped in a transaction(natively or by using the database support or manually, when transactions are not supported).

For more information visit the [migration](/v1/migration/) and [transaction section](/v1/features/transactions/)

```java
package io.mongock.examples.migration;

import io.mongock.api.annotations.Execution;
import io.mongock.api.annotations.ChangeUnit;
import io.mongock.api.annotations.RollbackExecution;

@ChangeUnit(id="client-initializer", order = "001", author = "mongock")
public class ClientInitializerChange {

  private final MongoTemplate mongoTemplate;
  private final ThirPartyService thirdPartyService;

  public ClientInitializerChange(MongoTemplate mongoTemplate,
                                 ThirPartyService thirdPartyService) {
    this.mongoTemplate = mongoTemplate;
    this.thirdPartyService = thirdPartyService;
  }

  /** This is the method with the migration code **/
  @Execution
  public void changeSet() {
    thirdPartyService.getData()
            .stream()
            .forEach(client -> mongoTemplate.save(client, CLIENTS_COLLECTION_NAME));
  }

  /**
   This method is mandatory even when transactions are enabled.
   They are used in the undo operation and any other scenario where transactions are not an option.
   However, note that when transactions are avialble and Flamingock need to rollback, this method is ignored.
   **/
  @RollbackExecution
  public void rollback() {
    mongoTemplate.deleteMany(new Document());
  }
}
```
### 5- Build the driver (only requried for builder approach)

Although all the drivers follow the same build pattern, they may slightly differ from each other. Please visit the specific [driver's page](/v1/driver) for more details. 

### 6- Driver extra configuration
This step is **NOT MANDATORY**, however for certain features, the driver may require some extra help. For example, in order to enable transactions with spring data, the transaction manager needs to be injected in the application context.

Plese visit the specific [driver's page](/v1/driver) for more details.

### 7- Build the runner
<p class="tipAlt">When using the builder approach, the driver needs to be injected to the runner by using the method: <b>setDriver</b></p>
<br />

There are two approaches when comes to build the Flamingock runner, the builder and the autoconfiguration approach. Visit the [runner builder](/v1/runner#build) for more information. 
<br /><br />
For this example, we use the autoconfiguration approach with Springboot.

#### Properties
```yaml
mongock:
  migration-scan-package:
    - io.mongock.examples.migration
```
#### Indicate spring to use Flamingock
This approach lies on the underlying framework to provide a smoothly experience. In this case, we take advantage of the Springboot annotations to tell Spring how to run Flamingock. However, this approach requires the Spring ApplicationContext and MongoTemplate to be injected in the Spring context.

```java
@EnableFlamingock
@SpringBootApplication
public class App {
  public static void main(String[] args) {
    new SpringApplicationBuilder().sources(App.class).run(args);
  }
}
```

### 8- Execute the runner
[Execute runner](/v1/runner#build)

When using the Springboot runner, you don't need to worry about the execution.  Flamingock takes care of it 😉


Congratulations! Our basic Flamingock setup is done. We just need to run our application and we should see something like this in our log.
```
2021-09-17 17:27:42.157  INFO 12878 --- [main] i.m.r.c.e.o.c.MigrationExecutorBase      : 
APPLIED - ChangeEntry{"id"="client-initializer", "author"="mongock", "class"="ClientInitializer", "method"="changeSet"}
```

--------------------------------------------------

## Examples

For code examples, visit the [resource page](/v1/resources)