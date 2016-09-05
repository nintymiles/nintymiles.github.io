---
layout: post
title:  Learning Notes - XCodeBuild命令
---
## 使用 XCodeBuild test 命令执行iOS单元测试 target
可执行脚本样例如下：

```
xcodebuild test -workspace HL_App_Tzbao.xcworkspace -scheme HL_App_Tzbao -sdk iphonesimulator9.3 -destination OS=9.3,name="iPhone 6 Plus"  -configuration Debug  2>&1 | ocunit2junit 
```

## XCodeBuild命令
查看xcode的版本号和build版本

```
$ xcodebuild -version
```

显示当前系统的sdk、及其版本

```
$ xcodebuild -showsdks
```

显示工程项目信息,先cd到工程目录下（有＊.xcodeproj的目录）

```
$ xcodebuild -list
```

用ios9.3模拟器（iphonesimulator9.3）编译工程的sdk参数制定

```
$ xcodebuild ... -sdk iphonesimulator9.3 ...
```

## unix/linux - 2>&1 命令

在shell中,文件描述符通常是:STDIN,STDOUT,STDERR,即:0,1,2。

- 标准输入的控制

```
语法：命令< 文件将文件做为命令的输入。
例如：
mail -s “mail test” wesongzhou@hotmail.com < file1 将文件file1 当做信件的内容，主
题名称为mail test，送给收信人。
```

- 标准输出的控制

```
语法：命令> 文件将命令的执行结果送至指定的文件中。
例如:
ls -l > list 将执行“ls -l” 命令的结果写入文件list 中。

语法：命令>! 文件将命令的执行结果送至指定的文件中，若文件已经存在，则覆盖。
例如：
ls -lg >! list 将执行“ls - lg” 命令的结果覆盖写入文件list 中。

语法：命令>& 文件将命令执行时屏幕上所产生的任何信息写入指定的文件中。
例如：
cc file1.c >& error 将编译file1.c 文件时所产生的任何信息写入文件error 中。

语法：命令>> 文件将命令执行的结果附加到指定的文件中。
例如:
ls - lag >> list 将执行“ls - lag” 命令的结果附加到文件list 中。

语法：命令>>& 文件将命令执行时屏幕上所产生的任何信息附加到指定的文件中。
例如:
cc file2.c >>& error 将编译file2.c 文件时屏幕所产生的任何信息附加到文件error 中。
```

- 下面还几种不常见的用法：

```
n<&- 表示将 n 号输入关闭
<&- 表示关闭标准输入（键盘）
n>&- 表示将 n 号输出关闭
>&- 表示将标准输出关闭
```


