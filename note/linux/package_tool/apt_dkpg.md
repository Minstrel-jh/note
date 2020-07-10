[linux]: /note/linux/README.md
[url:linux-package-management]: https://www.sysgeek.cn/linux-package-management/
[url:apt-vs-apt-get]: https://www.sysgeek.cn/apt-vs-apt-get/
[url:package_management]: https://hacpai.com/article/1569310098526

# Ubuntu的包管理工具: apt/apt-*/dkpg

[返回][linux]

> [Linux软件包管理基本操作入门][url:linux-package-management]  
> [Linux中apt与apt-get命令的区别与解释][url:apt-vs-apt-get]  
> [Ubuntu/Centos 下显示已安装包信息][url:package_management]  

## 刷新包索引

```bash
apt update
apt-get update
```

### 列出已安装的软件

```bash
apt list --installed
dpkg --get-selections
dpkg -l
```
