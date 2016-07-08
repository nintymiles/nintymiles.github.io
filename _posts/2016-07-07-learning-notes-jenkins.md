---
layout: post
title:  Learning Notes - Jenkins Concepts
---

## What is Jenkins?
The leading open source **automation server**, Jenkins provides hundreds of plugins to **support building, deploying and automating any project**.  自动化服务器，支持构建、部署、和自动化任何项目。

## 什么是持续集成？
随着软件开发复杂度的不断提高，团队开发成员间如何更好地协同工作以确保软件开发的质量已经慢慢成为开发过程中不可回避的问题。

持续集成正是针对这一类问题的一种软件开发实践。它倡导团队开发成员必须经常集成他们的工作，甚至每天都可能发生多次集成。而每次的集成都是通过自动化的构建来验证，包括**自动编译**、**发布**和**测试**，从而尽快地发现集成错误，让团队能够更快的开发内聚的软件。

持续集成也是**敏捷开发**等软件工程方法的需求。

持续集成的核心价值在于：
- 持续集成中的**任何一个环节都是自动完成**的，无需太多的人工干预，有利于减少重复过程以节省时间、费用和工作量；
- 持续集成保障了**每个时间点上**团队成员提交的代码是能成功集成的。换言之，任何时间点都能第一时间发现软件的集成问题，使任意时间发布可部署的软件成为了可能；
- 持续集成还能利于软件本身的发展趋势，这点在需求不明确或是频繁性变更的情景中尤其重要，持续集成的质量能帮助团队进行有效决策，同时建立团队对开发产品的信心。

持续集成的原则包括：
1. 需要版本控制软件保障团队成员提交的代码不会导致集成失败。常用的版本控制软件有  Git、CVS、Subversion 等；
2. 开发人员必须及时向版本控制库中提交代码，也必须经常性地从版本控制库中更新代码到本地；
3. 需要有专门的集成服务器来执行集成构建。根据项目的具体实际，集成构建可以被软件的修改来直接触发，也可以定时启动，如每半个小时构建一次；
4. 必须保证构建的成功。如果构建失败，修复构建过程中的错误是优先级最高的工作。一旦修复，需要手动启动一次构建。

持续集成系统的组成 ，一个完整的构建系统必须包括：
1. 一个自动构建过程，包括自动编译、分发、部署和测试等。
2. 一个代码存储库，即需要版本控制软件来保障代码的可维护性，同时作为构建过程的素材库。
3. 一个持续集成服务器。Jenkins就是一个配置简单和使用方便的持续集成服务器。

##Jenkins的安装
Jenkins 的安装非常简单，只需要从 Jenkins 的主页上下载最新的 jenkins.war 文件然后运行 java -jar jenkins.war。同时，还可以点击 Jenkins 页面上的 launch 按钮完成下载和运行 Jenkins。

Jenkins 还提供了非常丰富的插件支持，这使得很强大。可以方便的安装各种第三方插件，从而方便快捷的集成第三方的应用。

Jenkins 提供了丰富的管理和配置的功能，包括系统配置、管理插件、查看系统信息、系统日志、节点管理、Jenkins 命令行窗口、信息统计等功能。

The main aim of CI is to prevent integration problems, referred to as "integration hell" in early descriptions of XP. In XP, CI was intended to be used in combination with automated unit tests written through the practices of test-driven development.

## iOS Build Plugin - XCode Plugin
[XCode Plugin](http://wiki.jenkins-ci.org/display/JENKINS/Xcode+Plugin#XcodePlugin-Installationguide)

[CocoaPods Plugin](https://wiki.jenkins-ci.org/display/JENKINS/CocoaPods+Plugin)

### System Requirements
Obviously, the build machine for iOS has to be an OS X machine with XCode developer tools installed.

**Certificates**, **Identities** and **Provisions** must be installed on the build machine separately.

If xcode related binaries aren't stored in the **default** location, update the global configuration of the plugin (Manage Jenkins -> Configure System)

### Source Code Management
Android设置
- Repository URL:svn://192.168.20.91:9999/mobile/android/trunk/HL_app_Tzbao
- Credentials: svn的用户名密码 	
- Local module directory:检出的源码放置的目录	

## Jekins的一些最佳实践记录
Jenkins最佳实践，其实大部分对于其他的CI工具同样的适用：

* Jenkins的安全。对Jenkins的用户使用授权和访问控制。默认地Jenkins不执行任何的安全检查，这意味着任何人都可以访问Jenkins来配置Jenkins，修改job，和执行build。这对于在企业内部使用也许可以接受，但是存在很高的安全风险，例如其他人错误滴删除了job，错误地配置你的job在每分钟运行，启动太多的builds等。所以一般使用plugin来对Jenkins增加授权和访问控制。

* 有规律地对Jenkins的home目录的备份。

* 使用file fingerprinting来管理依赖关系。当在Jenkins上你的job依赖其他的job时，可以使用file fingerprinting来帮助定位依赖的版本信息。

* 最可靠的build是clean builds，clean builds意思是与build相关的所有的3rd party，build脚本，发布说明等都需要在Source code control。

* 与issue tracking系统紧密的集成，例如JIRA或bugzilla，从来减少对change log的修改。

* 与repository浏览工具紧密的集成，例如FishEye如果你使用Subversion作为source code管理工具。

* 总是配置job产生趋势报告和自动化测试，当你运行一个Java build。趋势报告帮助项目经理和开发人员快速地了解当前项目的进度和状态。

* 确保Jenkins的home目录拥有足够的空间。

* 在删除不使用的job前请先存档。

* 为不同的branch建立不同的job，build来尽早地发现错误。

* 为并行的项目builds分配不同的端口，来避免多个jobs同时启动时所遇到的冲突。

* 为不同的项目的开发人员建立email aliais，使得项目所有相关的人员都第一时间了解项目的状态。

* 增加额外的步骤来尽早地发现失败。例如log检查，微测试等。

* 对于经常的维护性的工作可以使用job来自动地完成，例如对磁盘的清除工作。

* 在build成功后对源代码打tag，label或baseline。

* 配置Jenkins bootstrapper来在build前更新工作目录。