# pve

## 直通硬盘

1. 打开 pve 终端，输入：`ls /dev/disk/by-id/`，记住需要直通的硬盘编号
2. 输入：`qm set 103 -sata1 /dev/disk/by-id/ata-ST1000LM024_HN-M101MBB_S2SMJ9DD724040`

> Note: 103 为虚拟机编号，ata-ST1000LM024_HN-M101MBB_S2SMJ9DD724040 为硬盘编号

## 一些问题

### 解决订阅问题

利用已经写好的脚本，不仅可以解决订阅问题，还可以将apt源换成中科大、修改pve-no-subscription源，源地址：[百度网盘](https://pan.baidu.com/s/10lOS9i9UdQs6alBAyRL42w)，提取码：rek3

### Proxmox VE /Debian /Ubuntu 设置合上笔记本盖子不休眠的方法

服务器是没有AB面的，但是笔记本有，不能让屏幕一直打开亮着，但是默认都是关闭盖子休眠的。因为是Debian系的，可以编辑 `/etc/systemd/logind.conf`

```conf
#HandlePowerKey按下电源键后的行为，默认power off
#HandleSleepKey 按下挂起键后的行为，默认suspend
#HandleHibernateKey按下休眠键后的行为，默认hibernate
#HandleLidSwitch合上笔记本盖后的行为，默认suspend（改为ignore；即合盖不休眠）在原文件中，还要去掉前面的#
```

然后再重启服务，`service systemd-logind restart`