---
layout: post
title: 修改node_modules中的内容
tags: node_modules patch-package 
categories: work
---

* TOC 
{:toc}

修改`node_modules`源码，bug进行修复或者个性化开发。  
通过`patch-package`修改。

---

# 实现步骤

### 安装依赖

```
npm i patch-package --save-dev
```

### 增加脚本

在`package.json`文件中的`scripts`属性中。  

```
"postinstall": "patch-package"
```

### 修改内容

修改`node_modules`中的具体代码。  


### 创建补丁

```
npx patch-package xxx
```







