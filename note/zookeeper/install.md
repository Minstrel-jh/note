[zookeeper]: /note/zookeeper/README.md

# 部署

[返回][zookeeper]

## 环境准备

虚拟机：3台  
网络：NAT  
ip：192.168.2.101/192.168.2.102/192.168.2.103  
系统：Ubuntu 16.04.5 LTS  
需要软件：jdk  

## 安装zookeeper

```bash
sudo apt-get install zookeeper
```

默认信息

```text
# 安装路径
/usr/share/zookeeper
# 配置文件
/etc/zookeeper/conf/zoo.cfg
```

## 配置

```yml
tickTime=2000
initLimit=5
syncLimit=2
dataDir=/zk/data
dataLogDir=/zk/datalog
clientPort=2181

server.1=192.168.2.101:2888:3888
server.2=192.168.2.102:2888:3888
server.3=192.168.2.103:2888:3888
```

* tickTime：Zookeeper服务器心跳时间，单位毫秒  
* dataDir：数据持久化路径，必须事先建好  
* clientPort：客户端连接zookeeper服务的端口号  
* syncLimit：Leader与Follower之间的最大响应时间单位，响应超过syncLimit*tickTime，Leader认为Follwer死掉，从服务器列表中删除Follwer  
* initLimit：投票选举新leader的初始化时间  
* server.n=ip:2888:3888  
  * n是一个标识，与myid文件对应  
  * 2888端口号是zookeeper服务之间通信的端口，表示的是这个服务器与集群中的 Leader 服务器交换信息的端口；  
  * 3888端口号表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口。

## 创建myid文件

```bash
sudo mkdir -p /zk/data
sudo mkdir -p /zk/datalog
```

可以把/var/lib/zookeeper/myid这个软链接移动到/zk/data中，也可以自己在/zk/data中创建一个myid文件，或者创建一个软链接myid，链接到/etc/zookeeper/conf/myid。

```bash
sudo ln -s /etc/zookeeper/conf/myid /zk/data/myid
```

myid中输入这台主机对应的server.n的n。如server.1=192.168.2.101:2888:3888，myid中就要输入1

## 启动

```bash
cd /usr/share/zookeeper/bin
sudo ./zkServer.sh start
```

## 访问

```bash
cd /usr/share/zookeeper/bin
sudo ./zkCli.sh -server host:port
```

## 创建节点

```text
[zk: 192.168.2.101:2181(CONNECTED) 0] create /node1 1
Created /node1
[zk: 192.168.2.101:2181(CONNECTED) 1] ls /
[zookeeper, node1]
```
