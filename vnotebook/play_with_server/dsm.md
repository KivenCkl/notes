# dsm

## 安装

## 创建共享

### linux 挂载 nas 盘

#### 群晖开启 NFS 文件夹共享

在 `控制面板——文件服务` 中启用 `NFS` 服务

在 `控制面板——共享文件夹` 中对需要共享的文件夹设置 `NFS 权限`，点击 `新增`

![NFS 权限](https://gitee.com/KivenC/chaos/raw/master/upload_images/20200808181352.png)

其中服务器名称为 linux 的名称或 IP，当填 `*`，表示任意服务器均可访问。

其它可以保持不变，如果希望 linux 普通用户能访问，则需要设置 `Squash` 为 `映射所有用户为 admin`

至此，群晖这边已经设置好了。

#### linux 挂载

首先安装 NFS 客户端工具

```bash
# 对于 archlinux
sudo pacman -S nfs-utils
```

在 linux 中检测开启 NFS 服务的群晖主机 IP

```bash
showmount -e 192.168.2.200
```

能够看到刚刚设置的群晖共享文件夹

![nfs共享文件夹](https://gitee.com/KivenC/chaos/raw/master/upload_images/20200808183242.png)

然后在 linux 创建一个挂载目录并挂载

```bash
sudo mkdir /mnt/SyncFolder
sudo mount -t nfs 192.168.2.200:/volume1/SyncFolder /mnt/SyncFolder
```

挂载成功即可

#### 取消挂载

```bash
sudo umount -l /mnt/SyncFolder
```

## 其它应用

### Syncthing

在套件中心点设置，在套件来源新增 `http://packages.synocommunity.com`

在常规页面选择信任 Synology Inc. 和信任的发行者

在 Beta 源页面打上勾

添加源之后，搜索 Syncthing，点击安装

## 参考

[群晖NAS还能怎么玩？告诉你群晖NAS都有那些玩法](https://zhuanlan.zhihu.com/p/72743343)
