---
layout: post
title: 使用 javadoc 生成 API 文档
---

**用于生成 API 文档的 demo 代码：**
```java
/**
 * javadoc demo 类
 * @author luyee
 * @version 1.0
 */
public class JavadocDemo {
  /**
   * 入口方法
   * @param args 执行程序时传递的参数
   */
  public static void main(String[] args) {
    System.out.println("A javadoc demo~");
  }
}
```

**执行 javadoc 命令生成 API 文档：**
```bash
# -author 和 -version 是支持的 javadoc 标签
javadoc -d {target_path} -author -version JavadocDemo.java
```
用浏览器打开生成的 `index.html` 即可浏览 API 文档