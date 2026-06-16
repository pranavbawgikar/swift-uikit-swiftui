# UIViewController
`UIViewController` manages screen content and view lifecycles.

Every `UIViewController` has a root `UIView` which fills the screen, everything else (buttons, label, table views) is added as a subview of the root view, which forms a tree called View Hierarchy.

In Programmatic UIKit, whenever a new view is added to the ViewController, `view.addSubView()` is present under the hood. When using Storyboard, the XML deserialization does this for you invisibly.

Example
```
view.addSubview(myButton)           // adds the view
myButton.translatesAutoresizingMaskIntoConstraints = false  // required before adding constraints
NSLayoutConstraint.activate([       // then you position it with Auto Layout
    myButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
    myButton.bottomAnchor.constraint(equalTo: view.safeAreaLayoutGuide.bottomAnchor, constant: -20)
])
```
This three-step pattern: `addSubview`, disable autoresizing mask, activate constraints is constantly seen in UIKit code.

### viewDidLoad
`viewDidLoad` is called when the ViewController's content view is created in memory or loaded from Storyboard.

`viewDidLoad` method fires once and only once for the entire lifetime of that ViewController instance.


### viewWillAppear
`viewWillAppear` is called every time the view comes on screen.

By the time `viewWillAppear` is called, the content view has already been created (that happened in `viewDidLoad`).
`viewWillAppear` signals that the view is about to be added to the window's visible hierarchy, meaning it is about to become visible to the user, but has not appeared on screen yet. 

The correct sequence is:
```
viewDidLoad          → view created in memory, subviews set up
viewWillAppear       → view about to become visible (not on screen yet)
viewDidAppear        → view is now fully visible on screen
viewWillDisappear    → view about to leave the screen
viewDidDisappear     → view has left the screen
```

### viewDidAppear
`viewDidAppear` fires after the content view is added to the view hierarchy.

### viewWillDisappear
`viewWillDisappear` is called before the content view is removed.

### viewDidDisappear
`viewDidDisappear` is called after the content view is removed.

### viewWillLayoutSubviews
`viewWillLayoutSubviews` is called when the content view's bounds change but before it lays out it's subviews.

### viewDidLayoutSubviews
`viewDidLayoutSubviews` is called when the content view's bounds change but after it lays out it's subviews.

## Child View Controllers
Child View Controllers are used to encapsulate distinct UI sections, resuse the same child in multiple parent screens and to separate concerns such that each View Controller manages only it's own view.
