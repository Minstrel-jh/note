[linux]: /note/linux/README.md

# 软件

[返回][linux]

## VIM

```text
x                               删除光标所在处字符
w                               光标以单词向后移动
b                               光标以单词向前移动
u                               撤销
ctrl+r                          恢复撤销
ctrl+b                          向前一屏
ctrl+f                          向后一屏
/字符串                         查询
n                               下一个
shift+n                         上一个

:set nu                         显示行号
:m,nd                           删除m到n行
```

## Mysql

```text
*登录
mysql -rroot -h127.0.0.1 -p
*运行sql脚本
(f1)> source <sqlscript>
(f2)mysql -rroot -h127.0.0.1 -p <database> < <sqlscript>
```

## VMBOX

```text
VBoxManage list vms                 列出vms
VBoxManage list runningvms          列出正在运行的vms
VBoxManage startvm <vsname> --type headless
VBoxManage controlvm <vsname> poweroff
```

## SCP

```text
scp -P <port> 文件名 user@ip:/home/foodbi/
```

## docker

```text
docker images
docker ps
    -a                              列出所有容器
docker start <container>
docker stop <container>
docker rm -v <container>
    -v                              删除关联卷
docker network ls
docker network inspect <网络名>
docker exec -it <container> bash
```
