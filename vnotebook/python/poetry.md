# poetry

poetry 是一个 Python 虚拟环境和依赖管理的工具。和 pipenv 类似，另外还提供了打包和发布的功能。

官方文档：[https://python-poetry.org/docs/](https://python-poetry.org/docs/)

## 安装

```bash
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python
```

可通过命令 `poetry --version` 来确认是否安装配置完成。

## 基本使用

创建一个项目脚手架，包括基本结构、pyproject.toml 文件

```bash
poetry new my-package
```

在已有的项目中使用 poetry

```bash
poetry init
```

安装依赖

```bash
poetry add flask  # 安装最新稳定版本的flask
poetry add pytest --dev  # 指定为开发依赖
poetry add flask=2.22.0  # 指定具体版本
poetry install  # 安装 pyproject.toml 文件中的全部依赖
poetry install --no-dev  # 只安装非 development 环境的依赖
```

导出依赖

```bash
poetry export -f requirements.txt --output requirements.txt
```

追踪包

```bash
poetry show -h  # 查看 poetry show 帮助
poetry show  # 查看项目安装的依赖
poetry show -t  # 树形结构查看项目安装的依赖
```

更新包

```bash
poetry update  # 更新所有锁定版本的依赖
poetry update flask  # 更新指定的依赖
```

卸载依赖

```bash
poetry remove requests  # 会将依赖包一起卸载
```

## 虚拟环境

当 `virtualenvs.create=true` 时，执行 `poetry install` 或 `poetry add` 会监测当前项目是否有虚拟环境，没有就自动创建。

还可以通过 `poetry env use python3` 来指定创建虚拟环境时使用的 python 解释器版本。

poetry 会自动检测当前虚拟环境，如果想在当前目录对应的虚拟环境中执行命令，可以使用以下命令

```bash
poetry run python flask.py
```

如果想显式地激活虚拟环境，使用 `poetry shell`

查找当前项目地虚拟环境

```bash
poetry env list  # 加上 --full-path 可显示绝对路径
```

删除虚拟环境路径，指定对应地解析器版本即可

```bash
poetry env remove python2
poetry env remove python3
```

## config 配置

poetry 提供了全局 config 配置和特定项目的 config 配置

目前 poetry 支持的参数有

| 参数                    | 说明                                                 |
| :--------------------- | :--------------------------------------------------- |
| cache-dir              | poetry 使用的缓存目录的路径                            |
| virtualenvs.create     | 如果当前项目不存在虚拟环境是否创建，默认值是true          |
| virtualenvs.in-project | 是否将虚拟环境创建在当前项目目录下，默认值是false         |
| virtualenvs.path       | 新创建的虚拟环境的目录，默认值是{cache-dir}/virtualenvs |
| repositories.<name>    | 设置新的备用存储库，具体的参数待确定                     |

可以通过 `poetry config <key> <value>` 来进行配置

本地参数配置

```bash
poetry config virtualenvs.create false --local
```

重置配置

```bash
poetry config virtualenvs.create --local --unset
```

列出当前配置

```bash
poetry config --list
```

> Note: 列出配置时，包括了全局和本地的配置，本地的配置会覆盖全局的参数

## 参考

- [Python包管理之poetry的使用](https://juejin.im/post/6844904083464126472)
- [https://python-poetry.org/docs/](https://python-poetry.org/docs/)