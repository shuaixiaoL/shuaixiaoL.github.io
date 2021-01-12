---
layout: post
title: App版本更新
tags: uni android
categories: work
---

* TOC 
{:toc}

APP版本更新，实现用户自动更新到最新版本。

# 原理

首先提供一个接口，查询最新的版本号以及下载路径。  
利用其返回数据，判断是否需要更新，并下载最新安装包。  
在APP中实现代码，每次启动APP都进行版本判断。  

# 实现代码

```
mounted() {
	let that = this;
	that.CheckUpdate();
},
methods: {
	//版本更新
	CheckUpdate: function() {
		var _this = this;
		//获取当前app版本信息
		var version = _this.$myApi.getApkVersion();
		uni.request({
			//获取服务器的版本信息
			url:  _this.$myApi.getApkInfoUrl(),
			data: {},
			success: (res) => {
				if(res.data.data.length > 0 ){
					var newVersion = res.data.data[0].version;
					var downUrl = res.data.data[0].downloadUrl
					if(newVersion > version && downUrl != ""){
						plus.nativeUI.confirm("检测到有新版本，是否更新", function(e) {
							if (e.index == 0) {
								if (plus.networkinfo.getCurrentType() != 3) {
									plus.nativeUI.confirm("检测到您目前非Wifi连接，是否继续更新", function(e) {
										if (e.index == 0) {
											_this.downWgt();
										} else {
										}
									}, "", )["取消", "确定"]
									return;
								}
								_this.downWgt(downUrl); //下载文件
							} else {
								//plus.runtime.quit();//安卓控制不更新退出应用
							
							}
						}, "", ["立即更新", "以后再说"]);
					}
				}
			},
			fail: () => {
				uni.showToast({
					title: '调用请求失败',
					mask: false,
					duration: 5000,
					icon: "none"
				});
			},
			complete: () => {}
		});
	},
	downWgt(downUrl) {
		/* uni.showLoading({
			title: '更新中……'
		}) */
		var showLoading = plus.nativeUI.showWaiting("正在更新，请耐心等待...");
		var downloadTask = uni.downloadFile({ //执行下载
			url: downUrl, //下载地址
			success: downloadResult => { //下载成功
				 plus.nativeUI.closeWaiting();
				if (downloadResult.statusCode == 200) {
					plus.runtime.install( //安装
						downloadResult.tempFilePath, {
							force: true
						},
					);
				}else if(downloadResult.statusCode == 400){
					uni.showToast({
						title: '更新失败',
						mask: false,
						duration: 2000,
						icon: "none"
					});
				}
			},
		});
		downloadTask.onProgressUpdate((res) => {
			showLoading.setTitle("  正在下载" + res.progress + "%  ");
		});
	},
},
```

# 问题说明

如果手机下载最新版本安装包之后未正常提示安装，可修改以下配置。

1.问题是因为没有添加APP安装应用的权限，解决方法在manifest.json文件里面APP模块权限配置的Android打包权限配置勾选以下权限  
```
<uses-permission android:name=\"android.permission.INSTALL_PACKAGES\"/>  
<uses-permission android:name=\"android.permission.REQUEST_INSTALL_PACKAGES\"/>
```

2.将build.gradle中的targetSdkVersion调到26或者更高  

3.在Androidmanifest.xml的application节点下添加provider节点，将里面的io.dcloud.HBuilder改成自己应用的包名。  
```
    <provider  
        android:name="io.dcloud.common.util.DCloud_FileProvider"  
        android:authorities="XXXX.XXX.XX(当前的应用包名).dc.fileprovider"  
        android:exported="false"  
        android:grantUriPermissions="true">  
        <meta-data  
            android:name="android.support.FILE_PROVIDER_PATHS"  
            android:resource="@xml/dcloud_file_provider" />  
    </provider>  
```

4.在Androidmanifest.xml中添加权限  
```
<uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES"/>  
```

