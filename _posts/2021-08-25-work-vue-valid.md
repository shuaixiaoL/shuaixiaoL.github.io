---
layout: post
title: Vue针对表单验证，错误定位。
tags: vue vaild 
categories: work
---

* TOC 
{:toc}

当表单中数据项过多时，错误信息需要定位，第一时间知道修改位置。

---

# 原理

Vue中存在validate方法，可直接校验表单项。  
我们只需要对错误信息进行处理，滚动定位到错误项位置。

# 实现代码

```
this.$refs["dialogForm"].validate((valid,object) => {
        if (valid) {
           this.toSaveForm("1");
        }else{
          this.msgError("请确认表单填写正确");
          for (let i in object) {
              let dom = this.$refs[i];
              if (Object.prototype.toString.call(dom) !== '[object Object]') {　　//多个表单项，取值为数组
                dom = dom[0];
              }　
              dom.$el.scrollIntoView({
                //滚动到指定节点
                block: 'center', //值有start,center,end，nearest，当前显示在视图区域中间
                behavior: 'smooth' //值有auto、instant,smooth，缓动动画（当前是慢速的）
              })
              break;　
          }
        }
      });
```





