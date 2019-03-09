- [UIKit](dash-apple-api://load?request_key=csuikit) 
- [Core App](dash-apple-api://load?request_key=ts2864379) 
- [Managing Your App's Life Cycle](dash-apple-api://load?request_key=ts2922729) 
- [Responding to the Launch of Your App](dash-apple-api://load?request_key=ts2922731) 
- About the App Launch Sequence

Article

# About the App Launch Sequence

Language

- Swift
- [Objective-C](dash-apple-api://load?topic_id=2922733&language=occ)

Learn the order in which your custom code is executed at launch time.

------

## Overview

Launching an app involves a complex sequence of steps, most of which UIKit handles automatically. During the launch sequence, UIKit calls methods of your app delegate so that you can perform custom tasks. illustrates the sequence of steps that occur from the time the app is launched until it is considered initialized.

Figure 1

![](https://ws4.sinaimg.cn/large/006tKfTcly1g0pgf3d6g4j30q40gtmya.jpg)

The app launch and initialization sequence





1. The app is launched, either explicitly by the user or implicitly by the system.
2. The Xcode-provided `main` function calls UIKit's [`UIApplicationMain(_:_:_:_:)`](dash-apple-api://load?topic_id=1622933&language=swift) function.
3. The [`UIApplicationMain(_:_:_:_:)`](dash-apple-api://load?topic_id=1622933&language=swift) function creates the [`UIApplication`](dash-apple-api://load?topic_id=1623037&language=swift) object and your app delegate.
4. UIKit loads your app's default interface from the main storyboard or nib file.
5. UIKit calls your app delegate's [`application(_:willFinishLaunchingWithOptions:)`](dash-apple-api://load?topic_id=1623032&language=swift) method.
6. UIKit performs state restoration, which calls additional methods of your app delegate and view controllers.
7. UIKit calls your app delegate's [`application(_:didFinishLaunchingWithOptions:)`](dash-apple-api://load?topic_id=1622921&language=swift) method .

When initialization is complete, the system moves your app to either the active (foreground) state or background state. When your app moves to the active state, its windows appear onscreen and it begins responding to user interactions. When your app moves to the background state, its windows remain hidden and it might run for only a short time before it is suspended.

Most of your launch-time initialization code should be the same, regardless of whether your app is launching into the foreground or background. For example, you should still initialize your app’s data structures and set up your app’s user interface. However, if you have custom tasks that you want to perform only when moving to the foreground or background, check the [`applicationState`](dash-apple-api://load?topic_id=1623003&language=swift)property of the [`UIApplication`](dash-apple-api://load?topic_id=1623037&language=swift) object. UIKit sets that property to [`UIApplication.State.inactive`](dash-apple-api://load?topic_id=1623000&language=swift) for an app that’s moving to the foreground, and [`UIApplication.State.background`](dash-apple-api://load?topic_id=1623074&language=swift) for an app that’s moving to the background.