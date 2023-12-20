---
layout: post
title: 在 Markdown 中画流程图
mermaid: true
---

在 Markdown 中画流程图需要用到代码块（语言类型为mermaid）：

```markdown
graph TB
java[.java 文件]
-->clazz[.class 文件]
clazz-->linux[在 Linux 运行]
clazz-->windows[在 Windows 运行]
clazz-->macos[在 MacOS 运行]
```

<div class="mermaid">
graph TB
java[.java 文件]
-->clazz[.class 文件]
clazz-->linux[在 Linux 运行]
clazz-->windows[在 Windows 运行]
clazz-->macos[在 MacOS 运行]
</div>

### 方向

1. TB，从上到下

2. TD，从上到下

3. BT，从下到上

4. RL，从右到左

5. LR，从左到右

### 常用符号

1. `A(起止框)`

2. `B[处理框]`

3. `C{判断框}`

### 其他

- 换行：`<br>`  
- 括号：`A["OOP(Object-oriented programming)"]`
