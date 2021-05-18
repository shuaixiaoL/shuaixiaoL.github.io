---
layout: post
title: vue轻量级富文本编辑器
tags: vue  quillEditor
categories: work
---

* TOC 
{:toc}

在vue中使用富文本编辑器，此处对quillEditor进行归纳总结。

---

# 准备工作

### 下载相关组件

1.下载vue-quill-editor。

```
npm install vue-quill-editor
```

2.下载vue-quill-editor-upload。

```
npm install vue-quill-editor-upload
```

### 封装组件

创建`@/components/QuillEditor/index.vue`文件。

进行组件封装，供外部使用。

# 实现代码

```
<template>
  <div>
    <quill-editor
        class="editor_div"
        v-model="context"
        ref="quillEditor"
        :options="editorOption"
        @focus="onEditorFocus"
        @blur="onEditorBlur"
        @change="onEditorChange"
    />
  </div>
</template>
<script>
import { quillEditor } from "vue-quill-editor"; // 调用富文本编辑器
import "quill/dist/quill.snow.css"; // 富文本编辑器外部引用样式  三种样式三选一引入即可
//import 'quill/dist/quill.core.css'
//import 'quill/dist/quill.bubble.css'
import * as Quill from "quill"; // 富文本基于quill
import { quillRedefine } from "vue-quill-editor-upload";
import {getToken} from "@/utils/auth";
import request from '@/utils/request'
export default {
    name: "quillEditorName",
    components:{
        quillEditor
    },
    props: {
        //图片上传url地址
        uploadUrl: {
            type: String,
            default: "/api/file/upload"
        },
        //页面默认值
        value: {
            type: String,
            default: "",
        },
        //文件类型
        accept: {
            type: String,
            default: "",
        },
        //文件大小, 单位为kb, 默认为20MB
        fileLimit: {
            type: Number,
            default: 20480
        },
        /*上传图片的file控件name*/
        fileTypeName: {
            type: String,
            default: "file"
        },
    },
    data() {
        return {
            token: "",
            context: "",
            //配置项
            editorOption: {
                placeholder: "请在这里输入",
                readOnly: false,
                theme: "snow",
                modules:{
                    toolbar:[
                        ['bold', 'italic', 'underline', 'strike'],    //加粗，斜体，下划线，删除线
                        ['blockquote', 'code-block'],     //引用，代码块
                        [{ 'header': 1 }, { 'header': 2 }],        // 标题，键值对的形式；1、2表示字体大小
                        [{ 'list': 'ordered'}, { 'list': 'bullet' }],     //列表
                        [{ 'script': 'sub'}, { 'script': 'super' }],   // 上下标
                        [{ 'indent': '-1'}, { 'indent': '+1' }],     // 缩进
                        [{ 'direction': 'rtl' }],             // 文本方向
                        [{ 'size': ['small', false, 'large', 'huge'] }], // 字体大小
                        [{ 'header': [1, 2, 3, 4, 5, 6, false] }],     //几级标题
                        [{ 'color': [] }, { 'background': [] }],     // 字体颜色，字体背景颜色
                        [{ 'font': [] }],     //字体
                        [{ 'align': [] }],    //对齐方式
                        ['clean'],    //清除字体样式
                        ['image','video']    //上传图片、上传视频
                        ]
                }
            }
        }
    },
    watch: {
        value: {
            handler(val) {
                this.context = val
            },
            deep: true,
            immediate: true
        },
    },
    created() {
        this.token = getToken();
        this.uploadConfigMet()
    },
    methods: {
        //文件上传配置项
        uploadConfigMet() {
            this.editorOption = quillRedefine({
                // 图片上传的设置
                uploadConfig: {
                    action: this.uploadUrl, // 必填参数 图片上传地址,这里的后台是node
                    methods: 'POST',  // 可选参数 图片上传方式  默认为post
                    size: this.fileLimit,  // 可选参数   图片限制大小，单位为Kb, 1M = 1024Kb
                    accept: this.accept,  // 可选参数 可上传的图片格式
                    name: this.fileTypeName || "file",
                    header: (xhr, formData) => {
                        xhr.setRequestHeader('X-token',this.token);
                    },
                    start: () => {
                    },  // 可选参数 接收一个函数 开始上传数据时会触发,
                    end: () => {
                    },  // 可选参数 接收一个函数 上传数据完成（成功或者失败）时会触发
                    success: (res) => {
                        //this.$message.success("上传成功！")
                    },  // 可选参数 接收一个函数 上传数据成功时会触发
                    error: () => {
                        this.$message.error("上传失败！");
                    },  // 可选参数 接收一个函数 上传数据中断时会触发
                    res: (respnse) => {
                        if (respnse.code == 200) {
                            return respnse.data.fileDownloadUri;
                        }else{
                            this.$message.error(respnse.msg);
                        }
                        return ""
                    },
                },
            });
        },
        //获取焦点的时候
        onEditorFocus(e) {
            this.$emit("onEditorFocus", this.context)
        },
        //失去焦点
        onEditorBlur(e) {
            this.$emit("onEditorBlur", this.context)
        },
        //富文本编辑器  文本改变时 设置字段值
        onEditorChange(e) {
            // console.log(e);
            this.$emit("onEditorChange", this.context)
        },
    }
}
</script>
<style lang="scss" scoped>
</style>
```

# 外部使用

```
<template>
<quill-editor
	v-model="form.feedBackText"
	ref="quillEditor"
	:options="editorOption"
	@focus="onEditorFocus($event)"
	@blur="onEditorBlur($event)"
	@change="onEditorChange($event)"
  ></quill-editor>
</template>
<script>
import quillEditor from './components/QuillEditor'
......
 components: {
      quillEditor
    },
......
</script>
```

# 说明

具体可参考[Vue-Quill-Editor](https://www.npmjs.com/package/vue-quill-editor)。






