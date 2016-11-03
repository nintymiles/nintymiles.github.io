---
layout: post
title: 关于 `iCloud Drive` 目录的本质
---
## 关于 `iCloud Drive` 目录的本质
Mac中 `iCloud Drive` 文件夹中的文件和目录可能都只是一个soft link，包括iBooks的**隐藏**同步目录。具体的文件夹在系统的实际位置可能如下：

```
====这是sketch在icloud drive中的目录====
/Users/seanren/Library/Mobile\ Documents/WUGMZZ5K46~com~bohemiancoding~sketch/Documents 
```

