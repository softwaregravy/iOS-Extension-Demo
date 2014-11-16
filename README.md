# DemoNotes: Demonstration of iOS 8 "today" and "share" extensions.

This project is a sample of how to create iOS "today" and "share" extensions. The sequence of commits in the repository demonstrates various steps in the process.

The central app is a trivial text notes app. The UI is simply the standard master/detail layout created using one of Xcode's project templates. The master view displays a list of notes. Tapping on one displays a text view that can be used to edit the note content. The app is intentionally simplistic in order to keep this repository focused on the process of developing app extensions.

The basic text notes app is complete as of commit a32cf84. The commits then go through the process of adding a today extension which also displays notes and a share extension that can be used to create new notes from other iOS apps. When complete,

* The today extension displays data shared with the app
* The today extension communicates with the app via a custom URL scheme so that users can launch the app to edit selected text notes
* The share extension allows adding custom notes from other apps, e.g. Safari.

The project then goes through the following steps:

* 15889ce Add a today extension target in Xcode, but don't make any changes to it. This creates a working today extension with a default "Hello, World" UI.
* 82727be Change the display name for the today extension to be more descriptive when it appears in the iOS notification center. The only significant change is to `Info.plist`.
* 2d99aa6 Replace the default today extension UI with a table view UI to match the app. This UI is non-functional at this point, since there's no code to support it yet.
* 2dc85b5 Add a custom framework target to the project to hold code that will be shared between the app and the extensions (in this case only the `DemoNote` model object). Change the app to `#import` the framework header instead of the shared code header.
* 3f05d63 Enable the "app groups" capability in Xcode so that the app and its extensions will be able to share data. (At this point Xcode will communicate with Apple's developer center to configure the app ID and generate custom provisioning profiles that include the new entitlements).
* 38c3efc Change the existing app code to read/write data via the new app group instead of in its private documents directory.
* 6edf19a Fix a bug in the original notes app which would result in unnecessary saving of unchanged data.
* 2cbde1d Implement the today extension. This adds code similar to the app to load and display notes. It also adds the shared framework to the today extension and-- importantly-- tells Xcode that the framework should only use extension-safe APIs only.
* dc5e4ea Add a URL scheme to the app and add code to the today extension to use it. With this change, a user can tap on a note in the notification center to launch the app and have it display that note.
* e9a638c Add a share extension target in Xcode, but don't make any changes to it. This creates a share extension which uses `SLComposeServiceViewController` to display a UI similar to the built-in share extensions for Twittrer, Facebook, etc..
* c7a7a85 Implement the share extension. It's now possible to use the share button in Safari to create a new note using either the page title or selected text from the current page. This commit adds a custom JavaScript file that's used to preprocess the web page to extract items of interest that are then passed along to the share extension.
* 0ea4e38 Update the app to make it more aware of the share extension. Since new notes can now be added from outside the app, the app needs to be sure to check for new notes when the app enters the foreground.
* e95872f Replace the standard `SLComposeServiceViewController`-based UI with a custom UI, while keeping the extension a "share" (`com.apple.share-services`) extension.

# Credits

By Tom Harrington, @atomicbird on most social networks.
