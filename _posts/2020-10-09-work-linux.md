---
layout: post
title: linux部署项目
tags: linux tomcat
categories: work
---

* TOC 
{:toc}

大多数情况都是在linux部署项目，对于利用tomcat部署进行总结。

---

# 准备工作

### 下载常用工具

1. xshell远程连接服务器。
2. xftp可以远程图形化界面直接操作服务器文件。

以上两个工具均可从官网下载，使用方便。

### 保证Linux的环境正常

服务器必须用jdk,版本最好和你项目使用的jdk版本一致。  
`java -version` 进行查看，如果没有可自行百度安装。

### tomcat下载

tomcat官网下载，上传至服务器，压缩文件需要解压。  
`tar -zxvf apache-tomcat-xxx.tar.gz` 解压即可。  
建议自己创建个文件夹保存tomcat.

---

# 开始部署

一、xshell连接服务器，上传打包好的war包到tomcat的webapp（建议使用xftp）。  

二、到tomcat的bin目录下执行`./startup.sh`即可启动tomcat。

三、war包会在webapp下自动解压，之后访问ip端口即可（其余配置和本地一致）。

---

# 常见问题

1. xshell连接服务器，密码输入不可见，直接输入回车即可。
2. tomcat启动项目报错，注意编码集以及lib下是否包含所需要的jar。
3. 启动成功项目访问不成功，可能是8080端口占用，防火墙的问题（百度相应方法）。
4. 如果使用阿里云等云服务器，注意需要自行在出入站规则以及白名单进行添加8080。
5. 如果更新war包，无法删除是权限问题，可以使用sudo。



