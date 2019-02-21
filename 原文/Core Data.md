Framework

# Core Data

Manage object graphs and object lifecycle, including persistence.

Language

- Swift
- [Objective-C](apple-reference-documentation://cccoredata)

SDKs

- iOS 3.0+
- macOS 10.4+
- tvOS 9.0+
- watchOS 2.0+

Framework

- Core Data

On This Page

- [Overview](apple-reference-documentation://cscoredata#overview)
- [Topics](apple-reference-documentation://cscoredata#topics)

------

## Overview

Use Core Data to manage the model layer objects in your application. It provides generalized and automated solutions to common tasks associated with object lifecycle and object graph management, including persistence.

Core Data drastically decreases the amount of code you write to support the model layer. This is primarily due to the following built-in features that you do not have to implement, test, or optimize:

- Change tracking and built-in management of undo and redo beyond basic text editing. 
- Maintenance of change propagation, including maintaining the consistency of relationships among objects. 
- Lazy loading of objects, partially materialized futures (faulting), and copy-on-write data sharing to reduce overhead. 
- Automatic validation of property values. Managed objects extend the standard key-value coding validation methods to ensure that individual values lie within acceptable ranges, so that combinations of values make sense. 
- Schema migration tools that simplify schema changes and allow you to perform efficient in-place schema migration. 
- Optional integration with the application’s controller layer to support user interface synchronization. 
- Grouping, filtering, and organizing of data in memory and in the user interface. 
- Automatic support for storing objects in external data repositories. 
- Sophisticated query compilation. Instead of writing SQL, you can create complex queries by associating an [`NSPredicate`](apple-reference-documentation://hsgeUN8BnB) object with a fetch request. 
- Version tracking and optimistic locking to support automatic multiwriter conflict resolution. 

## Topics

### First Steps

 Making Core Data Your Model Layer

Learn how Core Data decreases the amount of code you need to develop and access your app's model layer.

```
class NSManagedObject
```

A generic class that implements all the basic behavior required of a Core Data model object.

### Fetch Requests

Core Data retrieves persisted data to be used by your app.

```
class NSFetchRequest
```

A description of search criteria used to retrieve data from a persistent store.

```
class NSAsynchronousFetchRequest
```

A fetch request that retrieves results asynchronously and supports progress notification.

```
class NSAsynchronousFetchResult
```

A fetch result object that encompasses the response from an executed asynchronous fetch request.

```
class NSFetchedResultsController
```

A controller that you use to manage the results of a Core Data fetch request and display data to the user.

### Core Data Stack

The Core Data stack mediates between the objects in your app and external data stores. It handles all interactions with the external data stores so that your application can focus on its business logic.

```
class NSPersistentContainer
```

A container that encapsulates the Core Data stack in your application.

```
class NSManagedObjectContext
```

An object representing a single object space or scratch pad that you use to fetch, create, and save managed objects.

```
class NSPersistentStoreCoordinator
```

A coordinator that associates persistent stores with a model (or a configuration of a model) and that mediates between the persistent stores and the managed object contexts.

```
class NSManagedObjectModel
```

An object that describes a schema—a collection of entities (data models) that you use in your application.

### Data Migration

Core Data has built-in data migration tools to help keep your app's data up to date with the current data model.

 Using Lightweight Migration

Request lightweight (automatic) migration to update your data model to match changes in your app.

 Heavyweight Migration

Use heavyweight (manual) migration in rare cases when changes to the data model exceed the capabilities of lightweight migration.

### Background Tasks

 Using Core Data in the Background

Use Core Data in both a single-threaded and multithreaded app.

 Conflict Resolution

Detect and resolve conflicts that occur when data is changed on multiple threads.

 Batch Processing

Use batch processes to manage large data changes.

### Reference

 Core Data Constants

This document describes the constants defined in the Core Data framework and not described in a document for an individual class.