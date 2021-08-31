---
layout: post
title: JENKINS-48300 解决
tags: jenkins 
categories: work
---

* TOC 
{:toc}

最近发现偶尔出现JENKINS-48300问题，导致构建失败。

---

# 问题说明

具体错误信息：  

```
(JENKINS-48300: if on an extremely laggy filesystem, consider -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=86400)
```

# 解决方案

### 重启Jenkins

重启 Jenkins 以释放内存并防止交换（未验证）。

### 添加脚本

从 `Manage Jenkins` -> `Script Console` 添加：

```
System.setProperty("org.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL", "36000")
```

此方法使用之后暂无发现JENKINS-48300。


