---
layout: post
title: element-ui中的绑定事件传递多个参数
tags: vue element 
categories: work
---

* TOC 
{:toc}

 element-ui中的存在很多绑定事件，实际开发中需要传递多个其余参数如name,id等。

---

# 具体方法

其中最为主要的两种方法如下：
举例：`@change="changeEvent"`

### 第一种

```
@change = changeEvent( event,args);
```

### 第二种

```
@change="((val)=>{changeEvent(val,args)})";
```





