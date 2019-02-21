文章

# 使Core Data成为您的模型层

了解 core data 如何减少开发和访问应用模型层所需的代码量。

框架

- Core Data

------

## 概述

Core Data降低了创建和维护应用的模型层的复杂性。通过使用 core data 定义数据结构, 您可以删除通常需要的大多数重复代码。

使用 [` NSManagedObject`](apple-reference-documentation://hszrY8Gi1t) 的实例作为数据结构, 并使用数据模型定义这些对象之间的关系。

### 将Core Data集成到您的应用中

通过创建传统的Core Data堆栈或初始化 `NSPersistentContainer`, 可以将Core Data初步集成到应用中。这两个路径都在应用包中查找托管对象模型, 并使用该模型定义可供你的应用使用的托管对象 (数据对象)。

通常, Core Data是在应用启动过程中初始化的。您的用户界面, 然后获得对Core Data的访问权限。由于在视图层次结构之外启动此初始化, 因此建议您在应用的委托中开始该过程。

### 初始化持久容器

`NSPersistentContainer` 简化了Core Data堆栈的创建, 并为对Core Data进行子类化和添加自定义便利方法提供了一个方便的类。下面的示例演示持久性容器的初始化:

```swift
let container = NSPersistentContainer(name: "DataModel")
container.loadPersistentStores(completionHandler: { (description, error) in
    if let error = error {
        fatalError("Unable to load persistent stores: \(error)")
    }
})
```

虽然你的应用与Core Data的大部分交互将通过`NSPersistentContainer`中包含的 `NSManagedObjectContext` , 但建议您将对容器的引用传递给用户界面。

由于 `NSPersistentContainer` 是打算子类的, 因此它也是放置Core Data相关函数 (如返回数据子集的函数和将数据保留到磁盘的调用) 的方便位置。

### 创建Core Data堆栈

如果您决定不使用 [`NSPersistentContainer`](apple-reference-documentation://hsuz0WAzCN) 对象, 则需要初始化 [`NSManagedObjectModel`](apple-reference-documentation://hs4xyi9b9z) 的实例, [`nspersistentstreecorecorecoriso`] (apple-reference-documentation://hsRdlxSPjF), 以及至少一个实例 [`NSManagedObjectContext`](apple-reference-documentation://hseREZ0QHW)。

[`NSManagedObjectModel`](apple-reference-documentation://hs4xyi9b9z) 是在 xcode 的模型编辑器中创建和维护的托管对象模型的编程表示形式。要实例化对象模型, 请传入指向在 xcode 中创建的托管对象模型的位置的 url。模型文件通常是应用包的一部分。

```swift
guard let modelURL = Bundle.main.url(forResource: "DataModel", withExtension: "momd") else {
    fatalError("failed to find data model")
}
guard let mom = NSManagedObjectModel(contentsOf: url) else {
    fatalError("Failed to create model from file: \(url)")
}
```

创建托管对象模型后, 可以将 [`NSPersistentStoreCoordinator`](apple-reference-documentation://hsRdlxSPjF) 与模型相关联。

```swift
let psc = NSPersistentStoreCoordinator(managedObjectModel: mom)
```

如果您希望 core data 将数据模型保留到磁盘上, 则需要通知[`NSPersistentStoreCoordinator`](apple-reference-documentation://hsRdlxSPjF) 您希望文件驻留的位置以及要使用的格式, 如下所示例子。每种可用的商店类型都有优点和缺点。有关每种商店类型的详细信息, 请参阅 [`NSPersistentStoreCoordinator`](apple-reference-documentation://hsRdlxSPjF) 文档。

```swift
let dirURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).last
let fileURL = URL(string: "DataModel.sql", relativeTo: dirURL)
do {
    try psc.addPersistentStore(ofType: NSSQLiteStoreType, configurationName: nil, at: fileURL, options: nil)
} catch {
    fatalError("Error configuring persistent store: \(error)")
}
```

实例化持久存储协调器后, 您可以执行创建Core Data堆栈的最后一步, 即初始化的`NSManagedObjectContext`。

```swift
let moc = NSManagedObjectContext(concurrencyType: .mainQueueConcurrencyType)
moc.persistentStoreCoordinator = psc
```

应用与Core Data的大部分交互将与[`NSManagedObjectContext`](apple-reference-documentation://hseREZ0QHW) 对象进行交互。通过引用用户界面传递托管对象上下文。