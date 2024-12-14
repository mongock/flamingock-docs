---
title: Testing  
date: 2022-05-02 11:30:00 
permalink: /v5/testing/index.html
eleventyNavigation:
  version: v5
  root: true
  order: 80
---
<h1 class="title">Testing</h1>

[[TOC]]

# Introduction
This sections exaplains the different levels of testing(unit and integration tests) that can(and should) be applied to a Mongock migration and the tools Mongock provides for it.

# Unit testing
Unit tests are a good starting point to ensure the correctness of a migration. With this mechanism the ChangeUnits can be validated in isolation, covering all the changeUnit's methods: `Execution`, `RollbackExecution`, `BeforeExecution` and `RollbackBeforeExecution`


Mongock doesn't provide any speicific tool for this, but we illustrate how to do it  [in our example project](https://github.com/mongock/mongock-examples/tree/master/mongodb/springboot-quickstart)

A unit test for a change unit looks like this:

<p class="text-center">
    <img src="/images/changeUnit-unit-test.png" alt="ChangeUnit unit test">
</p>


# Integration test
Once you are confident that the ChangeUnits are tested in isolation, we can increase the testing robustness by adding integration tests. The intent with Integration tests is to test the entire migration suite with your application context and validate the expected database results.

This is a more complex level of testing as it requires to simulate the application context and implies the integration of the different components within the application. But it's probably the most important level of testing to ensure the correctness of the migration.

To see an example, please see [our example project](https://github.com/mongock/mongock-examples/tree/master/mongodb/springboot-quickstart)


## Integration test with Springboot Runner with Junit5
Mongock provides some useful classes making testing easier. In summary, you need to create your test class extending `MongockSpringbootJUnit5IntegrationTestBase`, which provides the following
- **BeforeEach method(automatically called):** Resets mongock to allow re-utilization(not recommended in production) and build the runner
- **AfterEach method(automatically called):** Cleans both Mongock repositories(lock and migration) 
- **Dependency injections:** It ensures the required dependencies(Mongock builder, connectionDriver, etc.) are injected 
- **executeMongock() method:** To perform the Mongock migration
- **@TestPropertySource(properties = {"mongock.runner-type=NONE"}):** To prevent Mongock from injecting(and automatically executing) the Mongock runner bean. This is important to allow multiple Mongock runner's executions.

Please follow these steps...
### 1. Import the `mongock-springboot-junit5` dependency to your project
Assuming you have imported already `mongock-springboot` to you project, you only need to add
```xml
        <dependency>
            <groupId>io.mongock</groupId>
            <artifactId>mongock-springboot-junit5</artifactId>
            <scope>test</scope>
        </dependency>
```

### 2. Add the additional dependencies to your project
You probably need `Springboot starter test`, `JUnit5`, `Testcontainers`...

### 3. Database initialization. 

Although there are multiple ways of doing this, we present what we think provides a good balance easy-flexible
<p class="text-center">
    <img src="/images/integration-test-springboot-junit5-db-initialization.png" alt="ChangeUnit unit test">
</p>

### 4. Create the test class extending the `MongockSpringbootJUnit5IntegrationTestBase`

This class, in addition to extending `MongockSpringbootJUnit5IntegrationTestBase`, it should also bring the database initialization and the application environment.
<br /><br />
This is an example.
<p class="text-center">
    <img src="/images/integration-test-springboot-junit5-test-class.png" alt="Integration test with JUnit5">
</p>



## Integration test with Springboot Runner WITHOUT Junit5

In this case Mongock provides pretty much the same than in the JUnit5 case, with the exception of the `before` and `after` methods, what forces you to make these calls explicitly.

Based on the previous scenario, the relevant modifications are
1. Import the dependency `mongock-springboot-test` instead `mongock-springboot-junit5`
1. Extend from `MongockSpringbootIntegrationTestBase` instead `MongockSpringbootJUnit5IntegrationTestBase`
2. Explicitly call the methods `super.mongockBeforeEach()` and `super.mongockAfterEach()`

The test class should look like this
<p class="text-center">
    <img src="/images/integration-test-springboot-test-class.png" alt="Integration test without JUnit5">
</p>


## Integration test with Standalone Runner
The standalone runner provides more control over the process, allowing you to implement integration tests without the need of additional support.

You need to take into account the following 

1. Mongock Runner cannot be execute multiple times, so you need to build a new runner instance and execute it in every test execution. 


2. ConnectionDriver cannot be reused, meaning you need to create a new ConnectionDriver instance in every test execution and provide it to the Mongock builder

3. If you are sharing the same database for multiple tests, make sure you clean the database, in case you want to start fresh each test case.



