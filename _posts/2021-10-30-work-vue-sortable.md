---
layout: post
title: Vue使用sortable.js实现拖拽
tags: vue sortable
categories: work
---

* TOC 
{:toc}

Vue使用sortable.js实现拖拽，对数据重新排序拖放。

---

# 引用

npm install sortablejs --save  

import Sortable from 'sortablejs'  

# 使用

可定义rowDrop方法,对拖拽得各个钩子函数进行自己的数据处理。
以下为自己针对树形表格拖拽的方法：  
```
//行拖拽
rowDrop() {
	this.$nextTick(() => {
		this.sortTable = null;
		//获取可拖拽的节点
		const tbody = document.querySelector('.right-box-botton .el-table .el-table__body-wrapper tbody')
		if (!tbody) {
			return;
		}
		const _this = this
		this.sortTable = Sortable.create(tbody, {
			onEnd({
				newIndex,
				oldIndex
			}) {
				var defaultList = JSON.parse(JSON.stringify(_this.data))
				//将树形数据进行平铺
				var liletData = _this.treeToTile(_this.data, "children");
				var indexNum = 1;
				if (oldIndex > newIndex) {
					indexNum = 0;
				}
				var oldRow = liletData[oldIndex]
				var newRow = liletData[newIndex]
				if (oldRow.parentId == newRow.parentId) { //不能跨级拖拽
					var oldparentId = oldRow.parentId;
					var oldparent = defaultList.find(d => d.id === oldparentId);
					if (oldparent) {
						var children = oldparent.children;
						var index = children.findIndex(d => d.id === oldRow.id);
						children.splice(index, 1);
					} else {
						var index = defaultList.findIndex(d => d.id === oldRow.id);
						defaultList.splice(index, 1);
					}
					var newParentId = newRow.parentId;
					var newParent = defaultList.find(d => d.id === newParentId);
					if (newParent) {
						var children = newParent.children;
						var index = children.findIndex(d => d.id === newRow.id);
						children.splice(index + indexNum, 0, oldRow)
					} else {
						defaultList.push(oldRow);
					}
				}
				//对数据进行排序后的赋值
				_this.$set(_this, "data", defaultList)
				//组件定义key,对组件加载进行刷新处理
				_this.$set(_this, "tableKey", Math.ceil(Math.random() * 1000000));
				_this.rowDrop()
			}
		})
	});
}，
//将树形数据进行平铺
treeToTile(treeData, childKey = 'children') {
	const arr = []
	const expanded = data => {
		if (data) {
			data.filter(d => d).forEach(e => {
				arr.push(e)
				expanded(e[childKey] || [])
			})
		}
	}
	expanded(treeData)
	return arr
}
```

# 扩展说明

其余配置项可参考[sortable中文配置](http://www.sortablejs.com/options.html)。  





