---
layout: post
title: linux中防火墙操作
tags: linux firewall
categories: work
---

* TOC 
{:toc}

linux服务器中对防火墙进行操作

---

# 操作汇总

### 开启端口

`firewall-cmd --zone=public --add-port=8080/tcp --permanent`

### 关闭端口

`firewall-cmd --zone=public --remove-port=5672/tcp --permanent`  

### 重新加载

`firewall-cmd --reload`  

### 查看端口列表

`firewall-cmd --zone=public --list-ports`




