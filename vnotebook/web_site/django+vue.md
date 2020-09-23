# django+vue

## 项目创建

### 创建 Django2 项目

```bash
django-admin startproject <mysite>
```

### 创建后端服务 app

```bash
django-admin startapp <backend>
```

### 创建 vue 项目

```bash
vue create <frontend>
```

## 存在问题

### 无法访问静态资源

> vue cli 3.0 build 之后在 dist 目录中没有 static 文件夹，但是 django 只认 static

解决：在 `<frontend>` 目录中创建 `vue.config.js`，并添加以下内容

```txt
module.exports = {
    assetsDir: 'static'
}
```

重新 build，会在静态文件上一层添加 static 目录。

### 使用 mysql 数据库

激活 mysql 时，出现错误

> no module named MySQLdb

这是因为 django 默认导入的驱动是 MySQLdb，但是其对于 py3 有很大问题，所以改为 PyMySQL 驱动

解决：安装 pymysql, `pip install pymysql`，并编辑项目文件下的 `__init__.py`，加入以下内容

```python
import pymysql

pymysql.install_as_MySQLdb()
```

运行 django 服务时，出现错误

> RuntimeError: cryptography is required for sha256_password or caching_sha2_password

解决：安装 cryptography

```bash
pip install cryptography
```