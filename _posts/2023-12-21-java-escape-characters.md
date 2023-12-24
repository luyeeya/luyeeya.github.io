---
layout: post
title: Java 常用转义字符
---

### 常用转义字符
1. 制表符：`\t`
2. 回车：`\r`
   > 回车这个词来源于打字机，在计算机中表示将输入光标回到当前行的行首
   
   举例说明：
   ```java
   System.out.println("123456789\rabc");
   ```
   输出结果：
   ```bash
   abc456789
   ```
3. 换行：`\n`
4. 单斜杠：`\\`
   举例说明：
   ```java
   System.out.println("\\t表示制表符");
   ```
   输出结果：
   ```bash
   \t表示制表符
   ```
5. 双引号：`\"`
   举例说明：
   ```java
   System.out.println("这是一个双引号：\"");
   ```
   输出结果：
   ```bash
   这是一个双引号："
   ```
