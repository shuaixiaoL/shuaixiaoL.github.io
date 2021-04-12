---
layout: post
title: vue项目中使用mock
tags: vue mock 
categories: work
---

* TOC 
{:toc}

项目前后端分离开发，可使用mock对接口数据进行模拟，提高工作速率。

---

# mock基础

```
Mock.mock( rurl?, rtype?, template ) 
```

表示当拦截到rurl和rtype的ajax请求时，将根据数据模板template生成模拟数据，并作为响应数据返回。

```
Mock.mock( rurl, rtype, function( options ) )
```

记录用于生成响应数据的函数。当拦截到匹配 rurl 和 rtype 的 Ajax 请求时，函数 function(options) 将被执行，并把执行结果作为响应数据返回。

# 使用mock

首先安装mockjs

```
npm install mockjs
```

mock文件夹下创建index.js文件,并作为同意相关配置。  

```
// 首先引入Mock
const Mock = require('mockjs');

// 设置拦截ajax请求的相应时间
Mock.setup({
  timeout: '200-600'
});

let configArray = [];

// 使用webpack的require.context()遍历所有mock文件
const files = require.context('.', true, /\.js$/);
files.keys().forEach((key) => {
  if (key === './index.js') return;
  configArray = configArray.concat(files(key).default);
});

// 注册所有的mock服务,正则拦截带有参数请求
configArray.forEach((item) => {
  if(item != undefined){
    for (let [path, target] of Object.entries(item)) {
      let protocol = path.split('|');
      Mock.mock(new RegExp('.*'+process.env.VUE_APP_BASE_API + protocol[1] + '.*'), protocol[0], target);
    }
  }
});

```

mock文件夹下，创建任意文件 `*.js` 对接口进行模拟。

```
export default {
    'get|/element/d/list':  option => {
    return {
      code: 200,
      message: 'success',
      data: []
    };
  }
}
```

可在环境变量配置参数，动态加载mock(注意使用`VUE_APP_XXX`的格式定义)。

```
# 是否使用mock
VUE_APP_MOCK = 'true'
```

`main.js` 中动态引入。

```
process.env.VUE_APP_MOCK == 'true' && require('./mock/index');
```

[查看更多MOCK数据模板规范详解](https://www.freesion.com/article/7167209341/)



