---
layout: post
title:  Learning Notes - Jenkins iOS build setting 
---

## Jenkins iOS 构建设置过程（注解截图）
使用Jenkins构建iOS App时要注意安装方式，一定要使用***brew install jenkins***的途径来安装。千万不要使用pkg包的方式安装，使用pkg包的方式安装时将会为jenkins建立独立用户和组，然后将会因为权限问题埋下很多大坑。

同时需要安装两个额外插件：

1. [CocoaPods Jenkins Integration](https://wiki.jenkins-ci.org/display/JENKINS/CocoaPods+Integration)
2. [Xcode integration](https://wiki.jenkins-ci.org/display/JENKINS/Xcode+Plugin)

Jenkins构建iOS APP时实现的过程大概是这样的，首先check out项目到工作目录，然后使用cocoapods更新依赖库，随后通过解锁当前用户keychain的方式获取已经设置到本机的 code signing 信息（已经使用xcode至少成功的debug building到真机上或者成功exported ipa），最后使用xcodebuild命令行构建出iOS App。

**具体设置过程如下图**：

![jenkins-ios-setting-1](/asset/technical/jenkins-ios/jenkins-ios-setting-1.jpg)

![jenkins-ios-setting-2](/asset/technical/jenkins-ios/jenkins-ios-setting-2.jpg)

![jenkins-ios-setting-3](/asset/technical/jenkins-ios/jenkins-ios-setting-3.jpg)

![jenkins-ios-setting-4](/asset/technical/jenkins-ios/jenkins-ios-setting-4.jpg)

![jenkins-ios-setting-5](/asset/technical/jenkins-ios/jenkins-ios-setting-5.jpg)

![jenkins-ios-setting-6](/asset/technical/jenkins-ios/jenkins-ios-setting-6.jpg)

## Jenkins 设置的一些注意事项

- General Tab 的 ***Use custom workspace*** 条目，可以设置***working directory***。

	> 当然也可以通过XCode Integration的Advanced Xcode build options的project directory来达到workaround的设置效果

- Subversion插件的Module设置
	
	***Local module directory***一般设置为 **"."**,若设置为其他名称，则会在check out时新建对目录。一般check out的路径（mac）为**/Users/Shared/Jenkins/Home/workspace/zbj_ios_app_project／xxx（若设置则有）	**	

## XCode Plugin 设置的一些注意事项	

- Advanced Xcode build options 设置

	**Xcode Workspace File**＝HL_App_Tzbao，此处指定的是HL_App_Tzbao.xcworkspace 文件
	**Xcode Project Directory**＝HL_App_Tzbao，此处指的是项目目录。但此处也可以用于将有些偏离的工作目录指定到当前**project**或**workspace**根目录
	**Xcode Schema File**＝＝HL_App_Tzbao，此处指＝HL_App_Tzbao.xcscheme文件， 有时候使用pod时，会对单Project项目将workspace和project放置在同目录，此时cocoapods可能并没有对project产生对应的scheme 文件，那么当指定Workspace File时，此处也会报错，需要手工产生项目对应的scheme。

> XCode的Identity->Team 的**有效性检验**及设置跟XCode Project没直接关系，也就是不体现在project文件设置上。
	
## Jenkins 部分xcodebuild Log分析
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


