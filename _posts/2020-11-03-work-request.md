---
layout: post
title: request响应拦截器
tags: request js
categories: work
---

* TOC 
{:toc}

记录request响应拦截器代码。

# 整体代码

创建request文件夹，并建立以下子文件

### config.js

```
import MyRequest from './request'

const myRequest = new MyRequest()

// 请求拦截器
myRequest.interceptors.request((request) => {
	if (uni.getStorageSync('token')) {
		request.header['Authorization'] = uni.getStorageSync('token');
	}
	return request
})

// 响应拦截器
myRequest.interceptors.response((response) => {
//对返回接口进行处理       
})

// 设置默认配置
myRequest.setConfig((config) => {
    config.baseURL = 'http://xxxx:8888';
	config.imgURL = 'http://xxx';
	config.version = 'xxx';
    if (uni.getStorageSync('token')) {
		config.header['Authorization'] = uni.getStorageSync('token');
    }
    return config;
})

export default myRequest
```

### apis.js

```
import request from './config.js'
export default{
  apis:{
		getLogin(data){//登录
			return request.get('/xx/login', data);
		}
	}
}
```

### request.js

```
const config = Symbol('config')
const isCompleteURL = Symbol('isCompleteURL')
const requestBefore = Symbol('requestBefore')
const requestAfter = Symbol('requestAfter')

class MyRequest {
    //默认配置
    [config] = {
        baseURL: '',
        header: {
            'content-type': 'application/json',
        },
        method: 'GET',
        dataType: 'json',
        responseType: 'text',
		formData:{}
    }
    //拦截器
    interceptors = {
        request: (func) => {
            if (func) 
            {
                MyRequest[requestBefore] = func
            } 
            else 
            {
                MyRequest[requestBefore] = (request) => request
            }
        },
        response: (func) => {
            if (func) 
            {
                MyRequest[requestAfter] = func
            } 
            else 
            {
                MyRequest[requestAfter] = (response) => response
            }
        }
    }

    static [requestBefore] (config) {
        return config
    }

    static [requestAfter] (response) {
        return response
    }

    static [isCompleteURL] (url) {
        return /(http|https):\/\/([\w.]+\/?)\S*/.test(url)
    }
    
    request (options = {}) {
        options.baseURL = options.baseURL || this[config].baseURL
        options.dataType = options.dataType || this[config].dataType
        options.url = MyRequest[isCompleteURL](options.url) ? options.url : (options.baseURL + options.url)
        options.data = options.data
        options.header = {...options.header, ...this[config].header}
        options.method = options.method || this[config].method
        options = {...options, ...MyRequest[requestBefore](options)}

        return new Promise((resolve, reject) => {
            options.success = function (res) {
                resolve(MyRequest[requestAfter](res))
            }
            options.fail= function (err) {
                reject(MyRequest[requestAfter](err))
            }
            uni.request(options)
        })
    }

    get (url, data, options = {}) {
        options.url = url
        options.data = data
        options.method = 'GET'
        return this.request(options)
    }

    post (url, data, options = {}) {
        options.url = url
        options.data = data
        options.method = 'POST'
        return this.request(options)
    }
	
	setConfig (func) {
	   this[config] = func(this[config]);
	}
	getConfig() {
	    return this[config]; 
	}
}

MyRequest.install = function (Vue) {
    Vue.mixin({
        beforeCreate: function () 
        {
            if (this.$options.apis) 
            {
                Vue._myRequest = this.$options.apis
            }
        }
    })
    
    Object.defineProperty(Vue.prototype, '$myApi', {
        get: function () 
        {
            return Vue._myRequest.apis
        }
    })
}

export default MyRequest
```

# 代码调用

```
this.$myApi.getLogin(param).then((res) => {
	console.log(res);
});
```
