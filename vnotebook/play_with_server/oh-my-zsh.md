# oh-my-zsh

## 引言

记录 `zsh` 和 `oh my zsh` 的配置使用教程。

<!--more-->

## 准备

### 查看当前环境 shell

```bash
echo $SHELL
```

### 查看系统自带哪些 shell

```bash
cat /etc/shells
```

## zsh

> 参考：<https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH>

### 安装 zsh

```bash
yum install zsh  # CentOS 安装
brew install zsh  # mac 安装
apt install zsh  # Ubuntu 安装
```

### 将 zsh 设置为默认 shell

```bash
chsh -s $(which zsh)
```

可以通过 `echo $SHELL` 查看当前默认的 shell，如果没有改为 `/bin/zsh`，那么需要重启 shell。

## oh my zsh

> 参考：<https://github.com/robbyrussell/oh-my-zsh>

### 安装 oh my zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"  # curl 方式
sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"  # wget 方式
```

### 配置 oh my zsh

通过修改 `~/.zshrc` 文件进行定制，然后需要 `source ~/.zshrc` 将配置生效。

#### zsh 主题

通过如下命令可以查看可用的 `Theme`：

```bash
ls ~/.oh-my-zsh/themes/
```

编辑 `~/.zshrc` 文件，将 `ZSH_THEME="candy"`，即将主题修改为 `candy`。

#### zsh 扩展

> 参考：<https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins>

通过如下命令可以查看可用的 `plugins`：

```bash
ls ~/.oh-my-zsh/plugins/
```

 在`~/.zshrc`中找到`plugins`关键字，就可以自定义启用的插件了，系统默认加载`git`。

##### git 插件

命令内容可以参考`cat ~/.oh-my-zsh/plugins/git/git.plugin.zsh`。

常用的：

```bash
gapa    git add --patch
gc!    git commit -v --amend
gcl    git clone --recursive
gclean    git reset --hard && git clean -dfx
gcm    git checkout master
gcmsg    git commit -m
gco    git checkout
gd    git diff
gdca    git diff --cached
gp    git push
grbc    git rebase --continue
gst    git status
gup    git pull --rebase
```

##### extract 插件

解压文件用的，所有的压缩文件，都可以直接`x filename`，不用记忆参数

当然，如果你想要用`tar`命令，可以使用`tar -`加`tab`键，zsh 会列出参数的含义。

##### autojump 插件

```bash
yum install autojump-zsh  # CentOS
brew install autojump  # Mac
apt install autojump  # Ubuntu
```

`CentOS`安装好之后，需要在`~/.zshrc`中配置一下，除了在`plugins`中增加`autojump`之外，还需要添加一行：

```bash
[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh
```

安装好之后，记得`source ~/.zshrc`，然后你就可以通过`j+目录名`快速进行目录跳转。支持目录名的模糊匹配和自动补全。

## 常用快捷键

- 命令历史记录
  - 一旦在 shell 敲入正确命令并能执行后，shell 就会存储你所敲入命令的历史记录（存放在`~/.zsh_history` 文件中），方便再次运行之前的命令。可以按方向键↑和↓来查看之前执行过的命令
  - 可以用 `r`来执行上一条命令
  - 使用 `ctrl-r` 来搜索命令历史记录
- 命令别名
  - 可以简化命令输入，在 `.zshrc` 中添加 `alias shortcut='this is the origin command'` 一行就相当于添加了别名
  - 在命令行中输入 `alias` 可以查看所有的命令别名

## 使用技巧

- 连按两次Tab会列出所有的补全列表并直接开始选择，补全项可以使用 ctrl+n/p/f/b上下左右切换
- 智能跳转，安装了 autojump 之后，zsh 会自动记录你访问过的目录，通过 j 目录名 可以直接进行目录跳转，而且目录名支持模糊匹配和自动补全，例如你访问过 hadoop-1.0.0 目录，输入j hado 即可正确跳转。j --stat 可以看你的历史路径库。
- 命令选项补全。在zsh中只需要键入 tar -<tab> 就会列出所有的选项和帮助说明
- 在当前目录下输入 .. 或 ... ，或直接输入当前目录名都可以跳转，你甚至不再需要输入 `cd` 命令了。在你知道路径的情况下，比如 `/usr/local/bin` 你可以输入`cd /u/l/b` 然后按进行补全快速输入
- 目录浏览和跳转：输入 d，即可列出你在这个会话里访问的目录列表，输入列表前的序号，即可直接跳转。
- 命令参数补全。键入` kill ` 就会列出所有的进程名和对应的进程号
- 更智能的历史命令。在用或者方向上键查找历史命令时，zsh支持限制查找。比如，输入ls,然后再按方向上键，则只会查找用过的ls命令。而此时使用则会仍然按之前的方式查找，忽略 ls
- 多个终端会话共享历史记录
- 通配符搜索：`ls -l **/*.sh`，可以递归显示当前目录下的 shell 文件，文件少时可以代替 `find`。使用 `**/` 来递归搜索
- 扩展环境变量，输入环境变量然后按 就可以转换成表达的值
- 在 .zshrc 中添加 `setopt HIST_IGNORE_DUPS` 可以消除重复记录，也可以利用 `sort -t ";" -k 2 -u ~/.zsh_history | sort -o ~/.zsh_history` 手动清除

### 参考

- [zsh+on-my-zsh配置教程指南](https://michael728.github.io/2018/03/11/tools-zsh-tutorial/)
- [oh my zsh]( https://github.com/robbyrussell/oh-my-zsh )
