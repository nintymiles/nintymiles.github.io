---
layout: post
title:  Learning Notes - About OCLint and AndroidLint
---

## OCLint
OCLint is a ***static code analysis tool*** for improving quality and reducing defects by inspecting C, C++ and Objective-C code and looking for potential problems like:

Possible bugs - empty if/else/try/catch/finally statements
Unused code - unused local variables and parameters
Complicated code - high cyclomatic complexity, NPath complexity and high NCSS
Redundant code - redundant if statement and useless parentheses
Code smells - long method and long parameter list
Bad practices - inverted logic and parameter reassignment
...
Static code analysis is a critical technique to detect defects that aren't visible to compilers. OCLint automates this inspection process with advanced features:

Relying on the abstract syntax tree of the source code for better accuracy and efficiency; False positives are mostly reduced to avoid useful results sinking in them.
Dynamically loading rules into system, even in the runtime.
Flexible and extensible configurations ensure users in customizing the behaviors of the tool.
Command line invocation facilitates continuous integration and continuous inspection of the code while being developed, so that technical debts can be fixed early on to reduce the maintenance cost. 

## Android Lint 
Android工程的静态代码扫描工具有FindBugs和**android lint**，两者的区别是前者能够发现一些java代码层面的问题，lint能够扫描出android工程的特定问题，比如无用的资源文件、android api版本兼容问题、布局文件等等问题。本文主要简单介绍后者的使用。

Android lint的使用方式有：从android studio中执行、命令行方式（可以用于持续集成）。

Android Lint工具用于android工程静态代码扫描，可以在以下几个层面分析代码：correctness，security，performance，usability，accessibility和internationalization。我们可以在持续集成时进行Lint静态扫描，改进代码质量。

![AndroidLint_in_AS](/asset/technical/AndroidLint_in_AS.png)

