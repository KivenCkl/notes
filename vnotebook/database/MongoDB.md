# MongoDB

MongoDB 是由 C++ 语言编写的非关系型数据库，是一个基于分布式文件存储的开源数据库系统，其内容存储形式类似于 JSON 对象，它的字段值可以包含其它文档、数组及文档数组，非常灵活。

## Python 操作 MongoDB

开始之前，确保安装了 MongoDB 并启动其服务，并且安装了 pymongo 库。

首先，引入该库

```python
import pymongo
```

### 创建连接


