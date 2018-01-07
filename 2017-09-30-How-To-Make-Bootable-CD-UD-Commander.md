---
layout: post
title: Mac OS 10.1x启动盘制作命令汇集
---
##制作MacOS 10.x启动盘的命令

```
1、要保证下载的原版安装包 **tall OS X *.app在“应用程序”中；
2、大于等于8G的U盘，用磁盘工具分成一个分区，
注意：
（1）GUID分区表；
（2）Mac OS X 扩展（日志式）；
（3）分区名称：OSX（这个可以自定，不过下面终端命令中的OSX也要改成你自定义的同样的名称）


3、终端命令（注意空格，最好是复制粘贴）：

制作OS X El Capitan 原版安装U盘：
sudo /Applications/**tall\ OS\ X\ El\ Capitan.app/Contents/Resources/create**tallmedia --volume /Volumes/OSX --applicationpath /Applications/**tall\ OS\ X\ El\ Capitan.app --nointeraction

4.按回车后，要输入你的系统密码（密码是看不见的），回车--等待完成

5.附上osx 10.12命令：sudo /Applications/**tall\ macOS\ Sierra.app/Contents/Resources/create**tallmedia --volume /Volumes/OSX --applicationpath /Applications/**tall\ macOS\ Sierra.app --nointeraction

6.osx 10.13命令：sudo /Applications/**tall\ macOS\ High\Sierra.app/Contents/Resources/create**tallmedia --volume /Volumes/OSX --applicationpath /Applications/**tall\ macOS\ High\ Sierra.app --nointeraction 
```

