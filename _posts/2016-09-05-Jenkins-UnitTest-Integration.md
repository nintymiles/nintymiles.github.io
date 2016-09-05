---
layout: post
title:  Learning Notes - Jenkins如何整合iOS单元测试
---
## 在Jenkins中集成iOS单元测试的两个基础步骤
1. 在build中添加`Execute Shell`，命令如下：

	```
	xcodebuild test -workspace HL_App_Tzbao.xcworkspace -scheme HL_App_Tzbao -sdk iphonesimulator9.3 -destination OS=9.3,name="iPhone 6 Plus"  -configuration Debug  2>&1 | ocunit2junit 
	```
	
	> ocunit2junit 也是一个关键步骤，xctest的测试结果不能被jenkins识别，需要使用`brew install ocunit2junit`安装此工具来进行格式转换。
	
2. 在Post-build Actions中添加Publish Junit test result report，填入如下键值即可：

		Test report XMLs ＝ `test-reports/*.xml`
		
		
	> jenkins 可以解析Junit测试结果，并以适当形式展示。


 **令人疑惑的地方**： Jenkins 更新 cocoapods 时，如果pod中AFNetworking没有指定版本号，则此处的pod更新时会选用可能已经存在的低版本的AFNetowrking库。

