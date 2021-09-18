---
layout: post
title: 大数据渲染
tags: js bigData
categories: work
---

* TOC 
{:toc}

当数据量过大，渲染页面可能会导致浏览器卡顿，对此进行优化。

---

# 实现原理

`window.requestAnimationFrame()` 告诉浏览器——你希望执行一个动画。   
并且要求浏览器在 下次重绘之前 调用指定的回调函数更新动画。  
该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。

# 实现代码

```
export function handleBigData(data, callback, pageSize = 100) {
  let totalCount = data.length; // 共多少条
  let currentPageNumber = 1; // 当前页数
  let totalPageNumer = Math.ceil(totalCount / pageSize); //可分多少页,就是分割为多少个小数组
  
  let handler = () => {
	let start = (currentPageNumber - 1) * pageSize;
	let end = currentPageNumber * pageSize;
	let currentData = data.slice(start, end); // 当前页的数据
	if (typeof callback === "function") {
	  callback(currentData, {
		totalCount,
		totalPageNumer,
		currentPageNumber,
		pageSize,
	  });
	}
	if (currentPageNumber < totalPageNumer) {
	  window.requestAnimationFrame(handler);
	}
	currentPageNumber++;
  };
  handler();
}
```

# 调用方式

```
self.handleBigData(opApplyProjectList, (data,map) => {
	for (var i = 0; i < data.length; i++) {
		var tempMap = data[i]
		//处理数据逻辑
		applicationList.push(tempMap);
	}
	if(map.currentPageNumber == map.totalPageNumer){
		//所有数据处理完再执行代码
	}
}); 
```





