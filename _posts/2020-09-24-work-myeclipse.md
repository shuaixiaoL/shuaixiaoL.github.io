---
layout: post
title: 设置myeclipse工作区背景图片
tags: myeclipse setUp
categories: work
---

借鉴eclipse的设置方法，但是有细微区别，进行记录。

---

# 为什么要设置背景图片

单一的工作区会导致工作热情降低，所以可以间隔一段时间进行修改背景图片，嘿嘿。

---

# 如何设置
 
 **第一步**
 
 进入myeclipse安装路径，找到以下文件：  
 D:\myeclipse\plugins\org.eclipse.ui.themes_1.2.1000.v20200528-1125  
 说明：（_1.2.1000.v20200528-1125）版本可能有出入，大致格式如此。  
 css文件夹中存放这，myeclipse ui 的样式。  
 images 存放这myeclipse ui 的图片(格式为png,才能识别)。  
 
 **第二步**
 
 修改css文件夹中 e4_basestyle.css 文件。  
 如果无效可修改 e4-dark_win.css （我的主题为dark）。  
 我的myeclipse修改以上文件均无效，直接将css文件中的的dark文件下的css样式全部修改生效。  
 将以下代码copy，paste进上述文件。  
 
 ```
 .MPart StyledText {
 	background-image: url(../images/background.png); 
 	background-position: no-repeat; 
 	background-size:100% 100%;
 	-moz-background-size:100% 100%;
 }
 ```
 **第三步**
 
 图片可设置透明度，图片大小也可自行调整，css样式也可自行修改。  
 至此重启myeclipse效果生效。  


 