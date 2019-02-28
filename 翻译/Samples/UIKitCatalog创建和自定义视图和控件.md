- [UIKit](dash-apple-api://load?request_key=csuikit) 
- [Views and Controls](dash-apple-api://load?request_key=ts2864393) 
- UIKitCatalog: Creating and Customizing Views and Controls

Sample Code

# uikititecaton:视图和控件的创建和自定义

语言

- Swift
- [Objective-C](dash-apple-api://load?topic_id=3026824&language=occ)

SDKs

- iOS 11.4+
- Xcode 10.0+

通过使用 uikit 中的视图和控件自定义应用的用户界面。

下载

------

## 概述

本示例指导您完成可在 ios 应用中执行的多种类型的自定义。该示例使用拆分视图控制器体系结构来导航 uikit 视图和控件。主视图控制器(`MasterViewController`) 显示可用的视图和控件。选择一个将显示与之关联的辅助视图控制器。

每个辅助视图控制器的名称反映其 * 目标项 *。例如, `AlertControllerViewController`类演示如何使用 `UIAlertController` 对象。此规则的唯一例外是`UISearchBar` 和 `UIToolbar`;这些 api 在多个视图控制器中演示, 以解释它们的控件如何工作以及如何自定义它们。为了演示如何管理故事板的复杂性, 所有视图控制器都托管在单独的情节提要中, 并在需要时加载。

此示例演示以下视图和控件 (下面的部分中引用了其中的几个视图和控件):

- [`UIActivityIndicatorView`](dash-apple-api://load?topic_id=1622846&language=swift)
- [`UIAlertController`](dash-apple-api://load?topic_id=1620108&language=swift)
- [`UIButton`](dash-apple-api://load?topic_id=1624013&language=swift)
- [`UIDatePicker`](dash-apple-api://load?topic_id=1615991&language=swift)
- [`UIImageView`](dash-apple-api://load?topic_id=1621067&language=swift)
- [`UIPageControl`](dash-apple-api://load?topic_id=1621236&language=swift)
- [`UIPickerView`](dash-apple-api://load?topic_id=1614387&language=swift)
- [`UIProgressView`](dash-apple-api://load?topic_id=1619847&language=swift)
- [`UISearchBar`](dash-apple-api://load?topic_id=1624310&language=swift)
- [`UISegmentedControl`](dash-apple-api://load?topic_id=1618568&language=swift)
- [`UISlider`](dash-apple-api://load?topic_id=1621350&language=swift)
- [`UIStackView`](dash-apple-api://load?topic_id=1616219&language=swift)
- [`UIStepper`](dash-apple-api://load?topic_id=1624084&language=swift)
- [`UISwitch`](dash-apple-api://load?topic_id=1623692&language=swift)
- [`UITextField`](dash-apple-api://load?topic_id=1619619&language=swift)
- [`UITextView`](dash-apple-api://load?topic_id=1618604&language=swift)
- [`UIToolbar`](dash-apple-api://load?topic_id=1617996&language=swift)
- [`WKWebView`](dash-apple-api://load?topic_id=1414968&language=swift)

### 向您的视图添加辅助功能支持

voiceover 和其他系统辅助功能技术使用视图和控件提供的信息来帮助所有用户浏览您的内容。uikit 视图包括默认的辅助功能支持, 但您可以通过提供自定义辅助功能信息来改善用户体验。

在此 UIKitCatalog示例中, 多个视图控制器配置其关联视图的`accessibilityType`  和`accessibilityLabel`属性。选取器视图列没有标签, 因此选取器视图要求其委托提供相应的辅助功能信息:

```swift
func pickerView(_ pickerView: UIPickerView, accessibilityLabelForComponent component: Int) -> String? {

        switch ColorComponent(rawValue: component)! {
    case .red:
        return NSLocalizedString("Red color component value", comment: "")

    case .green:
        return NSLocalizedString("Green color component value", comment: "")

    case .blue:
        return NSLocalizedString("Blue color component value", comment: "")
    }
}
```

### 显示自定义警报

`AlertControllerViewController`演示了几种从界面显示模式警报和操作表的技术。配置过程类似于所有警报:

1. 确定要在警报中显示的消息。
2. 创建和配置 "UIAlertController" 对象。
3. 为用户可能执行的操作添加处理程序。
4. 显示警报控制器。

 `showSimpleAlert`函数使用`NSLocalizedString`函数以用户的首选语言检索警报消息。此函数使用这些字符串来创建和配置 `UIAlertController`对象。尽管警报中的按钮的标题为 "确定", 但示例使用取消操作来确保警报控制器将正确的样式应用于按钮:

```swift
func showSimpleAlert() {
    let title = NSLocalizedString("A Short Title is Best", comment: "")
    let message = NSLocalizedString("A message should be a short, complete sentence.", comment: "")
    let cancelButtonTitle = NSLocalizedString("OK", comment: "")

    let alertController = UIAlertController(title: title, message: message, preferredStyle: .alert)

    // Create the action.
    let cancelAction = UIAlertAction(title: cancelButtonTitle, style: .cancel) { _ in
        print("The simple alert's cancel action occurred.")
    }

    // Add the action.
    alertController.addAction(cancelAction)

    present(alertController, animated: true, completion: nil)
}
```

### 自定义滑块的外观

此示例演示了显示 `UISlider`的不同方法, 该控件用于从连续的值范围中选择单个值。通过为左侧跟踪、右侧跟踪和拇指分配可拉伸图像, 可以自定义滑块的外观。在本例中, 轨道图像是可拉伸的, 并且是一个像素宽。使轨道图像变宽以提供圆角, 但一定要将这些图像设置为`capInsets`属性以允许它们。

"配置自定义滑块" `configureCustomSlider` 功能设置自定义滑块:

```swift
func configureCustomSlider() {
    let leftTrackImage = UIImage(named: "slider_blue_track")
    customSlider.setMinimumTrackImage(leftTrackImage, for: .normal)

    let rightTrackImage = UIImage(named: "slider_green_track")
    customSlider.setMaximumTrackImage(rightTrackImage, for: .normal)

    let thumbImage = UIImage(named: "slider_thumb")
    customSlider.setThumbImage(thumbImage, for: .normal)

    customSlider.minimumValue = 0
    customSlider.maximumValue = 100
    customSlider.isContinuous = false
    customSlider.value = 84

    customSlider.addTarget(self, action: #selector(SliderViewController.sliderValueDidChange(_:)), for: .valueChanged)
}
```

### 将搜索栏添加到您的界面

使用 `UISearchBar`从用户处接收与搜索相关的信息。有多种方法可以自定义搜索栏的外观:

-添加取消按钮。
-添加书签按钮。
-设置书签图像, 包括正常状态和突出显示状态。
-更改应用于搜索栏中的关键元素的色调颜色。
-设置搜索栏的背景图像。

 `configureSearchBar` 功能设置自定义搜索栏:

```swift
func configureSearchBar() {
    searchBar.showsCancelButton = true
    searchBar.showsBookmarkButton = true

    searchBar.tintColor = UIColor(named: "Tint_Purple_Color")

    searchBar.backgroundImage = UIImage(named: "search_bar_background")

    // Set the bookmark image for both normal and highlighted states.
    let bookmarkImage = #imageLiteral(resourceName: "bookmark_icon")
    searchBar.setImage(bookmarkImage, for: .bookmark, state: .normal)

    let bookmarkHighlightedImage = #imageLiteral(resourceName: "bookmark_icon_highlighted")
    searchBar.setImage(bookmarkHighlightedImage, for: .bookmark, state: .highlighted)
}
```

### 自定义工具栏的外观

此示例演示如何自定义  `UIToolbar`, 这是一个专门的视图, 沿界面的底部边缘显示一个或多个按钮。通过确定工具栏样式 (黑色或默认)、半透明、色调颜色和背景色, 自定义工具栏。

"自定义工具栏视图控制器"`CustomToolbarViewController` 中的以下 `viewDidLoad` 功能设置了一个着色工具栏:

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    // See the `UIBarStyle` enum for more styles, including `.Default`.
    toolbar.barStyle = .black
    toolbar.isTranslucent = true

    toolbar.tintColor = UIColor(named: "Tint_Green_Color")
    toolbar.backgroundColor = UIColor(named: "Tint_Blue_Color")

    let toolbarButtonItems = [
        refreshBarButtonItem,
        flexibleSpaceBarButtonItem,
        actionBarButtonItem
    ]
    toolbar.setItems(toolbarButtonItems, animated: true)
}
```

"自定义工具栏视图控制器" `CustomToolbarViewController`通过更改工具栏的背景图像来演示进一步的自定义:

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    let toolbarBackgroundImage = UIImage(named: "toolbar_background")
    toolbar.setBackgroundImage(toolbarBackgroundImage, forToolbarPosition: .bottom, barMetrics: .default)

    let toolbarButtonItems = [
        customImageBarButtonItem,
        flexibleSpaceBarButtonItem,
        customBarButtonItem
    ]
    toolbar.setItems(toolbarButtonItems, animated: true)
}
```

### 添加页面控制界面

使用 `UIPageControl`来构造应用的用户界面。页面控件是一个专门控件, 它显示一系列水平的点, 每个点对应于应用文档或其他数据模型实体中的页面。通过为所有页面指示器点和当前页面指示器点设置其色调颜色, 可以自定义页面控件。

"配置页面控制"  `configurePageControl` 功能设置自定义的页面控制:

```swift
func configurePageControl() {
    // The total number of available pages is based on the number of available colors.
    pageControl.numberOfPages = colors.count
    pageControl.currentPage = 2

    pageControl.pageIndicatorTintColor = UIColor(named: "Tint_Green_Color")
    pageControl.currentPageIndicatorTintColor = UIColor(named: "Tint_Purple_Color")

    pageControl.addTarget(self, action: #selector(PageControlViewController.pageControlValueDidChange), for: .valueChanged)
}
```