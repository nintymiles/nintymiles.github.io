---
layout: post
title:  Learning Notes - Jenkins iOS build setting 
---

## Jenkins XCode Plugin 的一些设置

- Subversion插件的Module设置
	
	***Local module directory***一般设置为 **"."**,若设置为其他名称，则会在check out时新建对目录。一般check out的路径（mac）为**/Users/Shared/Jenkins/Home/workspace/zbj_ios_app_project／xxx（若设置则有）	**	
	
- Advanced Xcode build options 设置

	**Xcode Workspace File**＝HL_App_Tzbao，此处指定的是HL_App_Tzbao.xcworkspace 文件
	**Xcode Project Directory**＝HL_App_Tzbao，此处指的是项目目录。但此处也可以用于将有些偏离的工作目录指定到当前**project**或**workspace**根目录
	**Xcode Schema File**＝＝HL_App_Tzbao，此处指＝HL_App_Tzbao.xcscheme文件， 有时候使用pod时，会对单Project项目将workspace和project放置在同目录，此时cocoapods可能并没有对project产生对应的scheme 文件，那么当指定Workspace File时，此处也会报错，需要手工产生项目对应的scheme。
	
- General Tab 的 ***Use custom workspace*** 条目，应该也可以设置working directory。可以代替`Xcode Project Directory`的work around 用法
	
## Jenkins Log
下面时一些Jenkins log，显示的原理是底层基本通过XCodeBuild工具集来实现。注意一些常用的build 命令。
- /usr/bin/xcodebuild -version
- /usr/bin/xcodebuild -showsdks
- /usr/bin/xcodebuild -list
- /usr/bin/security find-identity -p codesigning -v
- /usr/bin/agvtool mvers -terse1

```
Working directory is /Users/Shared/Jenkins/Home/workspace/zbj_ios_app_project/HL_App_Tzbao.

Working directory is /Users/Shared/Jenkins/Home/workspace/zbj_ios_app_project/HL_App_Tzbao.
[HL_App_Tzbao] $ /usr/bin/xcodebuild -version
Xcode 7.3.1
Build version 7D1014
Fetching marketing version number (CFBundleShortVersionString) from project.
[HL_App_Tzbao] $ /usr/bin/agvtool mvers -terse1
Found marketing version (CFBundleShortVersionString): 1.0.
Marketing version (CFBundleShortVersionString) found in project configuration: 1.0.
Fetching technical version number (CFBundleVersion) from project.
[HL_App_Tzbao] $ /usr/bin/agvtool vers -terse
No marketing version found (CFBundleVersion)
Technical version (CFBundleVersion) found in project configuration: .
Marketing version (CFBundleShortVersionString) used by Jenkins to produce the IPA: 1.0
Technical version (CFBundleVersion) used by Jenkins to produce the IPA: 
===========================================================
== Available provisioning profiles
[HL_App_Tzbao] $ /usr/bin/security find-identity -p codesigning -v
     0 valid identities found
== Available SDKs
[HL_App_Tzbao] $ /usr/bin/xcodebuild -showsdks
OS X SDKs:
	OS X 10.11                    	-sdk macosx10.11

iOS SDKs:
	iOS 9.3                       	-sdk iphoneos9.3

iOS Simulator SDKs:
	Simulator - iOS 9.3           	-sdk iphonesimulator9.3

tvOS SDKs:
	tvOS 9.2                      	-sdk appletvos9.2

tvOS Simulator SDKs:
	Simulator - tvOS 9.2          	-sdk appletvsimulator9.2

watchOS SDKs:
	watchOS 2.2                   	-sdk watchos2.2

watchOS Simulator SDKs:
	Simulator - watchOS 2.2       	-sdk watchsimulator2.2

== Available schemes
[HL_App_Tzbao] $ /usr/bin/xcodebuild -list -workspace HL_App_Tzbao.xcworkspace
Information about workspace "HL_App_Tzbao":
    Schemes:
        Charts-iOS
        Charts-OSX
        Charts-TV
        HL_App_Tzbao


===========================================================
Going to invoke xcodebuild:, scheme: HL_App_Tzbao, sdk: DEFAULT, workspace: HL_App_Tzbao, configuration: Debug, clean: NO, archive:NO, symRoot: DEFAULT, configurationBuildDir: DEFAULT, codeSignIdentity: DEFAULT
[HL_App_Tzbao] $ /usr/bin/xcodebuild -scheme HL_App_Tzbao -workspace HL_App_Tzbao.xcworkspace -configuration Debug build
=== BUILD TARGET Charts-iOS OF PROJECT Charts WITH CONFIGURATION Debug ===

Check dependencies
Code Sign error: No code signing identities found: No valid signing identities (i.e. certificate and private key pair) were found.

** BUILD FAILED **


The following build commands failed:
	Check dependencies
(1 failure)
Build step 'Xcode' marked build as failure
Finished: FAILURE	
```
	

## 关于 Gradle plugin 的一些设置
Android构建时Gradle plugin的设置最为重要。首先在系统级的`global tool config`设置gradle路径并命名。 然后在andorid的build设置中选择`invoke gradle script`，之后在相应的选项卡中选择设置的gradle plugin名称。

> 注： Android gradle构建时最重要的属性时`local.properties`，其中指定了Android SDK路径，其他地方的设置应该是无效的。Mac搭建环境时，尤其要注意svn上的local.properties属性是否正确。

