---
layout: post
title: codemirror插件的使用
tags: codemirror 
categories: work
---

* TOC 
{:toc}

codemirror插件的使用进行整理

---

# html代码


```
<textarea :v-show="false" ref="mycode" v-model="form.getConfig" placeholder="请填写sql" class="codesql" style="height:200px;width:600px;" />
```

# js代码

```
//引用
import 'codemirror/theme/ambiance.css'
import 'codemirror/lib/codemirror.css'
import 'codemirror/addon/hint/show-hint.css'
const CodeMirror = require('codemirror/lib/codemirror')
require('codemirror/addon/edit/matchbrackets')
require('codemirror/addon/selection/active-line')
require('codemirror/mode/sql/sql')
require('codemirror/addon/hint/show-hint')
require('codemirror/addon/hint/sql-hint')
import sqlFormatter from 'sql-formatter';

//js

//初始化CdMirror
initCdMirror(value) {
	value = value ? value : "";
	const mime = 'text/x-mariadb'
	if(this.editor == undefined){
		this.editor = CodeMirror.fromTextArea(this.$refs.mycode, {
		  mode: mime, // 选择对应代码编辑器的语言，我这边选的是数据库，根据个人情况自行设置即可
		  indentWithTabs: true,
		  smartIndent: true,
		  lineNumbers: true,
		  matchBrackets: true,
		})
	}
	this.editor.setValue(value);
}, 
//格式化sql
sqlFormat() {
	let sqlContent = ''
	sqlContent = this.editor.getValue()
	this.editor.setValue(sqlFormatter.format(sqlContent))
}

```

# API

```
this.CodeMirrorEditor.setValue(“Hello Kitty”)：设置编辑器内容

this.CodeMirrorEditor.getValue()：获取编辑器内容

this.CodeMirrorEditor.getLine(n):获取第n行的内容

this.CodeMirrorEditor.lineCount()：获取当前行数

this.CodeMirrorEditor.lastLine()：获取最后一行的行号

this.CodeMirrorEditor.isClean():boolean类型判断编译器是否是clean的

this.CodeMirrorEditor.getSelection()：获取选中内容

this.CodeMirrorEditor.getSelections():返回array类型选中内容

this.CodeMirrorEditor.replaceSelection(“替换后的内容”):替换选中的内容

this.CodeMirrorEditor.getCursor():获取光标位置,返回{line,char}

this.CodeMirrorEditor.setOption("",""):设置编译器属性

this.CodeMirrorEditor.getOption(""):获取编译器属性

this.CodeMirrorEditor.addKeyMap("",""):添加key-map键值，该键值具有比原来键值更高的优先级

this.CodeMirrorEditor.removeKeyMap(""):移除key-map

this.CodeMirrorEditor.addOverlay(""):Enable a highlighting overlay…没试出效果

this.CodeMirrorEditor.removeOverlay(""):移除Overlay

this.CodeMirrorEditor.setSize(width,height):设置编译器大小

this.CodeMirrorEditor.scrollTo(x,y):设置scroll到position位置

this.CodeMirrorEditor.refresh():刷新编辑器

this.CodeMirrorEditor.execCommand(“命令”):执行命

```



