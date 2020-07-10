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

```bash
docker run --name my-nginx --restart=always -d \
    -p 10080:80 \
    -v ~/docker/my-nginx/html:/usr/share/nginx/html:ro \
    nginx
```
