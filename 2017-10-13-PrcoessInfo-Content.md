---
layout: post
title:  ProcessInfo(NSProcessInfo)包含什么
--- 
## ProcessInfo(NSProcessInfo)包含什么
- NSProcessInfo - A collection of information about the current process.
 Each process has a single, shared ProcessInfo object, known as a **process information agent**.
 The process information agent can return information such as arguments, environment variables, host name, and process name. The processInfo class method returns the shared agent for the current process

```
(lldb) po ProcessInfo.processInfo.hostName
"xxxmacbook-pro.local"

(lldb) po ProcessInfo.processInfo.operatingSystemVersion
▿ OperatingSystemVersion
  - majorVersion : 11
  - minorVersion : 0
  - patchVersion : 0

(lldb) po ProcessInfo.processInfo.globallyUniqueString
"5A7B2872-7A3F-493C-B8D9-5142C3205675-1806-000002403C38200E"

(lldb) po ProcessInfo.processInfo.operatingSystemVersionString
"Version 11.0 (Build 15A372)"

(lldb) po ProcessInfo.processInfo.isLowPowerModeEnabled
false
(lldb) po ProcessInfo.processInfo.arguments
▿ 1 element
  - 0 : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/Containers/Bundle/Application/0AFE0C9B-6CBC-44C4-BB2C-3903A13BC50B/Client.app/Client"

(lldb) po ProcessInfo.processInfo
<NSProcessInfo: 0x604000253290>

(lldb) po ProcessInfo.processInfo.environment
▿ 48 elements
  ▿ 0 : 2 elements
    - key : "__XPC_DYLD_FRAMEWORK_PATH"
    - value : "/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Fennec-iphonesimulator:/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Release-iphonesimulator"
  ▿ 1 : 2 elements
    - key : "DYLD_ROOT_PATH"
    - value : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot"
  ▿ 2 : 2 elements
    - key : "SIMULATOR_AUDIO_DEVICES_PLIST_PATH"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/var/run/com.apple.coresimulator.audio.plist"
  ▿ 3 : 2 elements
    - key : "SIMULATOR_MAINSCREEN_WIDTH"
    - value : "1125"
  ▿ 4 : 2 elements
    - key : "IPHONE_SHARED_RESOURCES_DIRECTORY"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data"
  ▿ 5 : 2 elements
    - key : "SIMULATOR_AUDIO_SETTINGS_PATH"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/var/run/simulatoraudio/audiosettings.plist"
  ▿ 6 : 2 elements
    - key : "DYLD_LIBRARY_PATH"
    - value : "/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Fennec-iphonesimulator:/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Release-iphonesimulator:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/system/introspection"
  ▿ 7 : 2 elements
    - key : "SIMULATOR_MAINSCREEN_SCALE"
    - value : "3.000000"
  ▿ 8 : 2 elements
    - key : "DYLD_FALLBACK_FRAMEWORK_PATH"
    - value : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/System/Library/Frameworks"
  ▿ 9 : 2 elements
    - key : "SIMULATOR_MEMORY_WARNINGS"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/var/run/memory_warning_simulation"
  ▿ 10 : 2 elements
    - key : "OS_ACTIVITY_MODE"
    - value : "disable"
  ▿ 11 : 2 elements
    - key : "CLASSIC"
    - value : "0"
  ▿ 12 : 2 elements
    - key : "SIMULATOR_MAINSCREEN_HEIGHT"
    - value : "2436"
  ▿ 13 : 2 elements
    - key : "SIMULATOR_HID_SYSTEM_MANAGER"
    - value : "/Library/Developer/PrivateFrameworks/CoreSimulator.framework/Resources/Platforms/iphoneos/Library/Frameworks/SimulatorHID.framework"
  ▿ 14 : 2 elements
    - key : "IPHONE_SIMULATOR_ROOT"
    - value : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot"
  ▿ 15 : 2 elements
    - key : "XPC_SIMULATOR_LAUNCHD_NAME"
    - value : "com.apple.CoreSimulator.SimDevice.3020EA5B-7D75-408B-92C7-63009AE2CFED"
  ▿ 16 : 2 elements
    - key : "XPC_FLAGS"
    - value : "0x0"
  ▿ 17 : 2 elements
    - key : "SIMULATOR_DEVICE_NAME"
    - value : "iPhone X"
  ▿ 18 : 2 elements
    - key : "XPC_SERVICE_NAME"
    - value : "UIKitApplication:org.mozilla.ios.Fennec[0x54ea][1719]"
  ▿ 19 : 2 elements
    - key : "SIMULATOR_LEGACY_ASSET_SUFFIX"
    - value : "iphone"
  ▿ 20 : 2 elements
    - key : "SIMULATOR_RUNTIME_VERSION"
    - value : "11.0"
  ▿ 21 : 2 elements
    - key : "SIMULATOR_LOG_ROOT"
    - value : "/Users/xxx/Library/Logs/CoreSimulator/3020EA5B-7D75-408B-92C7-63009AE2CFED"
  ▿ 22 : 2 elements
    - key : "PATH"
    - value : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/bin:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/bin:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/sbin:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/sbin:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/local/bin"
  ▿ 23 : 2 elements
    - key : "TESTMANAGERD_SIM_SOCK"
    - value : "/private/tmp/com.apple.launchd.Hgygch0eiO/com.apple.testmanagerd.unix-domain.socket"
  ▿ 24 : 2 elements
    - key : "SIMULATOR_SHARED_RESOURCES_DIRECTORY"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data"
  ▿ 25 : 2 elements
    - key : "SIMULATOR_VERSION_INFO"
    - value : "CoreSimulator 494.13.6 - Device: iPhone X - Runtime: iOS 11.0 (15A372) - DeviceType: iPhone2017-C"
  ▿ 26 : 2 elements
    - key : "SIMULATOR_RUNTIME_BUILD_VERSION"
    - value : "15A372"
  ▿ 27 : 2 elements
    - key : "SIMULATOR_CAPABILITIES"
    - value : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/DeviceTypes/iPhone2017-C.simdevicetype/Contents/Resources/capabilities.plist"
  ▿ 28 : 2 elements
    - key : "NSUnbufferedIO"
    - value : "YES"
  ▿ 29 : 2 elements
    - key : "SIMULATOR_MAINSCREEN_PITCH"
    - value : "326.000000"
  ▿ 30 : 2 elements
    - key : "HOME"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/Containers/Data/Application/C3FD91DE-18A1-4D70-B65C-70B1257B2117"
  ▿ 31 : 2 elements
    - key : "OS_ACTIVITY_DT_MODE"
    - value : "YES"
  ▿ 32 : 2 elements
    - key : "SIMULATOR_MODEL_IDENTIFIER"
    - value : "iPhone10,3"
  ▿ 33 : 2 elements
    - key : "IOS_SIMULATOR_SYSLOG_SOCKET"
    - value : "/private/tmp/com.apple.CoreSimulator.SimDevice.3020EA5B-7D75-408B-92C7-63009AE2CFED/syslogsock"
  ▿ 34 : 2 elements
    - key : "IPHONE_TVOUT_EXTENDED_PROPERTIES"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/Library/Application Support/Simulator/extended_display.plist"
  ▿ 35 : 2 elements
    - key : "TMPDIR"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/Containers/Data/Application/C3FD91DE-18A1-4D70-B65C-70B1257B2117/tmp"
  ▿ 36 : 2 elements
    - key : "CUPS_SERVER"
    - value : "/private/tmp/com.apple.launchd.abCcw5cBKH/Listeners"
  ▿ 37 : 2 elements
    - key : "__XPC_DYLD_LIBRARY_PATH"
    - value : "/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Fennec-iphonesimulator:/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Release-iphonesimulator"
  ▿ 38 : 2 elements
    - key : "SIMULATOR_HOST_HOME"
    - value : "/Users/xxx"
  ▿ 39 : 2 elements
    - key : "SIMULATOR_BOOT_TIME"
    - value : "1507856570"
  ▿ 40 : 2 elements
    - key : "SIMULATOR_PRODUCT_CLASS"
    - value : "D22"
  ▿ 41 : 2 elements
    - key : "SIMULATOR_EXTENDED_DISPLAY_PROPERTIES"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/Library/Application Support/Simulator/extended_display.plist"
  ▿ 42 : 2 elements
    - key : "DYLD_FALLBACK_LIBRARY_PATH"
    - value : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib"
  ▿ 43 : 2 elements
    - key : "SIMULATOR_UDID"
    - value : "3020EA5B-7D75-408B-92C7-63009AE2CFED"
  ▿ 44 : 2 elements
    - key : "DYLD_FRAMEWORK_PATH"
    - value : "/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Fennec-iphonesimulator:/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Release-iphonesimulator"
  ▿ 45 : 2 elements
    - key : "__XCODE_BUILT_PRODUCTS_DIR_PATHS"
    - value : "/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Fennec-iphonesimulator:/Users/xxx/Library/Developer/Xcode/DerivedData/Client-dmyrrxhvvdhulmcgravflslmuwoa/Build/Products/Release-iphonesimulator"
  ▿ 46 : 2 elements
    - key : "SIMULATOR_ROOT"
    - value : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot"
  ▿ 47 : 2 elements
    - key : "CFFIXED_USER_HOME"
    - value : "/Users/xxx/Library/Developer/CoreSimulator/Devices/3020EA5B-7D75-408B-92C7-63009AE2CFED/data/Containers/Data/Application/C3FD91DE-18A1-4D70-B65C-70B1257B2117"
```

