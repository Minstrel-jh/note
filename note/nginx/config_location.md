[nginx]: /note/nginx/README.md

# 配置 - location规则

> [Nginx文档](http://nginx.org/en/docs/)  
> [Nginx中文文档](https://www.nginx.cn/doc/)  
> [nginx访问控制与参数调优](https://segmentfault.com/a/1190000018505993)

[返回][nginx]  

## 语法

- 注释 用`#`注释
- 全局变量
- location  
- return 指令

  可以用来调试

  ```text
  return 200 "msg"
  ```

## 模块

### ngx_http_core_module

- location 指令
  
  

## 实例

- 用正则抽取url的部分来拼接成代理url

```conf
server {
    location ~ /in/ {
        if ($document_uri ~ "^/in/(.*)/v1/(.*)$") {
            set $transhost $1;
            set $endpoint $2;
        }

        proxy_pass http://$transhost/v1/$endpoint?$args;
    }
}
```
