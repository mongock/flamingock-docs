---
title: FAQ
date: 2014-04-18 11:30:00 
permalink: /v1/faq/index.html
upperCase: true
eleventyNavigation:
  version: v1
  root: true
  order: 100
---
<h1 class="title">FAQ</h1>


[[TOC]]

## Why we have added the ChangeUnit annotation and deprecated ChangeLog
Flamingock team is aware of the implications of this change, that's why is totally backward compatible. All the old code related to `@ChangeLog` and `@ChangeSet` will remain compatible.

However, the Flamingock team has identified that the old changelog structure starts to struggle handling new features. The configuration was cumbersome and a bit more complex that needed.

This is the reason we have added the `@ChangeUnit`. A more practical version of the `@ChangeLog`, which can be see as a changeLog, wich a unique and mandatory changeSet(`@Execution`) that also requires a complementary method `@RollbackExecution`.

More information in the [migration section](/v1/migration).


## I've migrated to Flamingock 5 and there are some existing @ChangeLog classes in my project. Should I remove or change them?
No. Your old changeLogs should remain untouched. Although deprecated, they will remain in the code for backward compatibility. 

For more information about the recommended approach, please visit [this section](/v1/migration).

## What happens if a changeUnit fails
Flamingock always try to rollback the failed changes. In a transactional environment, Flamingock relies on the database transactional mechanism to rollback the migration changes(`@Execution` method) as well as the mongock metadata associated to the change. In summary it would be like that change was never started. In a non-transactional environment, Flamingock manually tries to rollback the migration change by executing the `@RollbackExecution` method and marks the change entry as `ROLLED_BACK` in the database. Please notice that although Flamingock will try its best to achieve this, it's not guaranteed.

Once the rollback operation is performed, Flamingock will abort the migration and throw an exception. The next time Flamingock is executed will carry on from the failed changeUnit. You need to understand that if the changeUnit keep failing, Flamingock will keep aborting. In an self-deployed infrastructure like Kubernetes this potentially means get into an infinite loop.  

## Should I implement the @RollbackExecution method in transactional environments?
Yes, we highly recommend to implement the `@RollbackExecution` method. 

The main reason for this is that some other operations like undo, rely on this method to work. However it's a very good practice as it provides a robust system that is less affected when moving to non-transactional environments. 


## Can Flamingock be used in applications deployed in Kubernetes?
Yes. Actually the main use of Flamingock is as part of the application start up, shipping together code and data changes, providing safer migrations in distributed systems.

## Does the CLI change something in my application?
No. The CLI just takes from your application what is needed to run the migration.

With Flamingock standalone, it takes the `FlamingockBuilderProvider` implementations and retrieves the builder. For Springboot, by default it loads the entire Springboot context, but you can filter which configuration class to load by annotating the main class with `@FlamingockCliConfiguration`. For more information please visit the [cli page](/v1/cli).

## Until which Spring data version is FlamingockTemplate compatible with?
4.2.3. But you can use more recent version of Spring Data. FlamingockTemplate won't reflect the new API, but it won't complain.

## I have some references to FlamingockTemplate in my project and I've upgraded to version 5, should I remove them?
No, even if you use a more updated version of Spring Data. You just use Spring MongoTemplate for now on.

## What's the difference between undo and rollback?
**Rollback** is the act of reverting a change after failing at execution time. On the other hand **undo** is the act of reverting a change some time after being successfully executed.

## Why does Flamingock provide the mongodb-reactive-driver, if it is synchronous by definition?
Before this driver, when Flamingock was used in a reactive project, developers were forced to import the synchronous MongoDB java driver(or Spring Data driver) just to be used in Flamingock migration. While this is not bad, some teams prefer to avoid it. With this reactive driver, this is not the case anymore. 

## Can I use the mongodb-reactive-driver with a Spring Data project?
Yes, you can! However, Flamingock doesn't provide a specific driver for reactive Spring Data, so developers are forced to use the MongoDB Java Reactive Streams driver in their changUnits. You can see an example [here](https://github.com/mongock/mongock-examples/tree/master/mongodb/springboot-reactive)

## Why does Flamingock use a code-first approach for its change units?
In Flamingock we find multiple benefits when using this approach. However, one of the main reasons Flamingock encourages a code-first 
approach is the fact that NoSql databases don't share a standard like SQL databases. In contrast, these NoSql databases normally provide 
a driver for the given programming language. 

However, said that, depending on the driver, Flamingock may also provide other approaches if it satisfies our technical 
requirements(performance, security, etc.)


<!--## My migrations take long and it impacts my startup time... what should I do?

## What if we have an environmment with the latest changes and others out of synch?
## How manage HA in changes-> two step changes-->