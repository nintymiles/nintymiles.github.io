---
layout: post
title:  Learning Notes - iOS Path Constant Definitions
---
## iOS Path Constant Definitions

```
typedef NS_ENUM(NSUInteger, NSSearchPathDirectory) {
    NSApplicationDirectory = 1,             // supported applications (Applications)
    NSDemoApplicationDirectory,             // unsupported applications, demonstration versions (Demos)
    NSDeveloperApplicationDirectory,        // developer applications (Developer/Applications). DEPRECATED - there is no one single Developer directory.
    NSAdminApplicationDirectory,            // system and network administration applications (Administration)
    NSLibraryDirectory,                     // various documentation, support, and configuration files, resources (Library)
    NSDeveloperDirectory,                   // developer resources (Developer) DEPRECATED - there is no one single Developer directory.
    NSUserDirectory,                        // user home directories (Users)
    NSDocumentationDirectory,               // documentation (Documentation)
    NSDocumentDirectory,                    // documents (Documents)
    NSCoreServiceDirectory,                 // location of CoreServices directory (System/Library/CoreServices)
    NSAutosavedInformationDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 11,   // location of autosaved documents (Documents/Autosaved)
    NSDesktopDirectory = 12,                // location of user's desktop
    NSCachesDirectory = 13,                 // location of discardable cache files (Library/Caches)
    NSApplicationSupportDirectory = 14,     // location of application support files (plug-ins, etc) (Library/Application Support)
    NSDownloadsDirectory NS_ENUM_AVAILABLE(10_5, 2_0) = 15,              // location of the user's "Downloads" directory
    NSInputMethodsDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 16,           // input methods (Library/Input Methods)
    NSMoviesDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 17,                 // location of user's Movies directory (~/Movies)
    NSMusicDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 18,                  // location of user's Music directory (~/Music)
    NSPicturesDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 19,               // location of user's Pictures directory (~/Pictures)
    NSPrinterDescriptionDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 20,     // location of system's PPDs directory (Library/Printers/PPDs)
    NSSharedPublicDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 21,           // location of user's Public sharing directory (~/Public)
    NSPreferencePanesDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 22,        // location of the PreferencePanes directory for use with System Preferences (Library/PreferencePanes)
    NSApplicationScriptsDirectory NS_ENUM_AVAILABLE(10_8, NA) = 23,      // location of the user scripts folder for the calling application (~/Library/Application Scripts/code-signing-id)
    NSItemReplacementDirectory NS_ENUM_AVAILABLE(10_6, 4_0) = 99,	    // For use with NSFileManager's URLForDirectory:inDomain:appropriateForURL:create:error:
    NSAllApplicationsDirectory = 100,       // all directories where applications can occur
    NSAllLibrariesDirectory = 101,          // all directories where resources can occur
    NSTrashDirectory NS_ENUM_AVAILABLE(10_8, NA) = 102                   // location of Trash directory

}; 
```


## Mac的启动日志在 Console 工具中查看

Mac查看开机时间

终端下：uptime

或者
ps -ax -o etime,command -c | grep Finder

ps：后者好像更准确一些，因为计算finder的运行时间是不受休眠影响的，只要不关闭程序，都会折算的。而Finder进程，只要打开mac就会运行的。

