---
layout: post
title:  Learning Notes -  How to set status bar style to LightContent
---
## How to set status bar style to LightContent in iOS 7+?
- Step 1: Head over to your project's info.plist. 
- Step 2: Change the settings for "View controller-based status bar appearance" to NO. If this key is not visible, simply add it by pressing the plus button. Should look like this.
- Step 3: Go to view controller class and add the following to your viewDidLoad method.

```
// status bar
    UIApplication.sharedApplication().setStatusBarStyle(UIStatusBarStyle.LightContent, animated: true) 
```

> It is not a common approach. `setStatusBarStyle` has been deprecated on iOS8+


## For anyone using a UINavigationController:

The UINavigationController does not forward on preferredStatusBarStyle calls to its child view controllers. Instead it manages its own state - as it should, it is drawing at the top of the screen where the status bar lives and so should be responsible for it. Therefor implementing preferredStatusBarStyle in your VCs within a nav controller will do nothing - they will never be called.

The trick is what the UINavigationController uses to decide what to return for UIStatusBarStyleDefault or UIStatusBarStyleLightContent. It bases this on it's UINavigationBar.barStyle. The default (UIBarStyleDefault) results in the dark foreground UIStatusBarStyleDefault status bar. And UIBarStyleBlack will give a UIStatusBarStyleLightContent status bar.

If you want UIStatusBarStyleLightContent on a UINavigationController use:

```
self.navigationController.navigationBar.barStyle = UIBarStyleBlack;
```

> It worked only in actual vc.


