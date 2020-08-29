[nginx]: /note/nginx/README.md
[docker>install_on_ubuntu]: /note/docker/install_on_ubuntu.md

# 部署 - Docker方式

[返回][nginx]  

## 下载镜像

```bash
docker pull nginx
```

如出现以下问题，可以尝试修改Docker的仓库地址为国内的地址，详见[在ubuntu上安装docker][docker>install_on_ubuntu]

```text
Error response from daemon: Get https://registry-1.docker.io/v2/library/nginx/manifests/latest: Get https://auth.docker.io/token?scope=repository%3Alibrary%2Fnginx%3Apull&service=registry.docker.io: net/http: TLS handshake timeout
```

## 创建容器

- 默认配置创建容器

    ```bash
    docker run --name my-nginx --restart=always -d \
        -p 10080:80 \
        -v ~/docker/my-nginx/html:/usr/share/nginx/html:ro \
        nginx
    ```

- 自定义配置创建容器

    ```bash
    docker run --name my-nginx --restart=always \
    -p 20080:20080 \
    -v ~/docker/my-nginx/conf.d:/etc/nginx/conf.d:ro \
    -v ~/docker/my-nginx/data/con/trunk/static:/data/con/trunk \
    -d nginx:1.17
    ```

    需要提前在conf.d下添加配置文件*.conf：

    ```text
    server {
        listen       20080;
        server_name  _;

        root /data/con/trunk;

        location / {
            proxy_pass http://127.0.0.1:8080;
        }
    }
    ```

- 查看日志 /var/log/nginx
