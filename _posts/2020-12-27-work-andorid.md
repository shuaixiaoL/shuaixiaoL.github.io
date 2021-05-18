---
layout: post
title: uni-app用AndroidStudio打包apk文件
tags: uni android
categories: work
---

* TOC 
{:toc}

uni-app用利用AndroidStudio打包apk文件，进行发布。

# 准备工作


安装[HBuilder X](https://www.dcloud.io/hbuilderx.html)。

安装[AndroidStudio](https://developer.android.google.cn/studio/)。

下载Dcloud[官方SDK](https://nativesupport.dcloud.net.cn/AppDocs/download/android)。

# 打包资源文件

HBuilderX生成本地打包文件，发行 -> 原生App-本地打包 -> 生成本地打包本地资源.

# 替换SDK里面的文件

`HBuilder-Hello\app\src\main\assets\apps`文件夹，删除HelloH5文件夹.  
将HBuilderX生成本地打包文件里面www的父文件夹拷贝到上面一步的HelloH5文件夹位置.  
拷贝manifest.json里的id值，赋值给dcloud_control.xml里的appid.  
修改build.gradle(Module:app)的applicationId,修改AndroidManifest.xml的package(固定格式为XXX.XXX.XXX，例如：com.uniapp.test).  
修改图标及启动页面(替换drawable里的三张图片即可).  
AndroidManifest.xml中的android:label修改APP名称.  

# 打包APK

Build -> Build Bundle(s)/APK(s) -> Build APK(s).

# 问题说明

### uni-app运行环境版本和编译器版本不一致的问题

首先确保全部升级，保持HBuilderX、自定义基座、cli项目编译器都是最新版。  
可以在manifest.json文件的源码视图中配置忽略这个提醒，方式如下：  
HBuilderX1.9.0及以上版本新增以下配置避免弹出提示框  
```
"app-plus": {  
"compatible": {  
    "ignoreVersion": true //true表示忽略版本检查提示框，HBuilderX1.9.0及以上版本支持  
},  
```
以下方法可针对指定版本避免弹出提示框(一般来说以上方法即可)。  
```
"app-plus": {  
"compatible": {  
    "runtimeVersion": "1.7.0", //根据实际情况填写  
    "compilerVersion": "1.7.1" //根据实际情况填写  
},  
```

### 启动app白屏,或者说基座错误

`HBuilder-Hello\app\libs`文件夹中:  
`uniapp-release.aar`和`lib.5plus.base-release.aar`这两个包是否缺少  
如缺少可从`SDK\libs`文件夹中找到并添加即可.






