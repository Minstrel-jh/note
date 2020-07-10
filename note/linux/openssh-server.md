[linux]: /note/linux/README.md
[url:enable_ssh_on_ubuntu]: https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-18-04/

# Ubuntu 18.04 开启SSH

[返回][linux]

> [How to Enable SSH on Ubuntu 18.04][url:enable_ssh_on_ubuntu]

## 安装openssh-server

```bash
sudo apt update
sudo apt install openssh-server
```

## 查看SSH服务的状态

```bash
sudo systemctl status ssh
```

## 设置秘钥登录

- 生成秘钥

    ```bash
    ssh-keygen -t rsa
    ```

    会在当前用户目录的.ssh文件夹下生成公钥和私钥

- 将公钥内容追加到authorized_keys文件末尾

    ```bash
    cd .ssh
    touch authorized_keys
    cat id_rsa.pub >> authorized_keys
    ```

- 修改权限

    ```bash
    cd ..
    chmod 700 .ssh
    chmod 600 .ssh/authorized_keys
    ```

- 修改下ssh的配置

    ```bash
    sudo vim /etc/ssh/sshd_config
    ```

    修改或增加内容如下:

    ```text
    RSAAuthentication yes
    PubkeyAuthentication yes
    # The default is to check both .ssh/authorized_keys and .ssh/authorized_keys2
    # but this is overridden so installations will only check .ssh/authorized_keys
    AuthorizedKeysFile      .ssh/authorized_keys
    ```

## 使用秘钥访问

- 修改私钥文件权限

    ```bash
    chmod 700 .ssh/id_rsa
    ```

- 省去连接时公钥确认步骤

    在首次连接服务器时，会弹出公钥确认的提示。这会导致某些自动化任务，由于初次连接服务器而导致自动化任务中断。或者由于  ~/.ssh/known_hosts 文件内容清空，导致自动化任务中断。
    SSH 客户端的 StrictHostKeyChecking 配置指令，可以实现当第一次连接服务器时，自动接受新的公钥。只需要修改 /etc/ssh/ssh_config 文件，包含下列语句：

    ```bash
    sudo vim /etc/ssh/ssh_config
    ```

    ```text
    StrictHostKeyChecking no
    ```
