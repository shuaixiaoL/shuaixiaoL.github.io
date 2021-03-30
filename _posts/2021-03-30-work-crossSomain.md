---
layout: post
title: 接口跨域解决方法
tags: nginx java 
categories: work
---

* TOC 
{:toc}

针对调用接口跨域问题，对解决方法进行总结

---

# nginx解决

直接使用nginx配置代理接口即可解决。  

```
server{
      listen80;
      server_namewww.test.com;

      location/ {
              proxy_passhttp://localhost:8081/;
              #proxy_set_headerHost $host:80;
              #proxy_set_headerX-Real-IP $remote_addr;
              #proxy_set_headerX-Forwarded-For $proxy_add_x_forwarded_for;

              add_headerAccess-Control-Allow-Origin $http_origin;
              add_headerAccess-Control-Allow-Methods *;
              add_headerAccess-Control-Allow-Headers $http_access_control_request_headers;
              add_headerAccess-Control-Max-Age 60000;
              add_headerAccess-Control-Allow-Credentials true;

              if($request_method = OPTIONS){
                     return200;
              }
    }
}
```

# 后台解决

添加java代码，直接解决跨域问题。  

```
@Configuration
class CorsConfig extends WebMvcConfigurerAdapter {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "HEAD", "POST", "PUT", "PATCH", "DELETE", "OPTIONS", "TRACE");
    }
}
```




