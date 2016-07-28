---
layout: post
title:  Learning Notes - Jenkins Android build setting 
---
	
## 关于 Gradle plugin 的一些设置
Android构建时Gradle plugin的设置最为重要。首先在系统级的`global tool config`设置gradle路径并命名。 然后在andorid的build设置中选择`invoke gradle script`，之后在相应的选项卡中选择设置的gradle plugin名称。

> 注： Android gradle构建时最重要的属性时`local.properties`，其中指定了Android SDK路径，其他地方的设置应该是无效的。Mac搭建环境时，尤其要注意svn上的local.properties属性是否正确。

