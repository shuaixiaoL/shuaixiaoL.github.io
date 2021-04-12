---
layout: post
title: vue生成txt文件下载
tags: vue txt 
categories: work
---

* TOC 
{:toc}

js拼接字符串之后，使用vue生成txt文件并下载。

---

# 方法

```
dowTXT(row){
	const element = document.createElement('a')
	element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(row.txt))
	element.setAttribute('download', row.name)
	element.style.display = 'none'
	element.click()
 }
```





