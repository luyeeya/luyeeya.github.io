---
layout: post
title: 终端开启代理
---

> 注意：我这里的操作针对的是 `zsh` 这个 shell（命令解释器）：

### 配置代理
```bash
# 1.打开 ~/.zshrc 文件
vim ~/.zshrc

# 2.追加开关代理的代码并保存退出
alias proxy='export all_proxy=socks5://127.0.0.1:7890'
alias unproxy='unset all_proxy'

# 3.激活配置
source ~/.zshrc
```

### 开启代理
使用 `proxy` 开启代理，执行 `curl ipinfo.io` 命令，可以看到切换到了国外 `ip`：
```bash
{
  "ip": "xxx.xxx.xxx.xxx",
  "hostname": "xxx-xxx-xxx-xxx.ip.linodeusercontent.com",
  "city": "Los Angeles",
  "region": "California",
  "country": "US",
  "loc": "xxx,xxx",
  "org": "AS63949 Akamai Connected Cloud",
  "postal": "90009",
  "timezone": "America/Los_Angeles",
  "readme": "https://ipinfo.io/missingauth"
}
```

### 关闭代理
使用 `unproxy` 开启代理，执行 `curl ipinfo.io` 命令，可以看到切回到了国内 `ip`：
```bash
{
  "ip": "xxx.xxx.xxx.xxx",
  "city": "Nanjing",
  "region": "Jiangsu",
  "country": "CN",
  "loc": "xxx,xxx",
  "org": "AS4134 CHINANET-BACKBONE",
  "timezone": "Asia/Shanghai",
  "readme": "https://ipinfo.io/missingauth"
}
```
