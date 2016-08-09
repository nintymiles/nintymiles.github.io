---
layout: post
title:  Learning Notes - About Processing Symbol Files In XCode
---

## What is symbols files when plug in iOS device on XCode? 
XCode downloads the (debug) symbols from the device, so it is possible to debug on devices with that specific iOS version and also to symbolicate crash reports that happened on that iOS version.

Since symbols are cpu specific, the above only works if you have imported the symbols not only for a specific iOS device but also for a specific CPU type. The currently CPU types needed are armv7 (e.g. iPhone 4, iPhone 4s), armv7s (e.g. iPhone 5) and arm64 (e.g. iPhone 5s).

So if you want to symbolicate a crash report that happened on an iPhone 5 with armv7s and only have the symbols for armv7 for that specific iOS version, Xcode won't be able to (fully) symbolicate the crash report. 

In Xcode, after "Processing Symbol Files", a folder containing symbols associated with the device (including iOS version and CPU type) was created in ~/Library/Developer/Xcode/iOS DeviceSupport/ like this:

![xcode_processing_symbol_files](/asset/technical/xcode_processing_symbol_files.png)


