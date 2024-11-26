---
layout: post
title:  Swift条件编译需要设置OTHER-SWIFT-FLAGS参数
---
## Swift Condtion Flag的设置
在Swift中如果要实现条件编译，需要设置Swift-Compiler的OTHER_SWIFT_FLAGS参数。

```
//:configuration = FirefoxBeta
OTHER_SWIFT_FLAGS = -DMOZ_CHANNEL_BETA

//:configuration = Firefox
OTHER_SWIFT_FLAGS = -DMOZ_CHANNEL_RELEASE

//:configuration = Fennec_Enterprise
OTHER_SWIFT_FLAGS = -DMOZ_CHANNEL_FENNEC

//:configuration = Fennec
OTHER_SWIFT_FLAGS = -DMOZ_CHANNEL_FENNEC

//:completeSettings = some
OTHER_SWIFT_FLAGS
```
设置之后可实现如下的条件编译代码：

```
public static let BuildChannel: AppBuildChannel = {
        #if MOZ_CHANNEL_RELEASE
            return AppBuildChannel.release
        #elseif MOZ_CHANNEL_BETA
            return AppBuildChannel.beta
        #elseif MOZ_CHANNEL_FENNEC
            return AppBuildChannel.developer
        #endif
    }()
```



