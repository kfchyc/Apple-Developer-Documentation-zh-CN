- [SiriKit](dash-apple-api://load?request_key=cssirikit) 
- [Media Intent Shortcuts](dash-apple-api://load?request_key=ts2978624) 
- Playing Media Through Siri Shortcuts

Sample Code

# Playing Media Through Siri Shortcuts

Language

- Swift
- [Objective-C](dash-apple-api://load?topic_id=2962157&language=occ)

SDKs

- iOS 12.0+
- Xcode 10.1+

Play audio and video from your app using media intent shortcuts.

Download

------

## Overview

Audio Cast is a simulated podcasting app that plays episodes from its podcast library. This sample code project demonstrates how to:

- Donate media intents for podcast episodes that the user has already heard.
- Play episodes in the app using Siri Shortcuts.
- Show the podcast or playlist containing an episode.
- Suggest new podcast episodes to the user through Siri.

The Audio Cast project contains the targets AudioCast and AudioCastIntents. The first is the app that people use to view and play podcast episodes. The second is the Intents app extension that handles [`INPlayMediaIntent`](dash-apple-api://load?topic_id=2963012&language=swift)requests.

The project also includes the target AudioCastKit, a framework containing the source code needed by the other targets for tasks such as data management of the podcast library and playback of audio content. Structuring the code in this manner avoids duplicating source code by providing a central location for the shared code. For more information about this code structure, see [Structuring Your Code to Support App Extensions](dash-apple-api://load?topic_id=2864027&language=swift).

### Configure the Sample Code Project

Before you can run Audio Cast on your iOS device, perform the following steps in Xcode:

1. Add a name to *App Groups* for the AudioCast and AudioCastIntents targets. To learn how to add an app group name, see [Configure app groups](https://help.apple.com/xcode/mac/current/#/dev8dd3880fe).
2. Change the value for the variable `AppGroup` to the name added in the previous step. AppGroup is located in the file *UserDefaults+DataSource.swift.*
3. Select a *Team* account under the *Signing* section in the project editor for the targets AudioCast and AudioCastIntents.

### Donate Media Intents

Before Siri can suggest a shortcut to audio or video content, an app must tell Siri about the content after the user interacts with it. To accomplish this, the app donates an [`INPlayMediaIntent`](dash-apple-api://load?topic_id=2963012&language=swift) object representing the media content to Siri. Audio Cast, for instance, makes a donation each time the user plays a podcast episode using the app.

You can group donations into logical containers, which helps you track donations that are similar to each other. To create a grouping, set the [`INInteraction`](dash-apple-api://load?topic_id=1639464&language=swift) object’s [`groupIdentifier`](dash-apple-api://load?topic_id=1638832&language=swift) property to a value identifying the logical container. For example, Audio Cast sets the property to the identifier of the podcast show or playlist containing the episode, which groups donations by those podcast shows or playlists.

```swift
public func donatePlayRequestToSystem(_ request: PlayRequest) {
    let interaction = INInteraction(intent: request.intent, response: nil)

        /*
     Set the groupIdentifier to be the container's ID so that all interactions can be
     deleted with the same ID if the user deletes the container.
     */
    interaction.groupIdentifier = request.container.itemID.uuidString

        interaction.donate { (error) in
        if error != nil {
            guard let error = error as NSError? else { return }
            os_log("Could not donate interaction %@", error)
        } else {
            os_log("Play request interaction donation succeeded")
        }
    }
}
```



Because Audio Cast groups donations, it’s able to delete all donations associated with a podcast show or playlist after the user unsubscribes from the show or removes the playlist.

```swift
public func deleteInteraction(for container: LibraryItemContainer) {
    // Use the container's ID for the group identifier to match the ID used when the donation was made.
    INInteraction.delete(with: container.itemID.uuidString) { (error) in
        if error != nil {
            guard let error = error as NSError? else { return }
            os_log("Could not delete interaction %@", error)
        } else {
            os_log("Deleting container interaction succeeded")
        }
    }
}
```



### Play Media Content

Because playing audio or video can be a long-running task, the main app—not the Intents app extension—plays the media content. The lifecycle of the app extension is too short to reliably play audio and video. In order for the extension to pass control to the main app, the extension’s intent handler responds with either [`INPlayMediaIntentResponseCode.handleInApp`](dash-apple-api://load?topic_id=2968465&language=swift) or [`INPlayMediaIntentResponseCode.continueInApp`](dash-apple-api://load?topic_id=2976236&language=swift).

When handling a request to play audio, respond with [`INPlayMediaIntentResponseCode.handleInApp`](dash-apple-api://load?topic_id=2968465&language=swift) to tell the system to launch the main app in the background and call the [`UIApplicationDelegate`](dash-apple-api://load?topic_id=1623080&language=swift) method [`application(_:handle:completionHandler:)`](dash-apple-api://load?topic_id=2887573&language=swift). It’s here that the app begins playing the audio in the background.

When handling a request to play video, the extension handler should respond with [`INPlayMediaIntentResponseCode.continueInApp`](dash-apple-api://load?topic_id=2976236&language=swift). This tells the system to launch the app in the foreground and call the [`application(_:continue:restorationHandler:)`](dash-apple-api://load?topic_id=1623072&language=swift) method on the app delegate.

Audio Cast plays only audio content, so its Intents app extension always returns the [`INPlayMediaIntentResponseCode.handleInApp`](dash-apple-api://load?topic_id=2968465&language=swift) response code when handling the intent.

```swift
public func handle(intent: INPlayMediaIntent, completion: @escaping (INPlayMediaIntentResponse) -> Void) {
    /*
     Media playback should start in the main app because the app extension's life span is short.

     For audio content, respond with the `.handleInApp` response code to have the system launch
     the main app in the background and call `application(_:, handle:, completionHandler:)` on
     the `UIApplicationDelegate`. This is the app's opportunity to play the audio in the background
     without the user needing to use the app directly.

     For video content, respond with the `.continueInApp` response code. This launches the main
     app in the foreground, and calls the `UIApplicationDelegate` method
     `application(_, continue:, restorationHandler:)`.
    */
    let response = INPlayMediaIntentResponse(code: .handleInApp, userActivity: nil)
    completion(response)
}
```



Audio Cast then handles the playback request in the [`application(_:handle:completionHandler:)`](dash-apple-api://load?topic_id=2887573&language=swift) method of the main app by playing the audio content in the background.

```swift
func application(_ application: UIApplication, handle intent: INIntent, completionHandler: @escaping (INIntentResponse) -> Void) {
    // If a user taps on the play button for a media suggestion, the app will deliver an intent to the intent extension,
    // and if the extension indicates the app can handle the intent, it will be delivered to this method to start playback.

        guard let mediaIntent = intent as? INPlayMediaIntent,
        let requestedContent = PlayRequest(intent: mediaIntent),
        let itemsToPlay = AudioPlaybackManager.shared.resolveItems(for: requestedContent)
    else {
        completionHandler(INPlayMediaIntentResponse(code: .failure, userActivity: nil))
        return
    }

        AudioPlaybackManager.shared.play(itemsToPlay)

        let response = INPlayMediaIntentResponse(code: .success, userActivity: nil)
    completionHandler(response)
}
```



Audio Cast can play audio in the background because its Xcode project has the background mode *Audio, AirPlay, and Picture in Picture* enabled for the AudioCast target. This setting adds the *Required Background Mode* key to the app’s *Info.plist* file. To learn more about this capability, see [Enabling Background Audio](dash-apple-api://load?topic_id=2943659&language=swift).

### Show Podcasts and Playlists

When Siri suggests or invokes a shortcut to Audio Cast, the user can tap the shortcut to get more information about the podcast show or playlist. In order to provide this information to the user, Audio Cast implements the app delegate method [`application(_:continue:restorationHandler:)`](dash-apple-api://load?topic_id=1623072&language=swift). After the user taps the shortcut, the system calls this method passing the media intent from the shortcut as part of a system-provided [`NSUserActivity`](dash-apple-api://load?topic_id=1414761&language=swift) object. The app uses information from the intent to restore the app to a state where it displays the show or playlist.

```swift
func application(_ application: UIApplication,
                 continue userActivity: NSUserActivity,
                 restorationHandler: @escaping ([UIUserActivityRestoring]?) -> Void) -> Bool {

        // If a user taps on the UI for a media suggestion, the app will open and the media suggestion will be
    // delivered to the app within a NSUserActivity. The activity type string on the activity will be the
    // name of the intent.

        guard userActivity.activityType == NSStringFromClass(INPlayMediaIntent.self),
        let mediaIntent = userActivity.interaction?.intent as? INPlayMediaIntent,
        let requestedContent = PlayRequest(intent: mediaIntent),
        let navigationController = window?.rootViewController as? UINavigationController
    else {
        return false
    }

        // Continuing a user activity should display the content rather than start playback of the content. Pass the
    // user activity to a view controller for display.
    userActivity.addUserInfoEntries(from: [NSUserActivity.LibraryItemContainerIDKey: requestedContent.container.itemID])
    restorationHandler(navigationController.viewControllers)

        return true
}
```



### Suggest New Media Content

Podcast apps receive content on a regular basis, such as when a new episode becomes available for a podcast the user is listening to. After receiving the new content, the app should suggest the content to Siri by providing [`INPlayMediaIntent`](dash-apple-api://load?topic_id=2963012&language=swift) objects for each new episode. To make the suggestion, pass an array of media intents to [`setSuggestedMediaIntents(_:)`](dash-apple-api://load?topic_id=2963078&language=swift) on the [`shared`](dash-apple-api://load?topic_id=2963079&language=swift) instance of [`INUpcomingMediaManager`](dash-apple-api://load?topic_id=2963076&language=swift). Audio Cast calls this method to add new episodes to the suggestion list, and to remove those episodes that the user already heard from the list.

```swift
func updateSystemUpcomingContentSuggestions() {

        // Turn the list of shows into [INPlayMediaIntent] with each intent representing one unplayed episode.
    var newMediaIntents = podcastLibrary.shows.reduce([INPlayMediaIntent]()) { (partialResult, show) -> [INPlayMediaIntent] in

                let episodesForShow = episodes(for: show.itemID)
        let intentPerEpisode = episodesForShow.map { (episode) -> INPlayMediaIntent in
            return INPlayMediaIntent(mediaItems: [episode.mediaItem],
                                     mediaContainer: show.mediaItem,
                                     playShuffled: false,
                                     playbackRepeatMode: .none,
                                     resumePlayback: false)
        }

                let results = partialResult + intentPerEpisode
        return results
    }

        // The intents returned to the system need to be ordered with the most important suggestion from
    // the app first. In this sample, do a simple alphabetical sort based on the episode title.
    newMediaIntents.sort { (intentA, intentB) -> Bool in
        guard let titleA = intentA.mediaItems?.first?.title,
            let titleB = intentB.mediaItems?.first?.title
            else {
                return false
        }

                return titleA.localizedCaseInsensitiveCompare(titleB) == .orderedAscending
    }

        // Apps with periodic new content, like podcasts, should set the prediction mode to `.onlyPredictSuggestedIntents` so that
    // episodes already listened to by the user are not suggested.
    INUpcomingMediaManager.shared.setPredictionMode(.onlyPredictSuggestedIntents, for: .podcastEpisode)
    INUpcomingMediaManager.shared.setSuggestedMediaIntents(NSOrderedSet(array: newMediaIntents))
}
```