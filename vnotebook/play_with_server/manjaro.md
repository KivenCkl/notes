# manjaro

## 下载安装

1. 从官网获取系统镜像，这里是从中科大源下载的 `Manjaro 20.0.1 KDE` 版本
2. 写入至 U 盘，这里使用的软件是 `Etcher`
3. U 盘启动进行安装即可，注意的是在载入 liveCD 时，如果有 Nvidia 独显时，需要将选项 `driver` 改为 `no free`。
4. 安装时可以断开网络连接，防止安装过程中从国外源下载文件，速度缓慢。

## 基础配置

### 更换源

配置中国的 mirrors，在终端执行下面的命令对中国源进行测速和设置：

```bash
sudo pacman-mirrors -g  # 排列源，可不执行
sudo pacman-mirrors -c China -m rank  # 更改源，在跳出的对话框里选择想要的源
```

为 Manjaro 增加中文社区的源来加速安装软件，在 `/etc/pacman.conf` 中添加 `archlinuxcn` 源，末尾加上：

```config
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

安装 `archlinuxcn-keyring` 包以导入 `GPG key`，否则的话 key 验证失败会无法安装：

```bash
sudo pacman -S archlinuxcn-keyring
```

同步并更新系统：


```bash
sudo pacman -Syyu
```

### 安装输入法

`fcitx` 是 Free Chinese Input Toy for X 的缩写。

```bash
sudo pacman -S fcitx-im  # 选择全部安装
sudo pacman -S fcitx-configtool  # 安装图形化配置工具
sudo pacman -S fcitx-googlepinyin  # 谷歌拼音
sudo pacman -S fcitx-skin-material  # 皮肤
```

解决中文输入法无法切换问题：添加文件 `~/.xprofile`

```config
export GTK_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

输入法需要重启生效。

### 解决和 Windows 双系统时间同步问题

参照 [Arch Wiki](https://wiki.archlinux.org/index.php/System_time_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 修改 Windows 系统的注册表。

以管理员身份运行 PowerShell，输入

```cmd
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD
```

如果是 32 位操作系统，则需将 `REG_QWORD` 改为 `REG_DWORD`

同时需要禁用 Windows 的自动同步时间功能。

### pacman 包管理工具

```bash
sudo pacman -S <app>  # 安装软件 app
sudo pacman -R <app>  # 删除单个软件包，保留其全部已经安装的依赖关系
sudo pacman -Rs <app>  # 删除指定软件包及其所有没有被其他已安装软家包使用的依赖关系
sudo pacman -Ss <app>  # 查找软件
sudo pacman -Sc  # 清空并且下载新数据
sudo pacman -Syu  # 升级所有软件包
sudo pacman -Qs  # 搜索已安装的软件包

sudo pacman -R $(pacman -Qdtq)  # 删除系统无用的包
sudo pacman -Scc  # 清除已下载的安装包
```

### yay 包管理工具

yay 是用 Go 编写的 Arch Linux AUR 包管理工具。

安装 yay:

```bash
sudo pacman -S yay
```

配置 yay 的 aur 源为清华源 AUR 镜像：

```bash
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```

修改的配置文件位于 `~/.config/yay/config.json`，还可以通过以下命令查看修改过的配置：

```bash
yay -P -g
```

yay 常用命令：

```bash
yay -S <app>  # 从 AUR 安装软件包
yay -Rns <app>  # 删除包
yay -Syu  # 升级所有已安装的包
yay -Ps  # 打印系统统计信息
yay -Qi <app>  # 检查安装的版本
```

yay 安装命令不需要加 sudo。

### git 配置

```bash
git config --global user.name "kiven"
git config --global user.email "chen941229@126.com"
git config --global core.editor vim  # 使用 Vim 来编辑 Git 提交信息
ssh-keygen -t rsa -C "chen941229@126.com"
```

防止 git push 反复输密码，在 `~/.gitconfig` 文件中增加：

```text
[user]
	name = 用户名
	email = 邮箱
[credential]
	helper = store
```

### 把个人目录对应的文件夹修改为英文

打开配置文件 `~/.config/user-dirs.dirs`

修改以下内容：

```text
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Download"
XDG_TEMPLATES_DIR="$HOME/Templates"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Documents"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Pictures"
XDG_VIDEOS_DIR="$HOME/Videos"
```

将 Home 目录下的中文目录修改为对应的英文目录：

```bash
cd ~   #进入个人目录
mv 公共 Public
mv 模板 Templates
mv 视频 Videos
mv 图片 Pictures
mv 文档 Documents
mv 下载 Download
mv 音乐 Music
mv 桌面 Desktop
```

### 修改 hosts 文件

编辑 hosts 文件：`sudo vi /etc/hosts`

Github 相关：

```text
# GitHub Start
52.74.223.119 github.com
192.30.253.119 gist.github.com
54.169.195.247 api.github.com
185.199.111.153 assets-cdn.github.com
151.101.76.133 raw.githubusercontent.com
151.101.108.133 user-images.githubusercontent.com
151.101.76.133 gist.githubusercontent.com
151.101.76.133 cloud.githubusercontent.com
151.101.76.133 camo.githubusercontent.com
151.101.76.133 avatars0.githubusercontent.com
151.101.76.133 avatars1.githubusercontent.com
151.101.76.133 avatars2.githubusercontent.com
151.101.76.133 avatars3.githubusercontent.com
151.101.76.133 avatars4.githubusercontent.com
151.101.76.133 avatars5.githubusercontent.com
151.101.76.133 avatars6.githubusercontent.com
151.101.76.133 avatars7.githubusercontent.com
151.101.76.133 avatars8.githubusercontent.com
# GitHub End
```

### 更改 pip 源

```bash
mkdir ~/.pip
vim ~/.pip/conf
```

写入以下内容：

```text
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```

这样就永久修改了用户级别的 pip 下载源。

### 将 Caps 按键作为 Ctrl

`设置->硬件->输入设备->键盘->高级->Ctrl 键位置->大写锁定键作为 Ctrl`

## zsh 配置

### 更改 shell

```bash
sudo pacman -S zsh
echo $SHELL
chsh -s /bin/zsh
```

### 通过 oh-my-zsh 美化：

#### 安装 oh-my-zsh：

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

#### 安装 PowerLevel10k  主题：

```bash
#安装字体
sudo pacman -S nerd-fonts-complete
#安装powerlevel10k主题
sudo pacman -Sy --noconfirm zsh-theme-powerlevel10k
#配置powerlevel10k
echo 'source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme' >>! ~/.zshrc
#使配置立即生效
source ~/.zshrc
```

如果遇到报错信息如下：

```text
Your git prompt may disappear or become slow.
Run the following command to retry with extra diagnostics:
GITSTATUS_ENABLE_LOGGING=1 gitstatus_start -s 1 -u 1 -d 1 -m -1 POWERLEVEL9K

OUTPUT:

[ERROR]: gitstatus failed to initialize.
......
```

可参考 <https://my.oschina.net/u/4231975/blog/4288020> 解决。

如需重新配置该主题，可运行命令 `10k configure`。

#### 安装自动补全插件：

由于 manjaro 自带该插件，只需要激活即可，编辑 `~/.zshrc`：

```bash
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
```

也可以重新克隆下来进行添加：

```bash
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

然后在 `~/.zshrc` 中找到 `plugins=(<other plugins> zsh-autosuggestions)` 添加上即可。

#### 安装语法高亮插件

由于 manjaro 自带该插件，只需要激活即可，编辑 `~/.zshrc`：

```bash
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

也可以重新克隆下来进行添加：

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

然后在 `~/.zshrc` 中找到 `plugins=(<other plugins> zsh-syntax-highlighting)` 添加上即可。

#### 安装 autojump 插件

先安装 autojump: `sudo pacman -S autojump`

然后在 `~/.zshrc` 中找到 `plugins=(<other plugins> autojump)` 添加。

#### 启用 extract 插件

该插件使得仅需使用 `x` 命令即可对压缩文件进行解压。

在 `~/.zshrc` 中找到 `plugins=(<other plugins> extract)` 添加。

### 安装 thefuck

```bash
pip install --user thefuck
```

在 `~/.zshrc` 文件末尾添加 `eval $(thefuck --alias)`，保存退出。

### 好用的 alias

在 `~/.zshrc` 文件中添加以下内容：

```text
alias vim='nvim'
alias cs='cowsay'
alias x='extract'
alias mv='mv -i'  # 防止移动覆盖误操作
alias vzsh='vim ~/.zshrc'
alias szsh='source ~/.zshrc'
alias ne='neofetch | lolcat'
```

### 额外配置

在 `~/.zshrc` 文件最后添加了 `cowfortune`，这样，每次打开终端或是激活配置文件时会打印一条名人名言。

### 使配置生效

`source ~/.zshrc`

## 安装软件配置

### 安装 deb 包

1. 安装 debtap，用于将 deb 包转为 archlinux 包，`yay -S debtap`
2. 替换源，`debtap` 文件位于 `/usr/bin/debtap`，先将其进行备份，然后 sed 替换
	1. `sudo sed -i "s/http:\/\/ftp.debian.org/https:\/\/mirrors.ustc.edu.cn/g" /usr/bin/debtap`
	2. `sudo sed -i "s/http:\/\/archive.ubuntu.com/https:\/\/mirrors.ustc.edu.cn/g" /usr/bin/debtap`
3. 执行升级，`sudo debtap -u`
4. 使用方法，`sudo debtap xxxx.deb`
5. 安装转换好的本地包，`sudo pacman -U xxxx.tar.xz`

> 注意：对 deb 包进行转化时，会提示输入包名，以及 license。包名随意，license 可以填 GPL，加入 `-q` 参数可以略过除了编辑元数据之外的所有问题，`-Q` 参数可以略过所有的问题。

### go 配置

安装 go：`sudo pacman -S go go-tools`

配置环境变量 `GOPATH`，可以在用户目录下的 `go/` 文件夹放置 Go 文件，同时为了更好地拉取 Go 相关的软件包，可以配置环境变量 `GOPROXY`，编辑 `~/.xprofile` 文件：

```bash
export GOPATH=~/go
export PATH=$PATH:$GOPATH/bin

export GO111MODULE=on
export GOPROXY=https://goproxy.cn
```

### zathura 配置

pdf 阅读器，用户配置路径：`~/.config/zathura/zathurarc`

编辑该配置文件，不存在则手动创建，添加下面的配置命令：

```text
#以宽度自适应打开
set adjust-open "width"

#字体与字号
set font "Noto Sans CJK SC Regular 10"

#GUI相关，留空可隐藏底部statusbar
set guioptions ""

#只显示文件名，否则显示完整路径
set window-title-basename true

#增强搜索，实时搜索
set incremental-search true

#显示右侧进度条
set show-v-scrollbar true

#粘贴版
set selection-clipboard clipboard

#默认高度，像素
set window-height 760

#默认宽度
set window-width 1300
```

更多可以参考：<https://github.com/lervag/dotfiles/blob/master/zathura/.config/zathura/zathurarc>

### 设置 ngrok 内网穿透（弃用，选择 zerotier）

ngrok 官网：<https://ngrok.com/>

1. 注册并下载 ngrok，`sudo pacman -S ngrok`
2. 查看 ngrok 的 token，在 <https://dashboard.ngrok.com/auth>
3. 在内网机器上连接 ngrok 帐号，`ngrok authtoken <authtoken>`
4. 开启 ssh 服务命令
	- `systemctl enable sshd.service  # 开机启动`
	- `systemctl start sshd.service  # 立即启动`
	- `systemctl restart sshd.service  # 立即重启`
5. 启动 ngrok 并打开 22 端口转发，`ngrok tcp 22`
6. 通过访问上一步得到的 tcp 开头的地址，就可以转发到本机的 22 端口上
7. 通过 ssh 访问内网机器，`ssh -p <port> <username>@<ip>`

> Note: 所有流量都要经过 ngrok 服务器，所以比较慢，另外存在安全隐患。

### 设置 ZeroTier 内网穿透

参考：

- [Arch Wiki](https://wiki.archlinux.org/index.php/Zerotier)
- [开始使用软件定义的网络并使用ZeroTier One创建VPN](https://www.howtoing.com/getting-started-software-defined-networking-creating-vpn-zerotier-one)

#### 官网注册并创建 network

注册帐号，创建网络，参考前面的链接即可。

#### manjaro 中进行配置

- 进行安装，`sudo pacman -S zerotier-one`
- 启动服务，`sudo systemctl start zerotier-one.service`
- 还可以将其自启动，`sudo systemctl enable zerotier-one.service`
- 加入 ZeroTier 网络，`sudo zerotier-cli join <NetworkID>`，如果输出 `200 join OK`，则成功
- 在 ZeroTier Web 控制台中，就可以查看到本机已经加入网络，将其勾选进行授权，允许网络桥接
- 这样，本机就成功加入个人的局域网，通过在网络中的其它主机进行 ping 验证

#### windows 中进行配置

- 官网安装客户端
- 在任务栏中找到程序，右击 `Join Network`，填写 `NetworkID`
- 在 ZeroTier Web 控制台中，就可以查看到本机已经加入网络，将其勾选进行授权，允许网络桥接
- 这样，本机就成功加入个人的局域网，通过在网络中的其它主机进行 ping 验证

### 科学上网

- 安装 shadowsocks 软件，`sudo pacman -S shadowsocks-qt5`
- 安装 genpac，`pip install genpac`
- 命令行生成 pac 文件，`genpac --proxy="SOCKS5 127.0.0.1:10808" --gfwlist-proxy="SOCKS5 127.0.0.1:1080" -o autoproxy.pac --gfwlist-url="https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt"`
- 设置系统自动代理，`设置-代理-使用代理自动配置 URL-填入之前生成的 pac 文件路径`

ssr: `yay -S electron-ssr`

v2ray: `sudo pacman -S qv2ray`  # 选择该代理即可，前面的代理软件备用

#### 终端代理

- 安装 proxychains，`yay -S proxychains-ng`
- `sudo vi /etc/proxychains.conf`，将 `proxy_dns` 这一行注释，使得 proxychains 能够代理 yay
- 编辑 proxychains 配置，`sudo vi /etc/proxychains.conf`，将最后面的 `sock4 127.0.0.1 9095` 改为 `socks5 127.0.0.1 10808`

#### git 代理

##### https 访问
为 `github.com` 设置 `socks5` 代理，`git config --global http.https://github.com.proxy socks5://127.0.0.1:10808`

取消代理： `git config --global --unset http.https://github.com.proxy`

##### ssh 访问

需要修改 `~/.ssh/config` 文件，没有的话就新建一个，同样仅为 `github.com` 设置代理：

```bash
Host github.com
	User git
	ProxyCommand nc -v -x 127.0.0.1:10808 %h %p
```

## 桌面美化

### dock 栏

安装：`sudo pacman -S latte-dock`

### 小部件

- todolist
- netspeed
- event calendar
- Active Window Control
- Simple Monitor

## 常用软件

```bash
# 日常软件
yay -S deepin-wine-wechat
yay -S deepin-wine-tim
# yay -S deepin-wine-baidupan
sudo pacman -S flameshot  # 截图软件
sudo pacman -S netease-cloud-music
sudo pacman -S wps-office wps-office-mui-zh-cn  # wps，需要英文版的话可去掉后面那个包
sudo pacman -S ttf-wps-fonts  # wps 字体
sudo pacman -S libreoffice  # 备用
sudo pacman -S calibre
sudo pacman -S chromium
sudo pacman -S google-chrome
sudo pacman -S neofetch  # 与 screenfetch 类似，获取本机信息
sudo pacman -S lolcat  # 更好看的 cat
# sudo pacman -S nutstore  # 坚果云，无法使用
sudo pacman -S typora  # markdown 编辑器
yay -S xdman  # 下载神器
sudo pacman -S uget aria2  # 下载工具
# 将 uGet -> Edit -> Settings -> Plug-in 中的设置设为 aria2
sudo pacman -S texlive-most texlive-langchinese  # latex
sudo pacman -S obs-studio  # 屏幕录像软件
sudo pacman -S mpv  # 视频播放器
# yay -S utools  # 辅助工具，包含很多功能，目前这种方式无法装上
yay -S debtap  # 将 deb 包转为 arch linux 包
# 利用 debtap 将 utools 包进行转换后安装
sudo pacman -S net-tools  # 可以使用 ifconfig 和 netstat

sudo pacman -S zathura  # vi 操作的 pdf 阅读器
sudo pacman -S gimp  # Linux 下的 PS
# sudo pacman -S bleachbit  # 快速释放磁盘空间，GUI 显示有问题，弃用

sudo pacman -S teamviewer
yay -S nomachine  # 远程桌面共享工具，配合 zerotier 可以完美远程控制，但是画质一般，另外网络有一定延迟
sudo pacman -S remmina  # 远程桌面共享工具
sudo pacman -S xrdp  # 远程桌面共享工具，RDP 协议
yay -S xorgxrdp-git

sudo pacman -S syncthing  # 文件同步
yay -S syncthingtray  # 文件同步客户端
sudo pacman -S inotify-tools

yay -S insync  # onedrive 和 google drive

sudo pacman -S telegram-desktop

# 开发软件
sudo pacman -S neovim
sudo pacman -S emacs
sudo pacman -S visual-studio-code-bin  # vs code
sudo pacman -S nodejs
sudo pacman -S npm
npm config set registry https://registry.npm.taobao.org  # 设置 npm 源
sudo pacman -S ranger  # 终端下的文件浏览器
sudo pacman -S clang
sudo pacman -S docker docker-compose  # Docker
sudo pacman -S jdk  # 安装最新版本 jdk
sudo pacman -S jdk8-openjdk  # 安装 jdk8

# 好玩的命令
sudo pacman -S toilet  # 用于打印 ASCII 字符串，可以配置字体等
sudo pacman -S cowsay  # 显示由母牛或其它动物输入的文本
sudo pacman -S cowfortune  # 名人名言
sudo pacman -S figlet  # 将文本转换为 ASCII 字符串
sudo pacman -S sl  # 在屏幕上绘制 ASCII 蒸汽机火车
sudo pacman -S cmatrix  # 黑客帝国，代码矩阵
```

## 考虑安装的软件

```bash
foxitreader   #PDF阅读器
guake      #带快捷启动的终端
audacious     #简洁的本地音乐播放器
deepin-screenshot      #深度截图工具，可同时进行标注
krita    #媲美PS的软件
filezilla       #FTP工具
virtualbox      #linux平台首选的虚拟机软件（全面更新系统后重启后再安装）
yay -S sendanywhere   #跨平台的文件传输软件
yay -S  geogebra         #图形计算器，支持函数，几何，代数，微积分，统计及3D数学
```

## 装好 Manjaro 必须要有的习惯：

1. 必须要做 timeshift，以防哪天玩坏只能重装
2. 每天要开机执行一遍 `sudo pacman -Syyu`
3. 防止滚挂的最好办法就是及时滚

### 好用的备份脚本

```bash
#!/bin/bash

# define
dayofweek=`date "+%u"`
today=`date "+%Y%m%d"`
source=/data/
backup=/backup/

# action
cd $backup

if [ $dayofweek -eq 1 ]; then
　　if [ ! -f "full$today.tar.gz" ]; then
　　　　rm -rf snapshot
　　　　tar -g snapshot -zcf "full$today.tar.gz" $source --exclude $sourceserver.log
　　fi
else
　　if [ ! -f "inc$today.tar.gz" ]; then
　　　　tar -g snapshot -zcf "inc$today.tar.gz" $source --exclude $sourceserver.log
　　fi
fi
```

在cron里设置,每周一凌晨2点执行(每周一全备份,其余时间增量备份)

## 注意事项

### 复制粘贴

manjaro kde 框选文字即复制，用鼠标中键可以粘贴！但是和系统剪贴板，即 ctrl+c ctrl+v 不是同一个，他们互不干扰！

