[nginx]: /note/nginx/README.md

# 配置 - location规则

[返回][nginx]  

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
