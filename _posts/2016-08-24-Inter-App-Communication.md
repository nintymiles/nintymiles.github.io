---
layout: post
title:  Learning Notes - About iOS URL Scheme
---

## iOS URL Scheme is main method to perform inter-app communication

###Receiving Files and Data Sent to Your App
To receive files sent to your app using AirDrop, do the following:

- In Xcode, declare support for the document types your app is capable of opening.
- In your app delegate, implement the application:openURL:sourceApplication:annotation: method. Use that method to receive the data that was sent by the other app.
- Be prepared to look for files in your app’s Documents/Inbox directory and move them out of that directory as needed.


The Info tab of your Xcode project contains a Document Types section for specifying the document types your app supports. At a minimum, you must specify a name for your document type and one or more UTIs that represent the data type. For example, to declare support for PNG files, you would include public.png as the UTI string. iOS uses the specified **UTIs** to determine if your app is eligible to open a given document.

**After transferring an eligible document to your app’s container**, iOS launches your app (if needed) and **calls** the application:openURL:sourceApplication:annotation: method of its app delegate. If your app is in the foreground, you should use this method to open the file and display it to the user. If your app is in the background, you might decide only to note that the file is there so that you can open it later. Because **files transferred via AirDrop are encrypted using data protection**, you cannot open files unless the device is currently unlocked.

Files transferred to your app using AirDrop are placed in your app’s **Documents/Inbox** directory. Your app has permission to *read* and *delete* files in this directory but it does not have permission to write to files. *If you plan to modify the file, you must move it out of the Inbox directory before doing so*. It is recommended that you delete files from the Inbox directory when you no longer need them. 

### Sending a URL to Another App
When you want to send data to an app that implements a custom URL scheme, create an appropriately formatted URL and call the openURL: method of the app object. The openURL: method launches the app with the registered scheme and passes your URL to it. At that point, control passes to the new app.

The following code fragment illustrates how one app can request the services of another app (“todolist” in this example is a hypothetical custom scheme registered by an app):

```
NSURL *myURL = [NSURL URLWithString:@"todolist://www.acme.com?Quarterly%20Report#200806231300"];
[[UIApplication sharedApplication] openURL:myURL];
```

If your app defines a custom URL scheme, it should implement a handler for that scheme as described in Implementing Custom URL Schemes. For more information about the system-supported URL schemes, including information about how to format the URLs, see Apple URL Scheme Reference.

### Handling URL Requests

An app that has its own custom URL scheme must be able to handle URLs passed to it. All URLs are passed to your app delegate, either at launch time or while your app is running or in the background. To handle incoming URLs, your delegate should implement the following methods:

- Use the application:willFinishLaunchingWithOptions: and application:didFinishLaunchingWithOptions: methods to retrieve information about the URL and decide whether you want to open it. If either method returns NO, your app’s URL handling code is not called.
- Use the application:openURL:sourceApplication:annotation: method to open the file.


If your app is not running when a URL request arrives, it is launched and moved to the foreground so that it can open the URL. The implementation of your application:willFinishLaunchingWithOptions: or application:didFinishLaunchingWithOptions: method should retrieve the URL from its options dictionary and determine whether the app can open it. If it can, return YES and let your application:openURL:sourceApplication:annotation: (or application:handleOpenURL:) method handle the actual opening of the URL. 


### Displaying a Custom Launch Image When a URL is Opened
Apps that support *custom URL schemes* can provide **a custom launch image** for each scheme. When the system launches your app to handle a URL and no relevant snapshot is available, it displays the launch image you specify. To specify a launch image, provide a PNG image whose name uses the following naming conventions:

`<basename>-<url_scheme><other_modifiers>.png`

In this naming convention, basename represents the base image name specified by the UILaunchImageFile key in your app’s Info.plist file. If you do not specify a custom base name, use the string Default. The <url_scheme> portion of the name is your URL scheme name. To specify a generic launch image for the myapp URL scheme, you would include an image file with the name Default-myapp@2x.png in the app’s bundle. (The @2x modifier signifies that the image is intended for Retina displays. If your app also supports standard resolution displays, you would also provide a Default-myapp.png image.)

## DCURLRouter 项目只是使用URL方式重新排布了一下ViewController之间的跳转关系

