---
layout: post
title: 前端对象判断是否一致
tags: compare 
categories: work
---

* TOC 
{:toc}

针对前端的对象,判断是否一致,此处进行代码记录。

---

# 具体代码

```
export function isObjectValueEqual(a, b) {
	var aProps = Object.getOwnPropertyNames(a);
	var bProps = Object.getOwnPropertyNames(b);
	if (aProps.length != bProps.length) {
		return false;
	}
	for (var i = 0; i < aProps.length; i++) {
		var propName = aProps[i]
		var propA = a[propName]
		var propB = b[propName]
		if ((typeof (propA) === 'object')) {
			if (this.isObjectValueEqual(propA, propB)) {
			  return true
			} else {
				return false
			}
		} else if (propA !== propB) {
			return false
		} else { }
	}
	return true
}
```





