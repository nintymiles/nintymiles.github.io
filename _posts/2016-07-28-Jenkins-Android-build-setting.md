---
layout: post
title:  Learning Notes - Jenkins Android build setting on Mac
---
	
## 关于 Gradle plugin 的一些设置
Android构建时Gradle plugin的设置最为重要。首先在系统级的`global tool config`设置gradle路径并命名。 然后在andorid的build设置中选择`invoke gradle script`，之后在相应的选项卡中选择设置的gradle plugin名称。

> 注： Android gradle构建时最重要的属性时`local.properties`，其中指定了Android SDK路径，其他地方的设置应该是无效的。Mac搭建环境时，尤其要注意svn上的local.properties属性是否正确。

## Mac上Jenkins的Android路径配置
由于Jenkins使用了专有用户，因此Android SDK安装目录不能在Jenkins之外的用户目录中，否则会导致Jenkins无法找到Android SDK目录（权限问题）。在Android Plugin的路径配置和Gradle的local.propertis文件中都需要注意。

## Jenkins gradle build项设置
一些设置的填写十分重要，并不是所有的选项都有默认值。

![jenkins-android-grandle-config](/asset/technical/jenkins-android-grandle-config.png)

下面是build成功的日志截图

![jenkins-android-gradle-log-1](/asset/technical/jenkins-android-gradle-log-1.png)

Jenkins Android Workspace截图

![jenkins-workspace-1](/asset/technical/jenkins-workspace-1.png)

![jenkins-workspace-2](/asset/technical/jenkins-workspace-2.png)







