---
layout: post
title: 使用Carthage Update Snapkit时碰到的问题
---

## 在cartfile更新Snapkit时，出现的scheme相关错误问题解决？
在snapkit从2.x升级为3.x时，`carthage update`出现 `xcodebuild: error: Scheme SnapKit tvOS is not currently configured for the clean action`时，简单的处理方法时将carthage工作目录`Carthage`下所有相关子目录下`snapkit`目录删除。重新执行update即可解决问题。

此问题可能是由于之前版本遗留的build相关文件导致的，也可以通过如下一些carthage命令来解决，但是不是很明确。

```
 If you remove Carthage/Checkouts/SnapKit 
 and re-run carthage bootstrap or carthage update 
 that should be fixed 
```

