Framework

# Core Data

Persist or cache data and support undo on a single device.

## Overview

Use Core Data to save your application’s permanent data for offline use, to cache temporary data, and to add undo functionality to your app on a single device.

Through Core Data’s Data Model editor, you define your data’s types and relationships, and generate respective class definitions. Core Data can then manage object instances at runtime to provide the following features.

### Persistence

Core Data abstracts the details of mapping your objects to a store, making it easy to save data from Swift and Objective-C without administering a database directly.

![Flow diagram showing an app saving data to and loading data from a persistent store.](https://ws1.sinaimg.cn/large/006tKfTcly1g0pn20j4wwj30c40acjrh.jpg)

### Undo and Redo of Individual or Batched Changes

Core Data’s undo manager tracks changes and can roll them back individually, in groups, or all at once, making it easy to add undo and redo support to your app.

![Figure showing a shake to undo gesture causing an element to be removed from a list.](https://ws2.sinaimg.cn/large/006tKfTcly1g0pn2q7pkbj30up0bbt97.jpg)

### Background Data Tasks

Perform potentially UI-blocking data tasks, like parsing JSON into objects, in the background. You can then cache or store the results to reduce server roundtrips.

![Flow diagram showing data from an endpoint populating objects in the background before updating the UI.](https://ws4.sinaimg.cn/large/006tKfTcly1g0pn2xnihqj30eg0axq36.jpg)



### View Synchronization

Core Data also helps keep your views and data synchronized by providing data sources for table and collection views.

### Versioning and Migration

Core Data includes mechanisms for versioning your data model and migrating user data as your app evolves.

## Topics

### First Steps

Creating a Core Data Model

Create a data model file to contain your app’s object structure.

Setting Up a Core Data Stack

Set up the classes that manage and persist your app’s objects.

Core Data Stack

Manage and persist your app’s model layer.

### Data Modeling

Modeling Data

Configure the data model file to contain your app’s object graph.

Core Data Model

Describe your app’s object structure.

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

A controller that you use to manage the results of a Core Data fetch request and to display data to the user.

### Data Migration

Core Data has built-in data migration tools to help keep your app's data up to date with the current data model.

Using Lightweight Migration

Request lightweight (automatic) migration to update your data model to match changes in your app.

Heavyweight Migration

Use heavyweight (manual) migration in rare cases when changes to the data model exceed the capabilities of lightweight migration.

### Background Tasks

Using Core Data in the Background

Use Core Data in both a single-threaded and multithreaded app.

Loading and Displaying a Large Data Feed

Consume data in the background, and lower memory usage by batching imports and preventing duplicate records in the Core Data store.

Conflict Resolution

Detect and resolve conflicts that occur when data is changed on multiple threads.

Batch Processing

Use batch processes to manage large data changes.

### Reference

Core Data Constants

This document describes the constants defined in the Core Data framework and not described in a document for an individual class.