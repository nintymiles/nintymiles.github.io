---
layout: post
title:  Swift Learning Notes - 如何使用 carthage，以整合 corestore 为例
---
## How to use corestore?
`corestore`支持swift2.3的版本目前为2.1.0. 使用下面carthage脚本安装。

```
github "JohnEstropia/CoreStore" >= 2.1.0
github "JohnEstropia/GCDKit" >= 1.3.0

carthage update 
```

> 注：carthage更新后，需要手工指定动态库。
> 注2: carthage生成的xcodebuild脚本会因为VVDocumenter和Alcartez插件的影响而无法编译corestone。需要在`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/`目录中去除影响编译的插件。

### Swift项目`link library with libraries`引入`corestone` framework 默认状态（required）时出现的运行错误？
错误信息如下：

```
dyld: Library not loaded: @rpath/CoreStore.framework/CoreStore
  Referenced from: /Users/seanren/Library/Developer/CoreSimulator/Devices/D538998F-89E9-44CA-8478-76DA890EA8B6/data/Containers/Bundle/Application/45E49DE6-2377-432A-BA02-34D0ED27738B/HanziStrokingSwift.app/HanziStrokingSwift

Reason: image not found 
```

> 如果`link library with libraries`时改变`corestone`的`status`为`optional` 时，则不出现运行错误。（此时程序中尚未调用`corestone`,若调用，库还是无法使用）。

#### 解决 `Library not loaded` 的一种途径（？需要找到正常途径）
在 `Build Phases` 中新建 `Copy Files` （Destination 指定为 Framework ，然后类似`link library`加入`carthage building`的指定framework），施加此动作则库可以正常使用。


### CoreStore的列表监控机制
`CoreStore`的`ListMonitor`等价于`NSFetchedResultController`，重新实现的一套机制？


