---
layout: post
title: 父页面Vue中使用Iframe实现数据通信
tags: vue iframe 
categories: work
---

* TOC 
{:toc}

父页面Vue中嵌入iframe之后，实现父子页面间的通信。

---

# 背景

实际业务中，需要获取父子页面中的属性或者方法调用，所以必须实现父子页面间的通信。  

父子页面通过EventListener监听信号，以postMessage方式进行互相通信。  

# 父组件

```
export default {
  data () {
    return {
      src: '你的src',
      iframeWin: {}
    }
  },
  mounted () {
    this.iframeWin = this.$refs.iframe.contentWindow
    this.$nextTick(() => {
      // 在外部 Vue 的 window 上添加 postMessage 的监听，并且绑定处理函数 handleMessage
      window.addEventListener('message', this.handleMessage)
    })
  },
  destroyed () {
      // 注意移除监听！注意移除监听！注意移除监听！
      window.removeEventListener('message', this.handleMessage)
  }    
  methods: {
    sendMessage () {
      // 外部vue向iframe内部传数据
      this.iframeWin.postMessage({
        cmd: 'doSomething',
        params: {}
      }, '*')
    },
    handleMessage (event) {
      // 根据上面制定的结构来解析 iframe 内部发回来的数据
      const data = event.data
      switch (data.cmd) {
        case 'ready-for-receiving':
          // 业务逻辑
          break
      }
    }
  }
}
```

# 子组件

```
export default {
  data () {
    return {}
  },
  mounted () {
    this.$nextTick(() => {
      window.addEventListener('message', this.handleMessage)
 
      // 告诉父组件准备好接收消息了
      window.parent.postMessage({
        cmd: 'ready-for-receiving'
      }, '*')
    })
  },
  destroyed () {
      // 注意移除监听！注意移除监听！注意移除监听！
      window.removeEventListener('message', this.handleMessage)
  }
  methods: {
    handleMessage (event) {
      // 根据上面制定的结构来解析 iframe 内部发回来的数据
      const data = event.data
      switch (data.cmd) {
        case 'doSomething':
          // 业务逻辑
          break
      }
    }
  }
}
```

# 注意事项

如果子组件为vue单文件是，可使用`this.refs.xxx.method()`直接调用方法。

如果父级页面定义了名为`vm`的vue,子页面可以使用`parent.vm.method(params)`。  

`window.parent`取不到，可尝试`window.parent.parent`。必要时可使用`window.top`。






