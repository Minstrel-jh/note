[docker]: /note/docker/README.md

# 网络

[返回][docker]

## 查看网络

1. 列出网卡

    ```bash
    docker network ls
    ```

2. 查看网卡设置

    ```bash
    docker network inspect <网络名>
    ```

## 设置网络

```bash
docker network create --driver bridge --subnet 172.98.0.0/16 my-bridge-network
```
