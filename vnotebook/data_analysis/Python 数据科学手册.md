# Python 数据科学手册

> 每章介绍一到两个 Python 数据科学中的重点工具包。首先从 IPython 和 Jupyter 开始，它们提供了数据科学家需要的计算环境；第2章讲解能提供 ndarray 对象的 NumPy，它可以用 Python 高效地存储和操作大型数组；第3章主要涉及提供 DataFrame 对象的 Pandas，它可以用 Python 高效地存储和操作带标签的/列式数据；第4章的主角是 Matplotlib，它为 Python 提供了许多数据可视化功能；第5章以 Scikit-Learn 为主，这个程序库为最重要的机器学习算法提供了高效整洁的 Python 版实现。

## IPython

- 符号 `？` 用于浏览文档，符号 `??` 用于浏览源代码。
- 利用 `*` 符号来实现通配符匹配方法。

**快捷键：**

| 快捷键    | 动作                     |
| :-------- | :----------------------- |
| Ctrl + a  | 将光标移到本行的开始处   |
| Ctrl + e  | 将当标移到本行的结尾处   |
| Ctrl + b  | 将光标回退一个字符       |
| Ctrl + f  | 将光标前进一个字符       |
| Backspace | 删除前一个字符           |
| Ctrl + d  | 删除后一个字符           |
| Ctrl + k  | 从光标开始剪切至行的末尾 |
| Ctrl + u  | 从行的开头剪切至光标     |
| Ctrl + y  | 粘贴之前剪切的文本       |
| Ctrl + t  | 交换前两个字符           |
| Ctrl + p  | 获取前一个历史命令       |
| Ctrl + n  | 获取后一个历史命令       |
| Ctrl + r  | 对历史命令的反向搜索     |

**IPython 魔法命令：**

魔法命令有两种：`行魔法(line magic)` 和 `单元魔法(cell magic)`。行魔法以单个 `%` 字符作为前缀，作用于单行输入；单元魔法以两个 `%%` 作为前缀，作用于多行输入。

- `%paste`: 粘贴并执行
- `%cpaste`: 打开一个交互式多行输入提示，在这个提示下粘贴并执行一个或多个代码块
- `%run`: 执行外部代码，例如，`%run “filename”.py` 
- `%timeit`: 计算代码运行时间
- `%%timeit`: 以处理多行输入的代码运行时间
- `%magic`: 获得可用魔法函数的通用描述以及一些示例
- `%lsmagic`: 快速而简单地获得所有可用魔法函数的列表
- `%history`: 一次性获取此前所有的输入历史

IPython 中 `In[1]:/Out[1]:` 形式的提示，不仅仅是好看的装饰形式，还给出了在当前会话中如何获取输入和输出历史的线索。`In` 对象是一个列表，按照顺序记录所有的命令（列表的第一项是一个占位符，以便 `In[1]` 可以表示第一条命令）；`Out` 对象是一个字典，它将输入数字映射到相应的输出（如果有的话）。

要禁止一个命令的输出，最简单的方式就是在行末尾处添加一个分号 `;`。

IPython 可以直接执行 shell 命令，一行中任何在 `!` 之后的内容将不会通过 Python 内核运行，而是通过系统命令行运行。

shell 命令不仅可以从 IPython 中调用，还可以和 IPython 命名空间进行交互。需要注意的是，这些结果并不以列表的形式返回，而是以 IPython 中定义的一个特殊 shell 返回类型的形式返回。该类型可以像列表一样操作，同时还有其他功能，例如 grep 和 fields 方法以及 s、n 和 p 属性，允许轻松搜索、过滤和显示结果。

可用的类似 shell 的魔法函数有：`%cd`、`%cat`、`%cp`、`%env`、`%ls`、`%man`、`%mkdir`、`%more`、`%mv`、`%pwd`、`%rm` 和 `%rmdir`。

IPython 中最方便的调试界面就是 `%debug` 魔法命令。

**部分调试命令：**

| 命令       | 描述                                   |
| ---------- | -------------------------------------- |
| list       | 显示文件的当前路径                     |
| h(elp)     | 显示命令列表，或查找特定命令的帮助信息 |
| q(uit)     | 退出调试器和程序                       |
| c(ontinue) | 退出调试器，继续运行程序               |
| n(ext)     | 调到程序的下一步                       |
| <enter>    | 重复前一个命令                         |
| p(rint)    | 打印变量                               |
| s(tep)     | 步入子进程                             |
| r(eturn)   | 从子进程跳出                           |

IPython 中执行代码计时和分析的操作函数：

- `%time`: 对单个语句的执行时间进行计时
- `%timeit`: 对单个语句的重复执行进行计时，以获得更高的准确度
- `%prun`: 利用分析器运行代码
- `%lprun`: 利用逐行分析器运行代码
- `%memit`: 测量单个语句的内存使用
- `%mprun`: 通过逐行的内存分析器运行代码

## NumPy

**创建数组**：

- `np.zeros(shape, dtype)`: 创建一个形状为 shape 的数组，数组的值都是 0
- `np.ones(shape, dtype)`: 创建一个形状为 shape 的数组，数组的值都是 1
- `np.full(shape, num)`: 创建一个形状为 shape 的数组，数组的值都是 num
- `np.arange(start, end, step)`: 创建一个数组，数组的值是一个线性序列，从 start 开始，到 end 结束，不包含 end，步长为 step
- `np.linspace(start, end, length)`: 创建一个 length 个元素的数组，这些数均匀地分配到 start–end，包含start, end
- `np.random.random(shape)`: 创建一个形状为 shape、在 0~1 均匀分布的随机数组成的数组
- `np.random.normal(mu, sigma, shape)`: 创建一个形状为 shape、均值为 mu、方差为 sigma 的正态分布的随机数数组
- `np.random.randint(start, end, shape)`: 创建一个形状为 shape、[start, end) 区间的随机整型数组
- `np.eye(num)`: 创建一个 num $\times$ num 的单元矩阵
- `np.empty(shape)`: 创建一个形状为 shape 的未初始化数组，数组的值是内存空间中的任意值

**NumPy 标准数据类型**：`bool_, int_, intc, intp, int8, int16, int32, int64, uint8, uint16, uint32, uint64, float_, float16, float32, float64, complex_, complex64, complex128`

每个 NumPy 数组都有以下几个属性，`ndim`(数组的维度)、`shape`(数组每个维度的大小)、`size`(数组的总大小)、`dtype`(数组的数据类型)、`itemsize`(每个数组元素的字节大小)、`nbytes`(数组总字节大小，可认为等于 itemsize 和 size 的乘积)。

NumPy 的一维数组的索引与 Python 列表相同，但多维数组中，可以用逗号分隔的索引元组获取元素。

> Note: 和 Python 的列表不同，NumPy 数组是固定类型的。这意味着当试图将一个浮点值插入一个整型数组时，浮点值会被截短成整型。

NumPy 的数组切片与 Python 类似，注意多维切片同样是输入元组。

> Note: 和 Python 列表切片不同，NumPy 数组切片返回的是数组数据的视图，而不是数值数据的副本，即修改子数组，会发现原始数据也被修改了。

因此，可以通过 `copy()` 方法创建数组的副本。

数组的变形是通过 `reshape(shape)` 函数来实现的。

**数组的拼接**：

1. `np.concatenate`: 将数组元组或数组列表作为第一个参数，可连接多个数组。

   ```python
   x = np.array([1, 2, 3])
   y = np.array([3, 2, 1])
   z = [99, 99, 99]
   print(np.concatenate([x, y, z]))
   # [1, 2, 3, 3, 2, 1, 99, 99, 99]
   
   # 二维数组的拼接
   grid = np.array([[1, 2, 3],
                    [4, 5, 6]])
   # 沿着第一个轴拼接
   print(np.concatenate([grid, grid]))
   # array([[1, 2, 3],
   #	    [4, 5, 6],
   #        [1, 2, 3],
   #        [4, 5, 6]])
   
   # 沿着第二个轴拼接（从 0 开始索引）
   print(np.concatenate([grid, grid], axis=1))
   # array([[1, 2, 3, 1, 2, 3],
   #        [4, 5, 6, 4, 5, 6]])
   ```

2. 沿着固定维度处理数组时，使用 `np.vstack` (垂直栈) 和 `np.hstack` (水平栈) 函数更简洁：

   ```python
   x = np.array([1, 2, 3])
   grid = np.array([[9, 8, 7],
   			  [6, 5, 4]])
   # 垂直栈数组
   print(np.vstack([x, grid]))
   # array([[1, 2, 3],
   #        [9, 8, 7],
   #        [6, 5, 4]])
   
   # 水平栈数组
   y = np.array([[99],
                 [99]])
   print(np.hstack([grid, y]))
   # array([9, 8, 7, 99], 
   #       [6, 5, 4, 99])
   ```

   与之类似，`np.dstack` 将沿着第三个维度拼接数组。



