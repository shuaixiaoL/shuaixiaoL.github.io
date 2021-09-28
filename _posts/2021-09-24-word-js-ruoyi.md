---
layout: post
title: RuoYi(若依)平台页面缓存无效
tags: ruoyi 
categories: work
---

* TOC 
{:toc}

RuoYi平台，菜单设置可设置页面是否缓存，存面缓存无效的情况。

---

# 原因

路由地址与我们的页面中的name不一致导致。

# 解决方法

### 修改页面name

需要遵守vue规范，vue中页面的name的命名为首字母大写即可。  

### 修改路由接口数据

修改框架本身的后台接口，路由的地址进行处理，不对首字母进行处理。  
保证路由地址与我们的页面中的name一致即可。







