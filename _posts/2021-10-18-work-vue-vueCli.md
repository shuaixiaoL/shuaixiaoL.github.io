---
layout: post
title: vue多页面应用
tags: vue 
categories: work
---

* TOC 
{:toc}

vue项目工程默认是属于单页面单入口,实际开发时会进行多页面多入口的开发需求。

---

# 多页面配置

vue.config.js文件中进行多页面配置，如下：

```
pages:{
    other:{
      entry: 'src/other/other.js',
      template: 'public/other.html',
      filename: 'other.html',
      title: 'Other Page',
      chunks: ['chunk-vendors', 'chunk-common', 'other']
   },
    index: {
      entry: 'src/main.js',
      template: 'public/index.html',
      filename: 'index.html',
      title: 'Index Page',
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    }
  },

```

此为示例，按实际需求定义页面入口。  
chunks说明：  
在这个页面中包含的块，默认情况下会包含  
提取出来的通用 chunk 和 vendor chunk。  
chunks: ['chunk-vendors', 'chunk-common', 'index']  
如有自定义块需自行引入。  

# 部署说明

vue多页面项目部署到服务器上时,页面空白的问题可能时由于nginx代理造成。  
对此在nginx中加上如下配置即可：  

```
location ~ ^/(xxxxx) {
              root /opt/deployment/front/xxx;
              try_files $uri $uri/ /xxxxx.html;
              index  xxxxx.html;
          }

```







