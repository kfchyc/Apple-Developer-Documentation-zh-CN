# Table Views

在可自定义行的单个列中显示数据。

Framework

- UIKit

On This Page

- [Topics](https://developer.apple.com/documentation/uikit/views_and_controls/table_views#topics)
- [See Also](https://developer.apple.com/documentation/uikit/views_and_controls/table_views#see-also)

## Topics

### View

```
class UITableView
```

使用排列在单个列中的行显示数据的视图。

```
class UITableViewController
```

专门管理表视图的视图控制器。

```
class UITableViewFocusUpdateContext
```

提供与从一个视图到另一个视图的特定焦点更新相关的信息的上下文对象。

```
class UILocalizedIndexedCollation
```

提供具有节索引的表视图的数据组织、排序和本地化方法的对象。

### Rows

```
class UITableViewCell
```

表视图中的单元格。

```
class UISwipeActionsConfiguration
```

在表格的行上轻扫时要执行的操作集。

```
class UIContextualAction
```

用户轻扫表行时显示的操作。

```
class UITableViewRowAction
```

当用户在表行中水平滑动时显示的单个操作。

```
enum UIContextualAction.Style
```

指示应用于操作按钮的样式信息的常量。

{ 函数 } 创建自调整大小的表视图单元格

创建支持动态类型的表格视图单元格, 并使用系统间距约束来调整文本标签周围的间距。

### Headers and Footers

```
class UITableViewHeaderFooterView
```

可重复使用的视图, 可放置在表节的顶部或底部, 以显示该部分的其他信息。

### Adornments

```
class UIRefreshControl
```

可以启动滚动视图内容的刷新的标准控件。

### Drag and Drop

可以启动滚动视图内容的刷新的标准控件。

从表视图启动拖放和句柄下降。

```
protocol UITableViewDragDelegate
```

用于从表视图启动拖动的接口。

```
protocol UITableViewDropDelegate
```

用于处理的接口在表视图中下降。

```
protocol UITableViewDropCoordinator
```

用于协调与自定义下拉相关的操作与表视图的接口。

[`class UITableViewDropPlaceholder`](https://developer.apple.com/documentation/uikit/uitableviewdropplaceholder)

```
class UITableViewDropProposal
```

您提出的处理表视图中的删除的解决方案。

```
protocol UITableViewDropItem
```

与要放入表视图中的项关联的数据。

```
protocol UITableViewDropPlaceholderContext
```

在等待接收计划显示的实际数据时临时插入表视图的对象。

```
protocol UIDataSourceTranslating
```

用于管理数据源对象的高级接口。

[`class UITableViewPlaceholder`](https://developer.apple.com/documentation/uikit/uitableviewplaceholder)

## See Also

### Container Views

Collection Views

使用可配置且高度可自定义的布局显示嵌套视图。

```
class UIStackView
```

用于在列或行中布局视图集合的简化界面。

```
class UIScrollView
```

允许滚动和缩放其包含的视图的视图。