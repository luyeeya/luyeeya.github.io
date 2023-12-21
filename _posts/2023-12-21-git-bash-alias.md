---
layout: post
title: 设置 Git-Bash 常用的 alias
---

### 如何设置 alias
```bash
# 1.添加 alias 到 bash 配置文件
echo "alias javac='javac -encoding utf-8'" >> ~/.bashrc
# 2.刷新配置文件
source ~/.bashrc
```

### 常用的 alias
```bash
alias ll='ls -l'
alias javac='javac -encoding utf-8'
```
