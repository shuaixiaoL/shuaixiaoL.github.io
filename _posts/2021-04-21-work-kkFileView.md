---
layout: post
title: kkFileView文件文档在线预览
tags: kkFileView  
categories: work
---

* TOC 
{:toc}

kkFileView可作为文件文档在线预览解决方案，

---

# 说明

本次只对部署使用中的问题进行汇总整理。  

[查看官方详细文档](https://github.com/kekingcn/kkFileView)

# 注意事项

1.`application.properties`文件内`office.home`需填写自己的路径。

2.导入项目之后需自行maven下载更新依赖，最后打包启动。  

3.传入文件路径需加密，使用时可[参考base64](https://cdn.jsdelivr.net/npm/js-base64@3.6.0/base64.min.js)。 也可自行封装方法。





