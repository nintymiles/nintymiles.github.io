---
layout: post
title:  Learning Notes - A few of Swift Initializers
---

## How to uninstall Jenkins(installed by pkg) on Mac ?

- sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist

	launchctl interfaces with launchd to manage and inspect daemons, agents
     and XPC services. launchctl 为 launchd 的管理接口，可以管理和监控daemons,agents和XPC服务
     
	```
	比如，如果你需要停止Spotlight服务，可以运行下面的命令：
	  launchctl unload /System/Library/LaunchDaemons/com.apple.metadata.mds.plist
	停止之后又要启动该服务，那么：
	  launchctl load /System/Library/LaunchDaemons/com.apple.metadata.mds.plist
	
	launchd是在OS X启动的时候又系统启动的，它根据自己的控制文件来启动相应的服务程序，而一般来说它的控制文件是：
	$HOME/.launchd.conf 或者 /etc/launchd.conf
	```

- sudo rm !$

- sudo rm -rf /Applications/Jenkins "/Library/Application Support/Jenkins" /Library/Documentation/Jenkins

	删除Jenkins配置及安装文件目录

- sudo rm -rf /Users/Shared/Jenkins

	删除Jenkins工作目录

- if you want to get rid of all the jobs and builds:

	sudo dscl . -delete /Users/jenkins
	
	```
	通过目录服务命令操作用户和组
	
	dscl is a general-purpose utility for operating on Directory Service
     directory nodes.  Its commands allow one to create, read, and manage
     Directory Service data.  If invoked without any commands, dscl runs in an
     interactive mode, reading commands from standard input.  Interactive pro-
     cessing is terminated by the quit command.  Leading dashes ("-") are
     optional for all commands.
	
	#创建节点
	dscl localhost -create /Local/Default/Users/newborn
	#指定login shell
	dscl localhost -create /Local/Default/Users/newborn UserShell /bin/bash
	#指定的（RealName）名字会在登陆界面上显示
	dscl localhost -create /Local/Default/Users/newborn RealName "Newborn John"
	#指定UID
	dscl localhost -create /Local/Default/Users/newborn UniqueID 502
	#指定GID
	dscl localhost -create /Local/Default/Users/newborn PrimaryGroupID 1000
	#创建家目录
	dscl localhost -create /Local/Default/Users/newborn NFSHomeDirectory /Users/newborn
	#设定密码
	dscl localhost -passwd /Local/Default/Users/newborn [define a password here]
	#或运行passwd [username]，输入两次密码
	passwd newborn
	```

- delete the jenkins user and group (if you chose to use them):

	sudo dscl . -delete /Groups/jenkins


## 通过brew安装及使用jenkins
`$ brew install jenkins`

```
启动jenkins
$ jenkins
```

```
卸载jenkins
$ brew uninstall jenkins
```

```
brew无效？
安装homebrew
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```


```
brew install jenkins

==> Downloading http://mirrors.jenkins-ci.org/war/1.577/jenkins.war

######################################################################## 100.0%

==> Caveats

Note: When using launchctl the port will be 8080.

To have launchd start jenkins at login:

    ln -sfv /usr/local/opt/jenkins/*.plist ~/Library/LaunchAgents

Then to load jenkins now:

    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist
```




