---
layout: post
title: 理解环境变量
---

环境变量是配置在操作系统中的全局变量，存储了供一个或多个程序使用的信息

### 常见的环境变量

#### PATH
`PATH` 是操作系统使用的环境变量，告诉操作系统去哪些路径寻找可执行程序
> 对于 Java 来说，需要把 `{jdk_path}/bin` 路径加入 PATH 变量，使得可以在任意路径下使用 `javac`、`java`、`javap` 等程序

#### JAVA_HOME
`JAVA_HOME` 指向 JDK 安装路径
> 它是一个约定，很多程序会使用到这个环境变量

#### CLASSPATH
`CLASSPATH` 告诉 Java 去哪些路径找运行程序需要的包和类