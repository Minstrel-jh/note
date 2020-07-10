[zookeeper]: /note/zookeeper/README.md

# 部署 - Docker方式

[返回][zookeeper]

## 环境准备

虚拟机：1台  
系统：Ubuntu 16.04.5 LTS  
需要软件：docker

## 配置

需要事先在本地用户目录中新建配置文件，在之后运行容器时指定卷关联。  
关联路径可以自己来设置，此处是在当前用户目录下创建。

```text
docker
├── zk1
│   ├── cfg
│   │   └── zoo.cfg
│   └── data
│       └── myid
├── zk2
│   ├── cfg
│   │   └── zoo.cfg
│   └── data
│       └── myid
└── zk3
    ├── cfg
    │   └── zoo.cfg
    └── data
        └── myid
```

* 配置内容

```yml
clientPort=2181
dataDir=/data
dataLogDir=/datalog
tickTime=2000
initLimit=5
syncLimit=2

server.1=172.98.0.21:2888:3888
server.2=172.98.0.22:2888:3888
server.3=172.98.0.23:2888:3888
```

myid中输入对应配置的数字

## 运行容器

容器镜像不存在会自动下载

* 容器1

    ```bash
    docker run --name zk1 --restart always -d \
        -p 12181:2181 \
        --network=my-bridge-network --ip=172.99.0.21 \
        -v ~/docker/zk1/cfg/zoo.cfg:/conf/zoo.cfg  \
        -v ~/docker/zk1/data/myid:/data/myid \
        -v ~/docker/zk1/data:/data \
        -v ~/docker/zk1/datalog:/datalog \
        zookeeper:3.4.12
    ```

* 容器2

    ```bash
    $ docker run --name zk2 --restart always -d \
        -p 22181:2181 \
        --network=my-bridge-network --ip=172.99.0.22 \
        -v ~/docker/zk2/cfg/zoo.cfg:/conf/zoo.cfg  \
        -v ~/docker/zk2/data/myid:/data/myid \
        -v ~/docker/zk2/data:/data \
        -v ~/docker/zk2/datalog:/datalog \
        zookeeper:3.4.12
    ```

* 容器3

    ```bash
    $ docker run --name zk3 --restart always -d \
        -p 32181:2181 \
        --network=my-bridge-network --ip=172.99.0.23 \
        -v ~/docker/zk3/cfg/zoo.cfg:/conf/zoo.cfg  \
        -v ~/docker/zk3/data/myid:/data/myid \
        -v ~/docker/zk3/data:/data \
        -v ~/docker/zk3/datalog:/datalog \
        zookeeper:3.4.12
    ```

* 参数意义
  * -d：后台云运行
  * -p：端口映射
  * -v：卷关联：本机物理地址和容器内地址的关联

## 访问

```bash
docker exec -it zk1 bash
cd bin
zkCli.sh -server 172.99.0.23:2181
```
