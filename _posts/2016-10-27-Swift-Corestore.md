---
layout: post
title:  Swift Learning Notes - 整合corestore
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

### Swift项目引入`corestone` framework 默认状态（required）时出现的运行错误？
错误信息如下：

```
dyld: Library not loaded: @rpath/CoreStore.framework/CoreStore
  Referenced from: /Users/seanren/Library/Developer/CoreSimulator/Devices/D538998F-89E9-44CA-8478-76DA890EA8B6/data/Containers/Bundle/Application/45E49DE6-2377-432A-BA02-34D0ED27738B/HanziStrokingSwift.app/HanziStrokingSwift

Reason: image not found 
```

如果改变`link library with libraries`时改变`corestone`的`status`为`optional` 时，则不出现运行错误。（此时程序中尚未调用`corestone`）。

## CoreStore的列表监控机制
`CoreStore`的`ListMonitor`等价于`NSFetchedResultController`，重新实现的一套机制？


