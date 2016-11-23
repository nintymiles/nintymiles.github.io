---
layout: post
title: Mac OS Sierra启动盘的制作
---
## Mac OS Sierra启动盘的制作
苹果安装包位于移动硬盘根目录下,安装包下 `Install\ macOS\ Sierra.app／Contents/Resources/createinstallmedia `为具体的安装媒介制作工具

```
localhost:Volumes seanren$ cd /Volumes/MyPassport2T/Install\ macOS\ Sierra.app
localhost:Install macOS Sierra.app seanren$ ls
Contents
localhost:Install macOS Sierra.app seanren$ cd Contents/
localhost:Contents seanren$ cd Resources/
localhost:Resources seanren$ sudo ./createinstallmedia --volume /Volumes/MacBoot/ --applicationpath /Volumes/MyPassport2T/Install\ macOS\ Sierra.app  --nointeraction  //实际执行命名及参数 --applicationpath 指安装包所在路径
Erasing Disk: 0%... 10%... 20%... 30%...100%...
Copying installer files to disk...

Copy complete.
Making disk bootable...
Copying boot files...
Copy complete.
Done. 
```

