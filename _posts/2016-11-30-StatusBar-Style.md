---
layout: post
title: iOS StatusBar style 设定的关键因素
---
## StatusBar style 设定的关键因素
StatusBar style设定的决定条件都在info.plist中

在项目Info.plist文件中，添加

View controller-based status bar appearance 为 No
Status bar is initially hidden 为 No
Status bar style 为 UIStatusBarStyleDefault

这三个元素都需要设定，这样应该是可以决定全局style。

如果基于ViewController设定style，则第一个元素应该设定为Yes，然后可能要实现回调方法。但可能会受到容器类的影响而无法设定。具体情况具体分析。

```
-(UIStatusBarStyle)preferredStatusBarStyle{
    return UIStatusBarStyleDefault;
} 
```

