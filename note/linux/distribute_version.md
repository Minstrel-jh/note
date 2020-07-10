[linux]: /note/linux/README.md

# 查看版本信息

[返回][linux]

## 查看内核版本

- 显示正在运行的内核版本

    ```bash
    cat /proc/version
    ```

    ```text
    Linux version 4.15.0-96-generic (buildd@lgw01-amd64-004) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #97-Ubuntu SMP Wed Apr 1 03:25:46 UTC 2020
    ```

- 显示电脑以及操作系统的相关信息

    ```bash
    uname -a
    ```

    ```text
    Linux u-218 4.15.0-96-generic #97-Ubuntu SMP Wed Apr 1 03:25:46 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
    ```

## 查看系统版本

- 列出所有版本信息

    ```bash
    lsb_release -a
    ```

    ```text
    No LSB modules are available.
    Distributor ID: Ubuntu
    Description:    Ubuntu 18.04.4 LTS
    Release:        18.04
    Codename:       bionic
    ```

- 适用于Redhat系的Linux

    ```bash
    cat /etc/redhat-release
    ```

- 适用于所有Linux发行版

    ```bash
    cat /etc/issue
    ```

## 查看cpu信息

```bash
cat /proc/cpuinfo
```
