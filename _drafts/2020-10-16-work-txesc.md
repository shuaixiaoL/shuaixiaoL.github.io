---
layout: post
title: 腾讯云服务器部署项目
tags: linux tomcat
categories: work
---

* TOC 
{:toc}

针对新买的腾讯云部署项目遇到的问题进行总结。

---

#准备工作

下载xshell，xftp工具进行远程连接。

登陆腾讯云控制台添加安全组入站出站8080端口。

下载需要的tomcat(tar.gz)。jdk选择需要的版本（可用yum直接安装）。

#配置JAVA环境

##使用jdk压缩包

xshell连接服务器，创建文件夹存放JDK(例如/java/),进入该文件目录。  





