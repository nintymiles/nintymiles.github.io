---
layout: post
title:  Learning Notes - iOS 10 iTunes backup leaks
---
## iOS 10 iTunes backup leaks

对于保存在 PC 上的 iPhone 备份，苹果不知何故采用了一种安全性更弱的哈希算法。这种算法将明文密码转化为所谓的“哈希”，或者说“散列”，简单来说就是由数字和字母组合成的一串字符。任何大小的数据都可以被转化为固定长度的哈希值，而且即使数据只有一点点改动，哈希的结果都会完全不同。可想而知，如果算法越复杂，用户设置的密码也越复杂，那么黑客就越难找到和这条“天书”对应起来的密码明文。暴力破解虽然是个办法，但效率太低。

从 iOS 4 到 iOS 9，苹果都采用的是 **PBKDF2** 算法。PBKDF2 基于迭代复杂度来增加密码安全性，而**过去版本的 iOS 会让明文密码反复执行该算法 10000 次**。如果黑客想要破解，那他就必须要先猜测可能的密码，然后同样反复执行该算法 10000 次。如果最终得到的哈希值不符合，还得重复上述流程，非常繁琐耗时。然而在 iOS 10 中，苹果采用了新的 **SHA256** 算法，而且仅经过一次转化而已。这样一来，破解者当然就轻松太多了。 

## iOS 10 iMessage App Store
新推出的 iMessage App Store 更是一片未开发的处女地，那么什么是 iMessage App Store 呢？iMessage App Store 其实就内置于系统自带的 Messages 应用中，它的使用方式与 App Store 如出一辙，说白了，其实 iMessage App Store 就是 iMessage 的应用商店市场，而苹果很可能要再打造一个除 App Store 之外的重要市场。

