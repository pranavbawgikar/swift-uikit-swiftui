# App Lifecycle
App Lifecycle has five states:
1. Not Running
2. Inactive
3. Active
4. Suspended
5. Background

Typical flow between the five states:
```
Not Running →
Active (fresh launch) →
Inactive (interruption like a phone call) →
Background (user switches apps) →
Suspended (OS freezes the app to save memory) →
Not Running (OS kills it if memory is needed).
```
The transitions between these are what trigger `AppDelegate` and `SceneDelegate` callbacks.

# AppDelegate
AppDelegate handles app-level events.

`willFinishLaunchingWithOptions` & `didFinishLaunchingWithOptions` belong to the AppDelegate.

### willFinishLaunchingWithOptions
`willFinishLaunchingWithOptions` fire before `didFinishLaunchingWithOptions` and before the state restoration process begins.

iOS has a feature called state restoration where it saves the exact screen a user was on when they left the app and rebuilds it the next time it is opened.
The restoration process starts immediately after `willFinishLaunchingWithOptions` returns. So, if there is anything that needs to be registered or configured before that restoration, it should go here.

### didFinishLaunchingWithOptions
`didFinishLaunchingWithOptions` fires after the state restoration is complete and the app is fully initialized but before the UI is visible to the user.

Third-party SDK initializations like Firebase, Crashlytics can be done in `didFinishLaunchingWithOptions`.

`didFinishLaunchingWithOptions` runs on the main thread and blocks the UI from appearing until it returns. Every millisecond spent here is a millisecond of blank screen the user sees at launch.
No network calls, heavy disk reads and no blocking operations of any kind.

#### What happens if you return `false` from `didFinishLaunchingWithOptions`?
```
func application(_ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    return false  // what happens?
}
```
Returning false tells iOS: "this app should not be launched." The system interprets it as the app declining to handle the launch. You should essentially never return `false` in a real app. It is considered undefined/legacy behavior on modern iOS and can cause unpredictable results depending on the iOS version.
