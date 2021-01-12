---
layout: post
title: 基于axios的请求拦截器
tags: request axios js
categories: work
---

* TOC 
{:toc}

在axios的基础上，封装的请求拦截器。

---

# 引入文件

`document.write("<script src='xx.js'></script>");`  
这里需要注意相对路径。  

为针对需要先引入js再使用方法的情况，可调用方法引入。  
```
function loadJS(url, callback) {
    var script = document.createElement('script'),
        fn = callback || function () {
        };
    script.type = 'text/javascript';
    //IE
    if (script.readyState) {
        script.onreadystatechange = function () {
            if (script.readyState == 'loaded' || script.readyState == 'complete') {
                script.onreadystatechange = null;
                fn();
            }
        };
    } else {
        //其他浏览器
        script.onload = function () {

            fn();
        };
    }
    script.src = url;
    document.getElementsByTagName('head')[0].appendChild(script);
}
```

# 拦截器代码

```
const baseUrl = "xxxx";

// http request 请求拦截器
axios.interceptors.request.use(config => {
	let pathname = location.pathname;
    if (localStorage.getItem('token')) {
        if (pathname != '/' && pathname != '/login') {
            config.headers.common['Authorization'] = localStorage.getItem('token');
        }
    }
    return config;
}, error => {
    if (error.response) {
        return false;
    }
});

// http response 响应拦截器
axios.interceptors.response.use(response => {
	if (response.data.code == 200 || response.data.code == '200') {
	    return response.data;
	}else {
	    return false;
	}
}, error => {
    if (error.response) {
        //localStorage.removeItem('token');
        //返回处理错误信息
        return false;
        //return Promise.reject(error.response);
    }
});

/**
 * 封装get方法
 * @param url
 * @param data
 * @returns {Promise}
 */
function get(url, params = {}) {
    return new Promise((resolve, reject) => {
        axios.get(baseUrl + url, {
            params: params
        }).then(response => {
            resolve(response);
        }).catch(err => {
            reject(err)
        })
    })
}

/**
 * 封装post请求
 * @param url
 * @param data
 * @returns {Promise}
 */
function post(url, params = {}) {
    return new Promise((resolve, reject) => {
        axios.post(baseUrl + url, params).then(response => {
            resolve(response);
        }, err => {
            reject(err)
        })
    })
}

/**
 * 封装delete请求
 * @param url
 * @param data
 * @returns {Promise}
 */
function del(url, params = {}) {
    return new Promise((resolve, reject) => {
        axios.delete(baseUrl + url, {data: params}).then(response => {
            resolve(response);
        }, err => {
            reject(err)
        })
    })
}

```

