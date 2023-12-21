---
layout: post
title: 解决 Windows 下 Git-Bash 运行 Java 代码打印中文乱码问题
---

### 问题

**乱码现象：**
```bash
$ javac EscapeCharacter.java && java EscapeCharacter
鍖椾含  涓婃捣  骞垮窞  娣卞湷  鏉窞
```

**打印中文的代码：**
```java
public class EscapeCharacter {
  public static void main(String[] args) {
    System.out.println("北京\t上海\t广州\t深圳\t杭州");
  }
}
```

### 原因

保存 Java 文件的编码和编译 Java 文件的编码不一致：这里我保存 Java 文件使用的是 utf-8 编码，而编译时使用了 GBK 编码

### 解决方法

使用 javac 编译 Java 代码时，指定用 utf-8 编码进行编译：
```bash
$ javac -encoding utf-8 EscapeCharacter.java && java EscapeCharacter
北京    上海    广州    深圳    杭州
```