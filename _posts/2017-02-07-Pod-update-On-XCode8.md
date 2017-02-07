---
layout: post
title: XCode8.x更新Pod时出现问题的解决 
---
# Mac OS 10.12 / XCode8.2.1 环境下更新Pod时出现问题的解决？

- 首先当pod版本小于1.2时会出现`Abort trap: 6`错误，如下：

```
localhost:xxx$ pod update
Update all pods
Updating local specs repositories

CocoaPods 1.2.0 is available.
To update use: `sudo gem install cocoapods`

For more information, see https://blog.cocoapods.org and the CHANGELOG for this version at https://github.com/CocoaPods/CocoaPods/releases/tag/1.2.0

Analyzing dependencies
Downloading dependencies
Using AFNetworking (3.1.0)
Using Colours (5.13.0)
Using FCUUID (1.3.1)
Using MJRefresh (3.1.12)
Using MagicalRecord (2.3.2)
Using Masonry (1.0.2)
Installing SnapKit 3.1.2 (was 0.22.0)
Using UICKeyChainStore (2.1.0)
Using WYPopoverController (0.3.9)
Using YYCache (1.0.4)
Using YYImage (1.0.4)
Using YYKeyboardManager (1.0.1)
Using YYModel (1.0.4)
Using YYText (1.0.7)
Using YYWebImage (1.0.5)
Using pop (1.0.9)
Generating Pods project
Abort trap: 6
localhost:HL_App_Zbenjia seanren$ pod update
Update all pods
Updating local specs repositories
```

- 出现上诉问题是因为需要升级pod版本来和xcode交互。使用下面命令卸载和重新安装pod

```
//卸载pods
sudo gem uninstall cocoapods

sudo gem install cocoapods
ERROR:  While executing gem ... (Gem::DependencyError)
    Unable to resolve dependencies: cocoapods requires cocoapods-core (= 1.1.1), cocoapods-downloader (< 2.0, >= 1.1.2), cocoapods-trunk (< 2.0, >= 1.1.1), xcodeproj (< 2.0, >= 1.3.3)
localhost:HL_App_Zbenjia seanren$ sudo gem install cocoapods
ERROR:  While executing gem ... (Gem::DependencyError)
    Unable to resolve dependencies: cocoapods requires cocoapods-core (= 1.1.1), cocoapods-downloader (< 2.0, >= 1.1.2), cocoapods-trunk (< 2.0, >= 1.1.1), xcodeproj (< 2.0, >= 1.3.3)
localhost:HL_App_Zbenjia seanren$ sudo gem install cocoapods

//升级gem，支持pod1.2
sudo gem update --system
Updating rubygems-update
Fetching: rubygems-update-2.6.7.gem (100%)
Successfully installed rubygems-update-2.6.7
Parsing documentation for rubygems-update-2.6.7
Installing ri documentation for rubygems-update-2.6.7
Installing darkfish documentation for rubygems-update-2.6.7
Installing RubyGems 2.6.7
RubyGems 2.6.7 installed
Parsing documentation for rubygems-2.6.7
Installing ri documentation for rubygems-2.6.7

=== 2.6.7 / 2016-09-26

```


- 使用特定命名安装pods1.2.0 ，否则出现`xcodeproj`字样错误

```
> sudo gem install cocoapods
Fetching: nanaimo-0.2.3.gem (100%)
Successfully installed nanaimo-0.2.3
Fetching: claide-1.0.1.gem (100%)
Successfully installed claide-1.0.1
Fetching: CFPropertyList-2.3.5.gem (100%)
Successfully installed CFPropertyList-2.3.5
Fetching: xcodeproj-1.4.2.gem (100%)
ERROR:  While executing gem ... (Errno::EPERM)
    Operation not permitted - /usr/bin/xcodeproj
```

- 正确命令为  `sudo gem install -n /usr/local/bin cocoapods`


```	
localhost:xxx$ sudo gem install -n /usr/local/bin cocoapods
Successfully installed xcodeproj-1.4.2
Fetching: ruby-macho-0.2.6.gem (100%)
Successfully installed ruby-macho-0.2.6
Fetching: molinillo-0.5.5.gem (100%)
Successfully installed molinillo-0.5.5
Fetching: fourflusher-2.0.1.gem (100%)
Successfully installed fourflusher-2.0.1
Fetching: cocoapods-trunk-1.1.2.gem (100%)
Successfully installed cocoapods-trunk-1.1.2
Fetching: cocoapods-downloader-1.1.3.gem (100%)
Successfully installed cocoapods-downloader-1.1.3
Fetching: cocoapods-core-1.2.0.gem (100%)
Successfully installed cocoapods-core-1.2.0
Fetching: cocoapods-1.2.0.gem (100%)
Successfully installed cocoapods-1.2.0
Parsing documentation for xcodeproj-1.4.2
Installing ri documentation for xcodeproj-1.4.2
Parsing documentation for ruby-macho-0.2.6
Installing ri documentation for ruby-macho-0.2.6
Parsing documentation for molinillo-0.5.5
Installing ri documentation for molinillo-0.5.5
Parsing documentation for fourflusher-2.0.1
Installing ri documentation for fourflusher-2.0.1
Parsing documentation for cocoapods-trunk-1.1.2
Installing ri documentation for cocoapods-trunk-1.1.2
Parsing documentation for cocoapods-downloader-1.1.3
Installing ri documentation for cocoapods-downloader-1.1.3
Parsing documentation for cocoapods-core-1.2.0
Installing ri documentation for cocoapods-core-1.2.0
Parsing documentation for cocoapods-1.2.0
Installing ri documentation for cocoapods-1.2.0
8 gems installed
```

- Pod正常更新时的输出

```

localhost:xxx$ pod update
Update all pods
Updating local specs repositories
Analyzing dependencies
Downloading dependencies
Using AFNetworking (3.1.0)
Using Colours (5.13.0)
Using FCUUID (1.3.1)
Using MJRefresh (3.1.12)
Using MagicalRecord (2.3.2)
Using Masonry (1.0.2)
Installing SnapKit 3.1.2 (was 0.22.0)
Using UICKeyChainStore (2.1.0)
Using WYPopoverController (0.3.9)
Using YYCache (1.0.4)
Using YYImage (1.0.4)
Using YYKeyboardManager (1.0.1)
Using YYModel (1.0.4)
Using YYText (1.0.7)
Using YYWebImage (1.0.5)
Using pop (1.0.9)
Generating Pods project
Integrating client project
Sending stats
Pod installation complete! There are 15 dependencies from the Podfile and 16 total pods installed.
```


