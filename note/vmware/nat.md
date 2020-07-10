[vmware]: /note/vmware/README.md
[img:setting01]: /pic/VMware_NAT_setting01.png
[img:setting02]: /pic/VMware_NAT_setting02.png
[img:setting03]: /pic/VMware_NAT_setting03.png

# NAT设置

[返回][vmware]

## 固定IP设置

- 修改宿主机器上NAT对应的网卡配置

    找到`VMware Network Adapter VMnet8`的网卡，修改其配置如下：
    ![avatar][img:setting01]

- 取消勾选本地DHCP服务
  
    打开VMware的 `编辑` > `虚拟网络编辑器`，选中`Vmnet8`，取消勾选`使用本地DHCP服务...`

    ![avatar][img:setting02]

- NAT设置

    点击`NAT设置`，设置网关IP

    ![avatar][img:setting03]

- 为虚拟机设置固定IP(以ubuntu 18.04为例)

    ```bash
    sudo vim /etc/netplan/50-cloud-init.ymal
    ```

    ```ymal
    network:
        ethernets:
            ens33:
                dhcp4: no
                addresses: [192.168.101.1/24]
                gateway4: 192.168.101.2
                nameservers:
                    addresses: [192.168.101.2]
        version: 2
    ```

    ```bash
    sudo netplan apply
    ```
