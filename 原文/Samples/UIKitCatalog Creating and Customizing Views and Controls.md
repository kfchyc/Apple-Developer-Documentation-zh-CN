- [UIKit](dash-apple-api://load?request_key=csuikit) 
-  

- [Views and Controls](dash-apple-api://load?request_key=ts2864393)  
- UIKitCatalog: Creating and Customizing Views and Controls

Sample Code

# UIKitCatalog: Creating and Customizing Views and Controls

Language

- Swift
- [Objective-C](dash-apple-api://load?topic_id=3026824&language=occ)

SDKs

- iOS 11.4+
- Xcode 10.0+

Customize your app’s user interface by using views and controls in UIKit.

Download

------

## Overview

This sample guides you through several types of customizations that you can do in your iOS app. The sample uses a split view controller architecture for navigating UIKit views and controls. The primary view controller (`MasterViewController`) shows the available views and controls. Selecting one shows the secondary view controller associated with it.

The name of each secondary view controller reflects its *target item*. For example, the `AlertControllerViewController` class shows how to use a `UIAlertController` object. The only exceptions to this rule are `UISearchBar` and `UIToolbar`; these APIs are demonstrated in multiple view controllers to explain how their controls function and how to customize them. To demonstrate how to manage the complexity of your storyboards, all view controllers are hosted in a separate storyboard and loaded when needed.

This sample demonstrates the following views and controls (several of which are referenced in the sections below):

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

### Add Accessibility Support to Your Views

VoiceOver and other system accessibility technologies use the information provided by your views and controls to help all users navigate your content. UIKit views include default accessibility support, but you can improve the user experience by providing custom accessibility information.

In this UIKitCatalog sample, several view controllers configure the `accessibilityType` and `accessibilityLabel` properties of their associated view. Picker view columns don’t have labels, so the picker view asks its delegate for the corresponding accessibility information:

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

### Display a Custom Alert

`AlertControllerViewController` demonstrates several techniques for displaying modal alerts and action sheets from your interface. The configuration process is similar for all alerts:

1. Determine the message you want to display in the alert.
2. Create and configure a `UIAlertController` object.
3. Add handlers for actions that the user may take.
4. Present the alert controller.

The `showSimpleAlert` function uses the `NSLocalizedString` function to retrieve the alert messages in the user’s preferred language. This function uses those strings to create and configure the `UIAlertController`object. Although the button in the alert has the title OK, the sample uses a cancel action to ensure that the alert controller applies the proper styling to the button:

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

### Customize the Appearance of Sliders

This sample demonstrates different ways to display a `UISlider`, a control used to select a single value from a continuous range of values. You customize the appearance of a slider by assigning stretchable images for left-side tracking, right-side tracking, and the thumb. In this example, the track image is stretchable and is one pixel wide. Make the track images wider to provide rounded corners, but be sure to set these images’ `capInsets` property to allow for them.

The `configureCustomSlider` function sets up a custom slider:

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

### Add a Search Bar to Your Interface

Use a `UISearchBar` for receiving search-related information from the user. There are various ways to customize the look of the search bar:

- Add a cancel button.
- Add a bookmark button.
- Set the bookmark image, both normal and highlighted states.
- Change the tint color that applies to key elements in the search bar.
- Set the search bar’s background image.

The `configureSearchBar` function sets up a custom search bar:

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

### Customize the Appearance of Toolbars

This sample shows how to customize a `UIToolbar`, a specialized view that displays one or more buttons along the bottom edge of your interface. Customize a toolbar by determining its bar style (black or default), translucency, tint color, and background color.

The following `viewDidLoad` function in `CustomToolbarViewController` sets up a tinted tool bar:

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

`CustomToolbarViewController` demonstrates further customization by changing the toolbar’s background image:

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

### Add a Page Control Interface

Use a `UIPageControl` to structure your app’s user interface. A page control is a specialized control that displays a horizontal series of dots, each of which corresponds to a page in the app’s document or other data-model entity. You customize a page control by setting its tint color for all the page indicator dots, and for the current page indicator dot.

The `configurePageControl` function sets up a customized page control:

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