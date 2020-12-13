# pyinstaller

## 简述

pyinstaller 打包的流程：读取编写好的 python 脚本，分析其中调用的模块和库，然后收集这些文件的副本（包括 Python 的解释器）。最后把副本与脚本，可执行文件等放在一个文件夹中，或者可选地封装在一个可执行文件中。

## 基本使用方法

### 安装 pyinstaller

```bash
pip install pyinstaller
```

### 生成 spec 文件

进入主程序目录，输入 `pyi-makespec -w main.py` 生成 `main.spec` 文件。

几个常用参数

| 参数                    | 说明                                        |
| :---------------------- | :------------------------------------------ |
| -F,-onefile             | 打包一个单个文件                              |
| -D,-onedir              | 打包多个文件，在 dist 中生成很多依赖文件        |
| -w,-windowed,-noconcole | 当程序启动的时候不会打开命令行(只对windows有效) |

### 根据需要编辑 spec 文件

onefile 模式

```python
# -*- mode: python ; coding: utf-8 -*-

block_cipher = None


a = Analysis(['main.py'],
             pathex=['C:\\Users\\lunckl\\Desktop\\qcm-python'],
             binaries=[('./NovaQCM.exe', '.')],  # non-python modules needed by the scripts
             datas=[('./*.ico', '.'), ('./*.png', '.'), ('./QCM/*.txt', 'QCM')],  # non-binary files included in the app
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher,
             noarchive=False)
pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          a.binaries,
          a.zipfiles,
          a.datas,
          [],
          name='qcm',
          debug=False,
          bootloader_ignore_signals=False,
          strip=False,
          upx=True,
          upx_exclude=[],
          runtime_tmpdir=None,
          console=False,
          icon='favicon.ico')

```

onedir 模式

```python
# -*- mode: python ; coding: utf-8 -*-

block_cipher = None


a = Analysis(['main.py'],
             pathex=['C:\\Users\\lunckl\\Desktop\\qcm-python'],
             binaries=[('./NovaQCM.exe', '.')],
             datas=[('./*.ico', '.'), ('./*.png', '.'), ('./QCM/*.txt', 'QCM')],
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher,
             noarchive=False)
pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          [],
          exclude_binaries=True,
          name='qcm',
          debug=False,
          bootloader_ignore_signals=False,
          strip=False,
          upx=True,
          console=False,
          icon='favicon.ico')
coll = COLLECT(exe,
               a.binaries,
               a.zipfiles,
               a.datas,
               strip=False,
               upx=True,
               upx_exclude=[],
               name='QCM')

```

### 执行打包命令

```bash
pyinstaller main.spec
```

### 进行测试

进入生成的 dist 目录，执行 `main.exe`

## 常见问题

### 打包多进程、线程

用 pyinstaller 打包好 exe 后，双击运行，会出现无限循环地进入主程序的情况。此时需要在调用多进程的前面加上如下的代码：

```python
if __name__ == "__main__":
    multiprocessing.freeze_support()  # 不加这句，打包的程序就进不了下面的子进程了
    p1 = multiprocessing.Process(target=callback, target=(, ))
    p1.start()
```

1. 因为开启子进程是不支持打包 exe 文件的，所以会不停向操作系统申请创建子进程，而 multiprocessing.freeze_support() 作用就是支持打包到 windows 的 exe 文件。
2. 多进程的程序运行后，如果直接关闭控制台窗口，那么整个程序都会退出，如果是进入任务管理，单独结束控制窗口的进程，如果子进程不是守护进程，那么子进程还是会继续运行。
3. 如果是多线程，则没有这个问题，可以直接打包。

### pyinstaller 打包一个 exe 并加入内置图片

编辑生成的 main.spec 文件，修改 `datas` 列表，添加数据的格式为：`datas = [('source_path1', 'exe_dir1'), ('source_path2', 'exe_dir2')]`，可以使用通配符

- `source_path`: 资源文件
- `exe_dir`: 把资源文件放在 exe 程序中的文件夹。可以直接使用 `.` 表示把资源文件放在 exe 程序的顶级文件夹中。

最后需要在源代码的资源路径引用中进行如下修改：

```python
import os
import sys

def resource_path(relative_path):
    """Get absolute path to resource
    works for dev and for PyInstaller"""
    if getattr(sys, 'frozen', False):
        base_path = sys._MEIPASS
    else:
        base_path = os.getcwd()
    return os.path.join(base_path, relative_path)

pic_path = resource_path('pic.png')
```

pyinstaller 会将文件夹的路径信息存储在 `sys.MEIPASS` 中，当使用的是单文件打包的方式，`sys.MEIPASS` 的值就是程序运行时创建 _MEIxxxxxx临时目录的绝对路径。路径一般在 `C:\Users\user\AppData\Local\Temp\_MEIxxxxx`

修改好 `.spec` 文件和源代码后，重新打包即可，`pyinstaller main.spec`

对于以 dir 方式进行打包，则只需要修改 `.spec` 文件，添加资源文件即可。

### 调用外部程序使用无命令行窗口模式会出现程序报错

问题出在 subprocess 上面，简单来说，打包关闭了命令行窗口，stdin, stdout 无处安放，参考以下代码修改即可。

```python
def run_stuff(command_line):
    output_filename = 'somefile.txt'
    output_file = open(output_filename, "w")

    if gui_mode:
        result = subprocess.call(command_line, shell=True, stdout=outputFile, stderr=subprocess.STDOUT)
    else:
        proc = subprocess.Popen(command_line, shell=True, stdout=subprocess.PIPE, stderr=subprocess.STDOUT, stdin=subprocess.PIPE)
        proc.stdin.close()
        proc.wait()
        result = proc.returncode
        output_file.write(proc.stdout.read())
```

## 参考

[PyInstaller打包详解](https://yujunjiex.gitee.io/2018/10/18/PyInstaller打包详解/)
[pyinstaller 打包成 exe 遇到的一些坑](http://blog.gqylpy.com/gqy/19384/)
[Python subprocess.call() fails](https://stackoverflow.com/questions/337870/python-subprocess-call-fails-when-using-pythonw-exe)
[PyInstaller Manual](https://pyinstaller.readthedocs.io/en/stable/index.html)