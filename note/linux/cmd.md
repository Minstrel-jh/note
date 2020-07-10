[linux]: /note/linux/README.md

# 常用命令

[返回][linux]

## 命令行快捷键

```text
ctrl + ←/→                      单词间跳转
ctrl + a                        跳到命令开头
ctrl + e                        跳到命令末尾
ctrl + k                        删除光标后的命令
ctrl + u                        删除光标前的命令
```

## 帮助

```text
man <name>
    -f                          相当于whatis，查看关于name的手册的章节，并简要概括
    不同章节：
                                1是普通的命令
                                2是系统调用,如open,write之类的(通过这个，至少可以很方便的查到调用这个函数，需要加什么头文件)
                                3是库函数,如printf,fread
                                4是特殊文件,也就是/dev下的各种设备文件
                                5是指文件的格式,比如passwd, 就会说明这个文件中各个字段的含义
                                6是给游戏留的,由各个游戏自己定义
                                7是附件还有一些变量,比如向environ这种全局变量在这里就有说明
                                8是系统管理用的命令,这些命令只能由root使用,如ifconfig
    -a                          继续显示所有关于name的手册章节
    n                           查看第n章节
```

## 文件操作

```text
ls <dirname>
    -a                          显示全部文件(包括隐藏文件.开头/./..)
    -l                          列出长数据串
    --time=atime                输出访问时间，而非内容更改时间
    --time=ctime                输出权限改变时间，而非内容更改时间
cp <srcfile> <destfile>         复制
    -a                          -pdr
    -p                          连同文件属性一同复制，而非使用默认属性(备份常用)
    -d                          复制连接文件而非文件本身
    -r                          递归
    -i                          会询问覆盖
rm <file>
    -f                          强制删除
    -i                          会询问删除
    -r                          递归
mv <srcfile> <destfile>
    -f                          强制，目标存在会直接覆盖
    -i                          询问覆盖
```

## 文件权限

```text
chmod u+x file                  给file的属主增加执行权限
chmod 751 file                  给file的属主分配读、写、执行(7)的权限，给file的所在组分配读、执行(5)的权限，给其他用户分配执行(1)的权限
chmod u=rwx,g=rx,o=x file       上例的另一种形式 （u=rwx,g=rx,o=x中间不能有空格）
chmod =r file                   为所有用户分配读权限
chmod 444 file                  同上例
chmod a-wx,a+r                  同上例
chmod -R u+r                    目录名称 递归地给directory目录下所有文件和子目录的属主分配读的权限
```

## 文件查看

```text
cat <file>                      由第一行开始显示内容，并将所有内容输出
    -n                          显示行号
tac <file>                      从最后一行倒序显示内容，并将所有内容输出
more <file>                     一页一页查看，只能向后看
less <file>                     ★一页一页查看
    b                           向前一屏
    f                           向后一屏
    u                           前半页
    d                           后半页
    /字符串                     向下查询
    ?字符串                     向上查询
    n                           重复前一个查询
    N                           反向重复前一个查询
head -n <file>                  读取文件前n行
tail -n <file>                  读取文件后n行
    -f                          即时输出文件变化后追加的数据
tailf -n <file>                 动态跟踪日志文件
```

## 文件查找

```text
which <command>                 查找命令的位置
whereis                         只能用于查找程序名
locate                          根据var/lib/mlocate内的数据库文件查找，通常一天更新一次
updatedb                        更新数据库文件
find <path> -name "<pattern>"   在path路径下查找文件名满足的pattern的文件
    -iname                      基于文件名，忽略大小写
    -path
    -regex                      基于正则
    !                           逻辑非
    -type f                     文件类型f
    -maxdepth n                 查找深度n
```

## 文件系统

```text
df -lh                          列出本地多有文件系统
du --max-depth=1 -h .           列出当前目录文件的磁盘占用
```

## 全局正则表达式输出

```text
grep <pattern> <file|path>...   grep(global regular expression print，全局正则表达式输出)
    -l                          只列出文件名
    -n                          列出行号
    -r                          对目录递归查找
    -i                          忽略大小写
    -e                          查找多个模式
    -c                          计算匹配的数量
ps -ef | grep <搜索信息> | grep -v grep | awk '{print $2}'      找到进程号
    grep -v grep                去除grep本身
    awk '{print $2}'            对每一行执行{print $2}这个脚本
```

## 系统进程

```text
ps -ef
    -e                          显示所有进程
    -f                          全格式
kill -9 <PID>                   杀死进程
    -9                          强制
```

## 系统服务

```text
systemctl start <servicename>   打开服务
systemctl status <servicename>  查看服务
systemctl enable <servicename>  激活开机启动，把/usr/lib/systemd/system/链接到/etc/systemd/system/
```

## 网络

```text
ifgonfig
netstat                         显示各种网络相关信息，如网络连接，路由表，接口状态等等
    -t                          ★仅显示tcp相关选项
    -u                          仅显示udp相关选项
    -n                          ★拒绝显示别名，能显示数字的全部转化成数字
    -l                          ★仅列出有在LISTEN的服务状态，默认不显示LISTEN相关
    -p                          ★显示建立相关链接的程序名
    -e                          显示扩展信息，例如uid等
    -r                          显示路由信息，路由表
    -s                          按各个协议进行统计
    -c                          每隔一个固定时间，执行该netstat命令。
netstat -ntlp
```

## 计划任务

```text
crontab -l                      查看用户的计划任务
crontab -e                      编辑计划任务文件
格式：
Minute  Hour    Day     Month   Day     ofweek  command
分钟    小时    天      月      天      每星期  命令
特殊符号意义：
"*"代表取值范围内的数字
"/"代表"每"
"-"代表从某个数字到某个数字
","分开几个离散的数字
```

## 权限

```text
sudo !!                         sudo执行上一条命令
sudo -s                         ubuntu下提升权限
```

## 用户&用户组

```text
adduser --system --group --no-create-home <name>
groupadd <name>
groups [<name>]                 看用户所在的组,以及组内成员

cat /etc/group                  查看用户组,格式：group_name:passwd:GID:user_list
```

## dpkg

```text
dpkg-reconfigure                重新配置已经安装的软件包
dpkg -l                         列出已安装的软件包
```

## apt

```text
apt-cache search
apt-cache show

常用软件包：
sudo apt-get install build-essential    提供编译程序必须软件包的列表信息
```

## 压缩

```text
tar
====主选项(一条命令以下5个参数只能有一个):
    -c                          --create 新建一个压缩文件
    -x                          --extract,--get 解压文件
    -t                          --list 查看
    -r                          --append 追加
    -u                          --update 更新??
====辅助选项:
    -z                          .tar.gz 具有gzip属性
    -v                          显示过程
    -f                          使用文件名，注意，在f之后要立即接文档名，不要再加其他参数！

tar -tf 文件名                  查看内容
    -v                          查看详细信息
tar -zxvf 文件名                解压.tar.gz文件
```

## 运行

```text
nohup <command>[ &]             不挂断地运行命令
    &                           在后台运行
```

## 重定向

```text
最常使用的 FD (file descriptor) 大概有三个, 分别是:
0                               是一个文件描述符，表示标准输入(stdin)
1                               是一个文件描述符，表示标准输出(stdout)
2                               是一个文件描述符，表示标准错误(stderr)
====
[1]>filename                    将标准输出重定向到文件
2>filename                      将标准错误重定向到文件
2>&1                            将标准错误重定向到标准输出
```

## 技巧

```text
ls -l /bin/sh                   查看当前sh用的是什么
pwd                             输出当前路径
ctrl + d                        exit
free -m                         看内存
```
