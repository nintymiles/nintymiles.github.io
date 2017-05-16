---
layout: post
title: How to clone and build Swift Firefox-iOS Project from Github
---

## `Firefox-iOS`项目构建时需要注意的事项
严格按照如下步骤执行：
1. Clone the repository:`git clone https://github.com/mozilla-mobile/firefox-ios`
2. Pull in the project dependencies:
```cd firefox-ios
	 sh ./bootstrap.sh
```
3. Open Client.xcodeproj in Xcode.（前两步没有完成之前，一定不要开始第三步-用IDE打开项目，否则会出现编译错误）
4. Build the Fennec scheme in Xcode.


