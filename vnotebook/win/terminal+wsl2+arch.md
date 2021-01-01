# terminal+wsl2+arch

结合 `windows terminal` 与 `wsl2`，可以很好地将 linux 开发环境结合到 win 平台下，再加上 `Arch` 的加持，简直爽到炸裂。

总的思路是首先在 win 中安装 `windows terminal` ，并开启 `wsl2`，然后安装 `arch`，设置 `X server`，最后就是 `happy coding`。

## Windows Terminal

在微软商店中搜索下载即可，可稍作修改，参考 [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/get-started)

我的基本配置如下，主要设置了字体以及开启了透明度。

```json
{
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "defaultProfile": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
    "copyOnSelect": false,
    "copyFormatting": false,

    "profiles":
    {
        "defaults":
        {
            // Put settings here that you want to apply to all profiles.
            "colorScheme": "One Half Dark",
            "fontFace": "FiraCode Nerd Font",
            "fontSize": 11,
            "padding": "0, 0, 0, 0",
            "useAcrylic": true,
            "acrylicOpacity": 0.8,
            "cursorShape": "emptyBox",
       		"antialiasingMode": "cleartype",
        },
        "list":
        [
            {
                // Make changes here to the powershell.exe profile.
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": false
            },
            {
                // Make changes here to the cmd.exe profile.
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "name": "Command Prompt",
                "commandline": "cmd.exe",
                "hidden": false
            },
            {
                "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                "hidden": false,
                "name": "Azure Cloud Shell",
                "source": "Windows.Terminal.Azure"
            },
            {
                "guid": "{a5a97cb8-8961-5535-816d-772efe0c6a3f}",
                "hidden": false,
                "name": "Arch",
                "source": "Windows.Terminal.Wsl",
                "startingDirectory": "//wsl$/Arch/home/kiven/",
                "icon": "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Archlinux-icon-crystal-64.svg/1200px-Archlinux-icon-crystal-64.svg.png"
            },
            {
                "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
                "hidden": false,
                "name": "Ubuntu-20.04",
                "source": "Windows.Terminal.Wsl",
                "startingDirectory": "//wsl$/Ubuntu-20.04/home/kiven/"

            }
        ]
    },

    // Add custom color schemes to this array.
    // To learn more about color schemes, visit https://aka.ms/terminal-color-schemes
    "schemes": [],

    // Add custom actions and keybindings to this array.
    // To unbind a key combination from your defaults.json, set the command to "unbound".
    // To learn more about actions and keybindings, visit https://aka.ms/terminal-keybindings
    "actions":
    [
        // Copy and paste are bound to Ctrl+Shift+C and Ctrl+Shift+V in your defaults.json.
        // These two lines additionally bind them to Ctrl+C and Ctrl+V.
        // To learn more about selection, visit https://aka.ms/terminal-selection
        { "command": {"action": "copy", "singleLine": false }, "keys": "ctrl+c" },
        { "command": "paste", "keys": "ctrl+v" },

        // Press Ctrl+Shift+F to open the search box
        { "command": "find", "keys": "ctrl+shift+f" },

        // Press Alt+Shift+D to open a new pane.
        // - "split": "auto" makes this pane open in the direction that provides the most surface area.
        // - "splitMode": "duplicate" makes the new pane use the focused pane's profile.
        // To learn more about panes, visit https://aka.ms/terminal-panes
        { "command": { "action": "splitPane", "split": "auto", "splitMode": "duplicate" }, "keys": "alt+shift+d" }
    ]
}

```

## wsl2

> wsl2 要求系统版本应该在 win10, version 2004, build 19041 及以上。

首先需要开启 wsl，可通过 **控制面板->程序->启用或关闭 Windows 功能->勾选 适用于 Linux 的 Windows 子系统 和 虚拟机平台** 来开启，或者以管理员身份打开 `powershell`，运行如下命令来启动需要的组件

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

重启之后，需要在 [此处](https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel) 下载并安装适用于 x64 计算机的最新 wsl2 Linux 内核更新包。

最后，打开 powershell，运行如下命令，将 wsl 的默认版本设置为 wsl2

```powershell
wsl --set-default-version 2
```

简单来说，wsl 和 wsl2 的区别在于，wsl 是指令翻译的形式来实现 linux 功能，而 wsl2 则是基于 hyper-v 虚拟技术。

## Arch

[Arch](https://archlinux.org/) 对人的吸引力我就不赘述了。

### 安装 arch wsl

在 [yuk7/ArchWSL - Releases](https://github.com/yuk7/ArchWSL/releases/tag/20.11.25.0) 中下载 `Arch.zip`，根据 [yuk7/ArchWSL -Wiki](https://github.com/yuk7/ArchWSL/wiki) 进行安装。

### 换源更新

进入 arch wsl 中后，执行以下命令（root 身份）

```bash
passed  # 设置 root 密码
```

用 `vim` 或者 `nano` 打开 `/etc/pacman.d/mirrorlist`，将 `China` 下面的源取消注释。

执行如下命令

```bash
# 初始化 keyring
pacman-key --init
pacman-key --populate
pacman -Syu
```

用 `vim` 打开 `/etc/pacman.conf`，取消 `#Color` 注释，在文件末尾加上

```conf
[archlinuxcn]
Server = https://mirrors.aliyun.com/archlinuxcn/$arch
```

执行如下命令

```bash
pacman -Syy
pacman -S archlinuxcn-keyring
```

### 创建用户

一般使用普通用户进行操作，防止误操作导致 linux 无法运行。

```bash
# 新建用户，-m 为用户创建家目录，-G wheel 将用户添加到 wheel 用户组
useradd -m -G wheel <youname>
# 设置密码
passwd <youname>
```

编辑 `/etc/sudoers`，将 `# $wheel ALL=(ALL) ALL` 的注释去掉，使得 `wheel` 组用户能够使用 `root` 权限。

在 powershell 中进入到 Arch.exe 所在文件夹，设置 wsl 默认登陆用户和默认的 wsl

```powershell
.\Arch.exe config --default-user <youname>
wsl -s Arch
```

### 安装常用软件

```bash
# 基本 linux 命令集
sudo pacman -S base-devel
# yay
sudo pacman -S yay
# yay 换源
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
# 其它一些常用的指令
sudo pacman -S neofetch lolcat bat tree
# git openssh man-pages
sudo pacman -S git openssh man-pages
# 安装 firacode 字体
sudo pacman -S nerd-fonts-fira-code
```

### 安装 zsh

```bash
sudo pacman -S zsh
# 改变当前用户默认的 shell
chsh -s /bin/zsh
```

```bash
# 使用 antigen 管理 zsh 插件
sudo yay -S antigen
```

在 `.zshrc` 中添加

```txt
# Start antigen
# Load antigen -- plugin manager
source /usr/share/zsh/share/antigen.zsh
# Load oh-my-zsh
antigen use oh-my-zsh

antigen bundle git
antigen bundle zsh-users/zsh-completions; rehash
antigen bundle pip
antigen bundle command-not-found
antigen bundle zsh-users/zsh-syntax-highlighting
antigen bundle zsh-users/zsh-autosuggestions

# Antigen done
antigen apply
# End antigen
```

```bash
# 安装 starship 美化命令行提示
sudo pacman -S starship
```

参考 [starship](https://starship.rs/)，在 `.zshrc` 中添加

```txt
eval "$(starship init zsh)"
```

执行 `exec $SHELL` 即可看到更新的效果。

### 安装 pyqt5 环境

```bash
sudo pacman -S python python-pip
sudo pacman -S pyenv
sudo pacman -S python-pipenv
sudo pacman -S pyqt5
sudo pacman -S qt5-tools
```

在 `.zshrc` 中添加

```txt
# set pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

### 安装 node 开发环境

```bash
sudo pacman -S nvm
```

在 `.zshrc` 中添加

```txt
source /usr/share/nvm/init-nvm.sh
```

```bash
# 安装 node v10.15
nvm install v10.15.0
# 设置默认版本
nvm use v10.15.0
```

```bash
# 安装 yarn
sudo pacman -S yarn
# 安装 nrm
npm install -g nrm
# 查看有哪些源
nrm ls
# 选择 taobao 源
nrm usr taobao
```

## 启用 X Server 图形环境

### win 安装 VcXsrv

首先在 windows 上安装 [VcXsrv](https://sourceforge.net/projects/vcxsrv/)

找到软件的安装路径，对两个可执行文件 `vcxsrv.exe` 和 `xlaunch.exe` 进行操作：**右击->属性->兼容性->更改高 DPI 设置->勾选替代高 DPI 缩放行为**，目的是使得 VcXsrc 在高分辨率的显示屏上具有清晰的显示效果。

打开 `xlaunch.exe`，一路默认，在 `Extra settings` 界面中，勾选 `Disable acess control`，在 `Additional parameters for VcXsrv` 里填写 `-ac`，然后保存配置文件，然后就可以双击该文件直接启动或者移至 `Startup` 文件夹中开机自启。

### 配置 arch

```bash
# 首先需要安装 xorg
sudo pacman -S xorg xorg-xinit
```

在 `.zshrc` 中添加

```txt
# wsl settings
export DISPLAY=`cat /etc/resolv.conf | grep nameserver | awk '{print $2}'`:0
export LIBGL_ALWAYS_INDIRECT=1
```

配置显示输出地址

编辑 `/etc/X11/Xwrapper.config`，修改或增加

```txt
allowed_users=anybody
```

然后就可以通过 `xeyes` 命令来测试显示是否正常了。

## 问题

大部分问题都可以参考 [Known issues](https://wsldl-pg.github.io/ArchW-docs/Known-issues/) 来解决。

对于有些 gui 是无法用 vcxsrv 的 `multi window` 的方式来呈现的，例如 `dwm`，这时候可以选择 `one large window` 来显示。

## 最后

到此，基本环境已经搭建完毕，可以很好地在 win 中使用 arch，同时还可以在 arch 中使用 win 中的命令，例如可以在任意位置 `code .` 打开 vscode，还可以使用 `explorer.exe .` 打开 win 的资源管理器，还可以在 win 中下载 `docker desktop`，之后在 arch 中直接使用 `docker`，十分方便。

## 参考

- [Arch Linux WSL2 配置记录](https://www.cnblogs.com/zsmumu/p/archlinux-wsl2.html)
- [Known issues](https://wsldl-pg.github.io/ArchW-docs/Known-issues/)
- [基础环境搭建](https://www.yuque.com/jinghui/tvcbsd/iz20k7#grr4h)