---
layout: post
title: v-for 遍历 Map
tags: js vue 
categories: work
---

* TOC 
{:toc}

 v-for遍历List或Array，但v-for对于Map遍历较少，现对代码进行mark。

---

# 具体代码

```
<ul v-for="(value, key,index) in map" :key="index">
	<li>{{key}}</li>
	<ul v-for="val in value" :key="val.hexId">
		<li>{{val.name}}</li>
	</ul>
</ul>

```

key: map遍历的Key。  
value: map对应key的value值。  
index: map遍历的下标。





