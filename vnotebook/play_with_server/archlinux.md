# archlinux

## 基础安装

### 环境设置

这里选择的是 hyper-v 虚拟机，按需配置即可，需要注意的是：

1. 选择第一代虚拟机的话是 BIOS 启动模式，选择第二代虚拟机则是 UEFI 启动模式
2. 配置使用外部交换机

使用虚拟光驱加载镜像并启动。

默认为 root 账号启动。

### 磁盘分区

首先对磁盘分区，不过在此之前，需要判断是哪种启动方式，BIOS 和 UEFI 的分区有点不同。如果不清楚是什么启动方式，可以通过命令 `ls /sys/firmware/efi/efivars` 来判读，然后提示不存在这个文件，则说明启动模式为 BIOS，否则就是 UEFI。我选择的是第一代虚拟机，因为为 BIOS 启动模式。

#### BIOS 启动模式

利用 `fdisk -l` 查看当前虚拟机中的磁盘，找到添加的虚拟磁盘，我这边设备号为 `/dev/sda`

对该磁盘进行分区，`fdisk /dev/sda`，按照指令分区即可，如果想要运行桌面环境，需要休眠的，则需要设置交换分区，否则，直接将整块硬盘作为根目录即可。我是先分了 39G 作为根目录，然后剩下的 1G 作为交换分区。最后别忘记 `w` 命令执行操作后再退出。

|   分区    | 挂载点 |
| --------- | ------ |
| /dev/sda1 | /mnt   |
| /dev/sda2 | [swap] |

对分区进行格式化

```bash
# 对于根分区
mkfs.ext4 /dev/sda1

# 对于交换分区
mkswap /dev/sda2
swapon /dev/sda2
```

#### UEFI 启动模式

选择 GPT 分区表

|   分区    |   挂载点   |
| --------- | --------- |
| /dev/sda1 | /mnt/boot |
| /dev/sda2 | /mnt      |
| /dev/sda3 | [swap]    |

```bash
# 对于 efi 分区
mkfs.fat -F32 /dev/sda1

# 对于根分区
mkfs.ext4 /dev/sda2

# 对于交换分区
mkswap /dev/sda3
swapon /dev/sda3
```

### 挂载磁盘

格式化完成之后将硬盘进行挂载，这个时候系统是运行在 CD ROM 中，而电脑硬盘则相当于插在该系统的 U 盘。

对于 BIOS 启动模式而言

```bash
mount /dev/sda1 /mnt
```

对于 UEFI 启动模式而言，还需要挂载 efi 分区

```bash
mount /dev/sda2 /mnt
mkdir -p /mnt/boot
mount /dev/sda1 /mnt/boot
```

### 正式安装

首先选择合适的镜像源，打开 `/etc/pacman.d/mirrorlist`，找到 China 源，将其放置所有的源的最上方，这里提供部分 archlinux 源

```text
# 清华大学
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch

## 163
Server = http://mirrors.163.com/archlinux/$repo/os/$arch

## aliyun
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
```

正式安装系统，输入以下命令

```bash
pacstrap /mnt base linux linux-firmware base-devel vim
```

生成分区表

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

进入新系统

```bash
arch-chroot /mnt
```

设置时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

设置硬件时间，在虚拟机中可以跳过这一步

```bash
hwclock --systohc
```

本地化设置，打开 `/etc/locale.gen`，把 `en_US.UTF-8 UTF-8` 和 `zh_CN.UTF-8 UTF-8` 取消注释，然后保存退出，运行以下命令

```bash
locale-gen
echo LANG=en_US.UTF-8 >> /etc/locale.conf
```

设置 hostname

```bash
echo <myhostname> >> /etc/hostname
```

配置 hosts 文件，打开 `/etc/hosts`，输入以下内容，其中 `<myhostname>` 为上面自定义的名字

```bash
127.0.0.1    localhost
::1          localhost
127.0.1.1    <myhostname>.localdomain <myhostname>
```

设置 root 用户密码，命令：`passwd`

安装 `grub`，用于启动系统

```bash
# 对于 BIOS 启动模式
pacman -S grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

# 对于 UEFI 启动模式
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch_grub
grub-mkconfig -o /boot/grub/grub.cfg
```

根据 CPU 类型安装，可通过 `neofetch` 来查看 cpu 和显卡信息

```bash
pacman -S neofetch
neofetch

# intel CPU
pacman -S intel-ucode

# amd CPU
pacman -S amd-ucode
```

卸载分区，并重启机器

```bash
exit

# 对于 BIOS 启动方式
umount /mnt

# 对于 UEFI 启动方式
umount /mnt/boot
umount /mnt

reboot
```

## 简单配置

重新开机后，输入 root 账号和密码，进入 archlinux 系统。

### 创建一个普通用户账号

`<usrname>` 可以改成自己的用户名，`-m` 表示创建用户的家目录 `~`，`-G` 表示把用户放进 `wheel` 这个组。

```bash
useradd -m -G wheel <username>
```

设置用户密码

```bash
passwd <username>
```

为了让普通用户能够暂时获得管理权限，需要修改 `/etc/sudoers` 文件，找到 `%wheel ALL=(ALL) ALL`，将这一行取消注释，然后强制改写该文件，使得 `wheel` 这个组内的用户都可以使用 `sudo`。

使用 `su <username>` 换成普通用户登录系统，就可以正式开始使用 archlinux 系统了。

### 配置 IP

使用 `systemd-networkd` 服务，默认配置文件位于 `/etc/systemd/network`

开启 `systemd-networkd` 服务：`sudo systemctl enable systemd-networkd`

#### 配置 DHCP 动态 IP 地址

通过 `ip addr` 找到需要配置的网卡信息，例如 `eth0`

编辑 `/etc/systemd/network/eth0.network` 网卡配置文件

```text
[Match]
Name=eth0

[Network]
DHCP=ipv4
```

#### 配置 STATIC 静态 IP 地址

通过 `ip addr` 找到需要配置的网卡信息，例如 `eth0`

编辑 `/etc/systemd/network/eth0.network` 网卡配置文件

```text
[Match]
Name=eth0

[Network]
Address=192.168.2.20/24
Gateway=192.168.2.1
DNS=8.8.8.8
DNS=4.4.4.4
```

如果 `eth0.network` 中添加了 DNS 选项则需要同时启动 `systemd-resolved.service` 服务配合使用

```bash
sudo systemctl enable systemd-resolved.service
```

#### 重启网络服务，使配置生效

修改网卡配置文件之后，需要重启 `systemd-networkd` 服务使配置生效

```bash
sudo systemctl restart systemd-networkd
```

### 安装并启用 ssh

1. install ssh: `sudo pacman -S openssh`
2. set auto start: `sudo systemctl enable sshd`
3. start ssh service: `sudo systemctl start sshd`

从而可远程登录。

### 添加中文社区仓库

编辑 `/etc/pacman.conf` 配置文件，在文件最后添加以下内容

```text
[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch # 清华源

# 还可以选择其它服务器，但是只能添加一个
# Server = http://mirrors.163.com/archlinux-cn/$arch # 163 源
# Server = http://repo.archlinuxcn.org/$arch # 官方源
```

顺便可以将 `#Color` 这一行取消注释，pacman 将输出彩色信息。

安装 `archlinuxcn-keyring` 包以导入 `GPG key`，否则的话 key 验证失败会无法安装：

```bash
sudo pacman -S archlinuxcn-keyring
```

执行 `sudo pacman -Syyu` 进行更新。

### 安装 yay

yay 是 Go 编写的 archlinux AUR 包管理工具

```bash
sudo pacman -S yay
```

配置 yay 的 aur 源为清华源 AUR 镜像

```bash
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```

yay 常用命令

```bash
yay -S <app>  # 从 AUR 安装软件包
yay -Rns <app>  # 删除包
yay -Syu  # 升级所有已安装的包
yay -Ps  # 打印系统统计信息
yay -Qi <app>  # 检查安装的版本
```

### 安装并配置 git

安装 git

```bash
sudo pacman -S git
```

git 配置

```bash
git config --global user.name "kiven"
git config --global user.email "chen941229@126.com"
git config --global core.editor vim  # 使用 Vim 来编辑 Git 提交信息
ssh-keygen -t rsa -C "chen941229@126.com"
```

防止 git push 反复输密码，在 `~/.gitconfig` 文件中增加

```text
[user]
    name = 用户名
    email = 邮箱
[credential]
    helper = store
```

git 设置和取消代理

```bash
git config --global https.proxy http://127.0.0.1:1080

git config --global https.proxy https://127.0.0.1:1080

git config --global --unset http.proxy

git config --global --unset https.proxy
```

## 图形界面的安装

### 安装显卡驱动

Hyper-V 使用的是 `xf86-video-fbdev`

intel 集显用的是 `xf86-video-intel`

vmware 使用的是 `xf86-video-vmware`

### 安装桌面依赖

```bash
sudo pacman -S xorg-server xorg-apps xorg-xinit
```

### 安装登录管理器

可选 `sddm`, `lightdm` 等

```bash
sudo pacman -S sddm sddm-kcm
sudo systemctl enable sddm
```

### 安装桌面管理器

可选 `kde-plasma`, 'gnome`, 'deepin`, 'xfce` 等

```bash
sudo pacman -S plasma kde-applications
```

### 安装 yakuake 下拉式终端

```bash
sudo pacman -S yakuake
```

### 安装文件管理器

```bash
sudo pacman -S nautilus
sudo pacman -S ranger
```

### 安装 suckless 系列软件

安装 `dwm`

```bash
git clone https://git.suckless.org/dwm
cd dwm
make
sudo make install
```

安装 `st`, `dmenu` 同理，替换 `dwm` 即可。

在 `make` 的时候，可能会出现错误

```text
dwm.c:40:37: fatal error: X11/extensions/Xinerama.h: No such file or directory
compilation terminated.
make: *** [dwm.o] Error 1
```

运行 `sudo pacman -S libx11 libxinerama` 即可。

如果错误信息是 `drw.c:6:10: fatal error: X11/Xft/Xft.h: No such file or directory`，那么运行 `sudo pacman -S libXft` 即可。

### 配置 .xinitrc

`echo "exec dwm" >> ~/.xinitrc`

### 启动 X

`startx`

可能存在错误信息为

```text
/usr/lib/xorg/Xorg.wrap: Only console users are allowed to run the X server
```

解决方法

`echo "allowed_users = anybody" >> /etc/X11/Xwrapper.config`

依据来自 `man Xorg.wrap`

### 在 mobaxterm 中启用 X

1. 首先需要启用 `X11Forwarding`
    1. 打开 `/etc/ssh/sshd_config`
    2. 找到 `X11Forwarding` 这行，取消注释，并将 `no` 改为 `yes`
    3. 找到 `X11UseLocalhost` 这行，取消注释，并将 `yes` 改为 `no`
    > Note: 找到的资料是这么操作，但是发现第 3 条不执行也行。
2. 编辑 mobaxterm session，在 `Advanced SSH settings` 中启用 `X11-Forwarding` ，并将 `Remote environment` 改为对应的窗口管理器即可。（我这边是 `DWM desktop`）

## 声音管理器

```bash
sudo pacman -S alsa-utils pulseaudio pulseaudio-alsa
```

## 安装中文字体以及输入法

安装文泉微米黑，`sudo pacman -S wqy-microhei`

安装输入法

```bash
sudo pacman -S fcitx fcitx-im fcitx-configtool fcitx-googlepinyin fcitx-rime
```

配置输入法，编辑 `~/.xprofile`

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

## 开发环境

### docker

```bash
sudo pacman -S docker  //并尽可能安装可选依赖
sudo gpasswd -a $(whoami) docker   //不用sudo即可运行docker
sudo systemctl restart docker  //重启docker
sudo echo "{"registry-mirrors": ["http://hub-mirror.c.163.com"]}" >> /etc/docker/daemon.json  //写入网易镜像源
sudo systemctl enable docker   //开机自启docker
```

### nodejs

```bash
sudo pacman -S nodejs
sudo pacman -S npm
npm config set registry https://registry.npm.taobao.org  # 设置 npm 源
```

### go 配置

安装 go

```bash
sudo pacman -S go go-tools
```

配置环境变量 `GOPATH`，可以在用户目录下的 `go/` 文件夹放置 Go 文件，同时为了更好地拉取 Go 相关的软件包，可以配置环境变量 `GOPROXY`，编辑 `~/.xprofile` 文件：

```bash
export GOPATH=~/go
export PATH=$PATH:$GOPATH/bin

export GO111MODULE=on
export GOPROXY=https://goproxy.cn
```

## 其它

### makepkg

`PKGBUILD`: 完整地描述了一个包。是一个 Shell 脚本，描述了包的名字、版本、作者、如何编译、如何安装，以及冲突和依赖关系

`makepkg`: 是一个软件包自动编译脚本，直接由 pacman 包提供，该命令会调用 `PKGBUILD` 中封装的编译和安装脚本，这些脚本通常由 `configure`, `make`, `make install` 的流程构成。

1. 从 `aur` 获取 `PKGBUILD`，下载源码 `https://aur.archlinux.org/`
2. 编译得到 `PKGBUILD`
    ```bash
    makepkg -s xxx
    ```
    `-s` 参数表示不仅做编译，而且自动下载依赖，执行结束后可以得到一个 `xxx.pkg.tar.xz`
3. 使用 pacman 进行安装
    ```bash
    pacman -U xxx.pkg.tar.xz
    ```

### some packages

```txt
sl  # 在屏幕上绘制 ASCII 蒸汽机火车
cmatrix  # 黑客帝国，代码矩阵
cowfortune  # 名人名言
lolcat  # 彩色 cat
toilet  # 用于打印 ASCII 字符串，可以配置字体等
figlet  # 将文本转换为 ASCII 字符串
neofetch  # 获取系统信息
tree  # 打印文件目录
visual-studio-code-bin  # vscode
qt5-base qt5-doc qtcreator pkgconf  # qt 开发
netease-cloud-music  # 网易云音乐
intellij-idea-community-edition  # idea 社区版
baidunetdisk-bin  # 百度网盘
mpv  # 视频播放器
typora  # markdown 编辑器
vnote-git  # markdown 笔记编辑器
obs-studio  # 录屏录音软件
freedownloadmanager  # 下载软件
wireshark-qt  # 网络分析器 sudo gpasswd -a $(whoami) wireshark   # 不用sudo权限即可抓网卡
deepin.com.wechat2  # 微信
flameshot  # 截图
```

### kde 美化

```txt
终端：konsole, yakauke, alacritty
窗口管理器：kwin
登录管理器：sddm
文件管理器：nautilus(可视化)、ranger
gtk2/3主题：whiteSur
kde主题：whiteSur
图标主题：uos20
SHELL：zsh
图形显示框架：openGL 3
网络浏览器：google-chrome, chromium
沟通：微信(wine)、tim(wine)、linux-qq
有线音乐：网易云音乐
音乐/视频播放器：mpv
网盘：百度网盘
文章写作：typora(Markdown), vnote
录屏/录音/直播：obs studio(可视化)、ffmpeg
截图：scrot、spectatle
图片查看器：feh
下载器：FDM(可视化)、aria2、wget、curl、axel
远程控制：teamviewer
网络分析：wireshark
输入法框架：fcitx-pinyin
wifi管理：networkmanager
虚拟机：vmbox
引导程序：grub2
Aur助手：yay
文字处理：wps
系统信息查看：neofetch
```

## 参考

[Installation guide (简体中文)](https://wiki.archlinux.org/index.php/Installation_guide_(简体中文))

[从零开始配置自己的archlinux桌面（极简）](https://zhuanlan.zhihu.com/p/112536524)

[Error when trying to use Xorg: Only console users are allowed to run the X server?](https://unix.stackexchange.com/questions/478742/error-when-trying-to-use-xorg-only-console-users-are-allowed-to-run-the-x-serve)

[Archlinux 安装完后需要做的事情](https://juejin.im/post/6844904021520089102)

[Arch Linux 2020-07 安装kde桌面环境](https://www.jianshu.com/p/5e7726d1cb16)
