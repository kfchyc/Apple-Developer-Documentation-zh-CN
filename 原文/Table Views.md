# Table Views

Display data in a single column of customizable rows.

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

A view that presents data using rows arranged in a single column.

```
class UITableViewController
```

A view controller that specializes in managing a table view. 

```
class UITableViewFocusUpdateContext
```

A context object that provides information relevant to a specific focus update from one view to another.

```
class UILocalizedIndexedCollation
```

An object that provides ways to organize, sort, and localize the data for a table view that has a section index. 

### Rows

```
class UITableViewCell
```

A cell in a table view.

```
class UISwipeActionsConfiguration
```

The set of actions to perform when swiping on rows of a table.

```
class UIContextualAction
```

An action to display when the user swipes a table row.

```
class UITableViewRowAction
```

A single action to present when the user swipes horizontally in a table row. 

```
enum UIContextualAction.Style
```

Constants indicating the style information that is applied to the action button. 

Creating Self-Sizing Table View Cells

Create table view cells that support Dynamic Type and use system spacing constraints to adjust the spacing surrounding text labels.

### Headers and Footers

```
class UITableViewHeaderFooterView
```

A reusable view that can be placed at the top or bottom of a table section to display additional information for that section.

### Adornments

```
class UIRefreshControl
```

A standard control that can initiate the refreshing of a scroll view’s contents. 

### Drag and Drop

Supporting Drag and Drop in Table Views

Initiate drags and handle drops from a table view.

```
protocol UITableViewDragDelegate
```

The interface for initiating drags from a table view.

```
protocol UITableViewDropDelegate
```

The interface for handling drops in a table view.

```
protocol UITableViewDropCoordinator
```

An interface for coordinating your custom drop-related actions with the table view.

[`class UITableViewDropPlaceholder`](https://developer.apple.com/documentation/uikit/uitableviewdropplaceholder)

```
class UITableViewDropProposal
```

Your proposed solution for handling a drop in a table view.

```
protocol UITableViewDropItem
```

The data associated with an item being dropped into the table view.

```
protocol UITableViewDropPlaceholderContext
```

An object that you insert temporarily into the table view while waiting to receive the actual data that you plan to display.

```
protocol UIDataSourceTranslating
```

An advanced interface for managing a data source object.

[`class UITableViewPlaceholder`](https://developer.apple.com/documentation/uikit/uitableviewplaceholder)

## See Also

### Container Views

Collection Views

Display nested views using a configurable and highly customizable layout.

```
class UIStackView
```

A streamlined interface for laying out a collection of views in either a column or a row. 

```
class UIScrollView
```

A view that allows the scrolling and zooming of its contained views.