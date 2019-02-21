ArticleMaking

# Core Data Your Model Layer

Learn how Core Data decreases the amount of code you need to develop and access your app's model layer.

Framework

- Core Data

------

## Overview

Core Data reduces the complexity of creating and maintaining the model layer of your app. By using Core Data to define your data structures, you can remove most of the repetitive code that is typically required.

You use instances of [`NSManagedObject`](apple-reference-documentation://hszrY8Gi1t) as data structures, and you define the relationships between those objects using the data model.

### Integrate Core Data into Your App

You perform the initial integration of Core Data into your app by either creating the traditional Core Data stack or by initializing a `NSPersistentContainer`. Both paths look for a managed object model in your app bundle and use that model to define the managed objects (data objects) that are available to your app.

Typically, Core Data is initialized during your app's startup; your user interface then gets access to Core Data. Because you start this initialization outside of your view hierarchy, it is recommended that you begin the process in your app's delegate.

### Initialize a Persistent Container

The `NSPersistentContainer` simplifies the creation of the Core Data stack and provides a convenient class for subclassing and adding custom convenience methods to Core Data. The following example shows the initialization of the persistent container:

```swift
let container = NSPersistentContainer(name: "DataModel")
container.loadPersistentStores(completionHandler: { (description, error) in
    if let error = error {
        fatalError("Unable to load persistent stores: \(error)")
    }
})
```

While the bulk of your app's interaction with Core Data will be through the `NSManagedObjectContext` that is contained within the `NSPersistentContainer`, it is recommended that you pass a reference to the container to your user interface.

Because the `NSPersistentContainer` is intended to be subclassed, it is also a convenient place to put Core Data-related functions such as functions that return subsets of data and calls to persist data to disk.

### Create the Core Data Stack

If you decide not to use an [`NSPersistentContainer`](apple-reference-documentation://hsuz0WAzCN) object, you need to initialize an instance of [`NSManagedObjectModel`](apple-reference-documentation://hs4xyi9b9z),  [`NSPersistentStoreCoordinator`](apple-reference-documentation://hsRdlxSPjF), and at least one instance of [`NSManagedObjectContext`](apple-reference-documentation://hseREZ0QHW).

The [`NSManagedObjectModel`](apple-reference-documentation://hs4xyi9b9z) is a programmatic representation of the managed object model that is created and maintained in the model editor of Xcode. To instantiate an object model, you pass in a URL that points to the location of the managed object model that was created in Xcode. The model file is typically part of your app bundle.

```swift
guard let modelURL = Bundle.main.url(forResource: "DataModel", withExtension: "momd") else {
    fatalError("failed to find data model")
}
guard let mom = NSManagedObjectModel(contentsOf: url) else {
    fatalError("Failed to create model from file: \(url)")
}
```

After you create the managed object model, you can associate the [`NSPersistentStoreCoordinator`](apple-reference-documentation://hsRdlxSPjF) with the model.

```swift
let psc = NSPersistentStoreCoordinator(managedObjectModel: mom)
```

If you want Core Data to persist your data model to disk, you will need to inform the [`NSPersistentStoreCoordinator`](apple-reference-documentation://hsRdlxSPjF) of where you want the file to reside and what format you want to use, as shown in the following example. There are advantages and disadvantages to each of the store types that are available. Refer to the [`NSPersistentStoreCoordinator`](apple-reference-documentation://hsRdlxSPjF) documentation for details on each store type.

```swift
let dirURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).last
let fileURL = URL(string: "DataModel.sql", relativeTo: dirURL)
do {
    try psc.addPersistentStore(ofType: NSSQLiteStoreType, configurationName: nil, at: fileURL, options: nil)
} catch {
    fatalError("Error configuring persistent store: \(error)")
}
```

Once the persistent store coordinator is instantiated, you can perform the final step in the creation of the Core Data stack, which is the initialization of an `NSManagedObjectContext`.

```swift
let moc = NSManagedObjectContext(concurrencyType: .mainQueueConcurrencyType)
moc.persistentStoreCoordinator = psc
```

The bulk of your app's interaction with Core Data will be with a [`NSManagedObjectContext`](apple-reference-documentation://hseREZ0QHW) object. Pass the managed object context by reference to your user interface.