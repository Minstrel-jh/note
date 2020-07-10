[docker]: /note/docker/README.md
[url:docker_install]: https://docs.docker.com/install/linux/docker-ce/ubuntu/

# 在ubuntu上安装docker

[返回][docker]

> [在ubuntu上安装docker][url:docker_install]

## 使用apt仓库安装

### 设置apt仓库

1. 更新`apt`的包索引，并且安装`apt`使用HTTPS来使用仓库的依赖

    ```bash
    sudo apt-get update

    sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common
    ```

2. 添加Docker官方的GPG key

    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

3. 设置仓库

    ```bash
    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
    ```

### 安装Docker

1. 安装最新版本的Docker

    ```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    sudo docker run hello-world  // 测试安装成功
    ```

2. 用非root用户管理docker

    ```bash
    sudo groupadd docker
    sudo usermod -aG docker $USER
    ```

### 修改镜像

```bash
sudo vim /etc/docker/daemon.json
```

文件内容如下

```text
{
    "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

重启docker服务

```bash
sudo systemctl restart docker
```
