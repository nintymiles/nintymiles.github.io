---
layout: post
title: 使用IntelliJ IDE时碰到target相关的编译错误问题的解决
---

## 初次使用`IntelliJ IDE`设置Java项目时，如果没有制定`project bytecode version`，会报出编译错误。
错误表现为：`Error:java: javacTask: source release 8 requires target release 1.8`，解决方式有两种：
* 通过 preferences 设置，步骤如下：

```
1、File > Settings > Build, Execution, Deployment > Java Compiler
2、Change Target bytecode version to 1.8 of the module that you are working for.

```

* 如果使用maven，可以通过在`project`节点下加入`compiler plugin`来指定，但是maven对于`IntelliJ`的`build`菜单编译输出无效。

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```



