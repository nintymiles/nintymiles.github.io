---
layout: post
title:  Learning Notes - Carthage Introduce
---

## What is Carthage?
A simple,**decentralized** *dependency* manager for cocoa.

## Carthage 对 iOS/OS X 的支持？
Carthage 支持 OS X 的各个版本，但是对于 iOS 只支持 iOS 8 及其以上
> officially only supports dynamic frameworks. Dynamic frameworks can be used on any version of OS X, but only on iOS 8 or later. 

Carthage 是由 Swift 语言写的，只支持动态框架，只支持 iOS8+。 目前只支持github 或者git 相关管理源的库

## Carthage 和 CocoaPods 的区别
CoaoaPods 是一套整体解决方案，我们在 Podfile 中指定好我们需要的第三方库。然后 CocoaPods 就会进行下载，集成，然后修改或者创建我们项目的 workspace 文件，这一系列整体操作。

相比之下，Carthage 就要轻量很多，它也会一个叫做 Cartfile 描述文件，但 Carthage 不会对我们的项目结构进行任何修改，更不多创建 workspace。它只是根据我们描述文件中配置的第三方库，将他们下载到本地，然后使用 xcodebuild 构建成 framework 文件。然后由我们自己将这些库集成到项目中。Carthage 使用的是一种非侵入性的哲学。

## Carthage 的 decentralized 哲学
Carthage是去中心化的，它的包管理不像 CocoaPods 那样，有一个中心服务器(cocoapods.org)，来管理各个包的元信息，而是依赖于每个第三方库自己的源地址，比如 Github。这样也是有利有弊，好处就是我们对包管理不再依赖中心服务器，不会受中心服务器信息量和稳定性的限制(尤其是非网络的404问题)，弊端就是我们想查找第三方库的时候，没有一个中心服务器来帮助我们进行索引，而是必须从网络上自行查找。

## 以 SnapKit 为例如何使用carthage
首先使用brew安装Carthage

```
$ brew update
$ brew install carthage
```

其次在XCode项目根目录下建立Cartfile，然后指定github中SanpKit库的位置

```
github "SnapKit/SnapKit" >= 0.15.0
```

