---
layout: post
title:  Learning Notes - Jenkins Android build setting on Mac (Jenkins installed by pkg)
---
	
## Jenkins Android 构建的通用设置
在 Global tool configuration 中设置JDK，Mac中JDK的路径样本`/Library//Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home/`

> 通过Unix命令查找java路径 `find /Library/ -name 'java'`
	
### 关于 Gradle plugin 的路径设置
Android构建时Gradle plugin的设置最为重要。首先在系统级的`Global Tool Configuration`设置gradle路径并命名。 然后在andorid的build设置中选择`invoke gradle script`，之后在相应的选项卡中选择设置的gradle plugin名称。

> 注： Android gradle构建时最重要的属性时`local.properties`，其中指定了Android SDK路径，其他地方的设置应该是无效的。Mac搭建环境时，尤其要注意svn上的local.properties属性是否正确。


## Jenkins gradle build项设置
一些设置的填写十分重要，并不是所有的选项都有默认值。

![jenkins-android-grandle-config](/asset/technical/jenkins-android-grandle-config.png)

下面是build成功的日志截图

![jenkins-android-gradle-log-1](/asset/technical/jenkins-android-gradle-log-1.png)

Jenkins Android Workspace截图

![jenkins-workspace-1](/asset/technical/jenkins-workspace-1.png)

![jenkins-workspace-2](/asset/technical/jenkins-workspace-2.png)


## Mac上Jenkins的Android路径配置(此处问题主要出现在Jenkins使用Pkg以**独立用户**安装的情形)
由于Jenkins使用了专有用户，因此Android SDK安装目录不能在Jenkins之外的用户目录中，否则会导致Jenkins无法找到Android SDK目录（权限问题）。在Android Plugin的路径配置和Gradle的local.propertis文件中都需要注意。

## Android Lint工具
Android Lint是SDK Tools 16 之后才引入的工具，通过它可以对Android工程源代码进行扫描和检查，发现潜在的问题，以便程序员及早修正这个问题。Android Lint提供了命令行方式执行，还可与IDE集成，并提供了html形式的输出报告。




