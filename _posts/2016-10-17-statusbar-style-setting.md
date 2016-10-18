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

## UINavigationController barStyle Setting
`[[UINavigationBar appearance] setBarStyle:UIBarStyleBlack]`

