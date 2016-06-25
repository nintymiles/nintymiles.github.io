---
layout: post
title: Learning Notes - 禁用Sublime Text3的升级检测功能以及Twitter缓存印象
---

## 如何禁用sublime text 3的升级检测功能？
使用Sublime Text 3时，每次打开会默认进行更新监测，若有更新则每次都会弹出更新提醒框，若不想更新检测则需要如下步骤：

1. 进入Preferences -\> Settings-User（用户设置） 
2. 在JSON数据格式中（即curly braces（｛｝）所包括区域）添加一句Json格式配置："update\_check":false
3. 然后关闭且重启Submine Text 3即可

## Twitter的视频缓存印象
用twitter浏览一些新闻视频时，观察了一下twitrer的线程所打开的文件和端口。尤其看见了其视频缓存路径/Users/seanren/Library/Containers/com.twitter.twitter-mac/Data/Library/Caches/com.twitter.mac.videos/，当中的视频文件时根据列表中可见视频而进行预加载的。可以看到twitter并没有使用很花哨的手段。