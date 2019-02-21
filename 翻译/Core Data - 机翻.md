框架

# Core Data

管理对象图和对象生命周期，包括持久性。

语言

- Swift
- [Objective-C](apple-reference-documentation://cccoredata)

SDKs

- iOS 3.0+
- macOS 10.4+
- tvOS 9.0+
- watchOS 2.0+

框架

- Core Data

在本页面：

- [概览](apple-reference-documentation://cscoredata#overview)
- [话题](apple-reference-documentation://cscoredata#topics)

------

## 概览

使用Core Data管理应用程序中的模型层对象。它为与对象生命周期和对象图管理相关的常见任务提供通用和自动化解决方案，包括持久性。

Core Data大大减少了为支持模型层而编写的代码量。这主要是由于您不必实现，测试或优化以下内置功能：

- 更改基本文本编辑之外的撤消和重做的跟踪和内置管理。
- 维护变更传播，包括维护对象之间关系的一致性。
- 延迟加载对象，部分物化的期货（故障）和写时复制数据共享以减少开销。
- 自动验证属性值。托管对象扩展了标准键值编码验证方法，以确保各个值位于可接受的范围内，因此值的组合是有意义的。
- 模式迁移工具，可简化模式更改并允许您执行高效的就地模式迁移。
- 可选地与应用程序的控制器层集成以支持用户界面同步。
- 在内存和用户界面中对数据进行分组，过滤和组织。
- 自动支持在外部数据存储库中存储对象。
- 复杂的查询编译。您可以通过将 [`NSPredicate`](apple-reference-documentation://hsgeUN8BnB) 对象与提取请求相关联来创建复杂查询，而不是编写SQL。
- 版本跟踪和乐观锁定，以支持自动多写器冲突解决。

## 话题

### 第一步

 [^文章] 将Core Data作为模型层

了解Core Data如何减少开发和访问应用模型层所需的代码量。

 [^Class]


```
class NSManagedObject
```

一个泛型类，实现Core Data模型对象所需的所有基本行为。

### 获取请求

Core Data检索应用程序使用的持久数据。

```
class NSFetchRequest
```

用于从持久性存储中检索数据的搜索条件的描述。

```
class NSAsynchronousFetchRequest
```

一种获取请求，它以异步方式检索结果并支持进度通知。

```
class NSAsynchronousFetchResult
```

一个获取结果对象，它包含来自执行的异步获取请求的响应。

```
class NSFetchedResultsController
```

用于管理Core Data获取请求结果并向用户显示数据的控制器。

### Core Data 堆栈

Core Data堆栈介于应用程序中的对象和外部数据存储之间。它处理与外部数据存储的所有交互，以便您的应用程序可以专注于其业务逻辑。

```
class NSPersistentContainer
```

在应用程序中封装Core Data堆栈的容器。

```
class NSManagedObjectContext
```

表示用于获取，创建和保存托管对象的单个对象空间或便笺簿的对象。

```
class NSPersistentStoreCoordinator
```

一种协调器，它将持久性存储与模型（或模型的配置）相关联，并在持久性存储和托管对象上下文之间进行调解。

```
class NSManagedObjectModel
```

描述模式的对象 - 您在应用程序中使用的实体集合（数据模型）。

### Data Migration

Core Data具有内置的数据迁移工具，可帮助您使用当前数据模型保持应用程序数据的最新状态。

 使用轻量级迁移

请求轻量级 (自动) 迁移以更新数据模型, 使其与应用中的更改相匹配。

 重量级迁移

在极少数情况下, 当对数据模型的更改超过轻量级迁移的功能时, 请使用重量级 (手动) 迁移。

### 后台任务

 在后台使用Core Data

在后台使用Core Data

 冲突解决

检测并解决在多个线程上更改数据时发生的冲突。

 批处理

使用批处理管理大型数据更改。

### 参考

 Core Data常量

本文档描述了在Core Data框架中定义的常量, 而不是在单个类的文档中描述的常量。


[^文章]: 这是一篇文章。


[^Class]: 这是类。