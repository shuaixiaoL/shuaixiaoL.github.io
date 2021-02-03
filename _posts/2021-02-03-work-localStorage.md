---
layout: post
title: 利用localStorage获取码表数据
tags: localStorage js
categories: work
---

* TOC 
{:toc}

后台数据关联码表left关联太慢,前台利用localStorage获取码表数据。

# 思路

利用localStorage对码表数据进行缓存，由于码表数据过多，可针对使用到的类型进行分类缓存。  
码表数据存在多级子节点，可通过迭代进行数据处理。

# 具体实现代码

1.传入类型和编码，获取编码对应的内容。
```
function getCodeValue(type,code){
	var result = "";
	var currentCodeList = JSON.parse(localStorage.getItem(type+"temp"));
	if(currentCodeList){
		result = getValueById(currentCodeList,code);
	}else{
		axios.get(contextPath + "/api/code/xxxx", {
		    params: {code: type}
		}).then(response => {
		   if(response){
			   result = getValueById(response.data,code);
			   localStorage.setItem(type+"temp",JSON.stringify(response.data))
		   }
		}).catch(err => {
		    reject(err)
		})
	}
	return result;
}
```  

2.针对存在多级的情况，利用迭代进行数据获取。  
```
var getValueById = function(list,code){
	var result = "";
	for(var i = 0 ; i < list.length ; i ++ ){
		var temp = list[i];
		if(temp.code == code){
			return temp.name;
		}else if(temp.children){
			if(temp.children.length > 0 ){
				return getValueById(temp.children,code);
			}
		}
	}
	return result;
}
```

# 总结相关方法

1.查看localStorage大小。
```
var localStorageSpace = function(){
        var allStrings = '';
        for(var key in window.localStorage){
            if(window.localStorage.hasOwnProperty(key)){
                allStrings += window.localStorage[key];
            }
        }
        return allStrings ? 3 + ((allStrings.length*16)/(8*1024)) + ' KB' : 'Empty (0 KB)';
    };

```

2删除对应的缓存数据(刷新数据)。
```
Object.keys(localStorage).forEach(item => item.indexOf('temp') !== -1 ? localStorage.removeItem(item) : '');


```
