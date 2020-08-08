## 目标

为了研究在一块板上布置多个弹簧振子对板的减振效果，给定板边缘一个初始位移，观测弹簧的变形以及板的变形情况。结构如下图所示，利用 ANSYS Workbench 有限元软件进行仿真。

![弹簧振子分布](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185323.png)

<!--more-->

## 结构说明

板的长宽均为 500 mm，厚度为 2 mm，材料为铝，使用壳单元，弹簧振子均匀分布在板上，共 25 个，弹簧的刚度为 $9.593\times 10^3​$ N/m，弹簧上端有个质点单元，质量为 0.027 kg。

观察分析该结构，可以发现其在 y 方向（以水平方向为 x 轴，纵向为 y 轴，垂直纸面向外为 z 轴）为对称结构，可以仅分析单条（即水平 5 个弹簧振子布置）的变形，然后通过对结果进行阵列得到最终结果。

![单元结构](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185527.png)

## 实现过程

本仿真环境为 `Win10, ANSYS Workbench 18.0`。

为了实现最后的对结果集进行阵列，需打开 Workbench 的测试功能，选择工具栏中 `Tools -> Options -> Appearance` ，勾选 `Beta Options`。

![Workbench设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185555.png)

OK，一切就绪。

首先选择谐响应分析（动力学分析）`Harmonic Response` ，设置 `Engineering Data` 添加 `Aluminum Alloy` 材料。

接着绘图 `Geometry` ，可选择自己顺手的工具绘制导入即可。结构比较简单，我使用 `DesignModeler` 绘制该结构。注意的是板只需要绘制一个面即可，Workbench 会自动使用壳单元进行分析，之后输入厚度就好了。另外就是必须在面上定义好 5 个点，用于之后连接弹簧。

![模型](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185624.png)

然后双击 `Model` 打开 `Mechanical` 软件，设置板的厚度以及材料。

![板的属性设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185700.png)

定义 5 个质点 `Point Mass` ，右击选择 `Promote to Remote Point` ，即布置在远端点。并设置其质量为 0.027 kg。

![质点位置设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185755.png)

![质点质量设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185834.png)

对远端点进行设置，选择自由点 `Free Standing`，并设置其各自位置。

![远端点设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185856.png)

设置好 5 个质点之后，就只剩下弹簧的连接了。右击 `Model` ，选择插入连接件(`Connections`) 中的弹簧(`Spring`)，接着设置其刚度，一端连接质点位置（同远端点位置），另一端连接板上对应的点（之前绘制模型中设置的点）。按照同样设置好 5 个弹簧。

![弹簧设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505185937.png)

![弹簧模型](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505190000.png)

设置扫频范围以及边界条件，频率范围为 `[10, 800]` ，求解方法为 `Full` ，因为求解的是给定初始位移问题，无法利用模态叠加法进行求解。边界条件为，添加 `Remote Displacement` 约束上下右三条边 x, y 方向固定，其它自由；添加 `Displacement` 给定左边初始 z 方向位移 10 mm，x, y 方向 Free。

![扫频设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505190020.png)

最后，右击 `Model` 选择 `Symmetry`，这里只需要一个方向的阵列即可，因此按如下设置即可。

![对称设置](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505190033.png)

对其进行网格划分之后，就可以看到完整模型结构了。

![网格](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505190052.png)

求解。观察 200 Hz 和 300 Hz 下的变形情况。

200 Hz：

![200Hz](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505190105.gif)

300 Hz：

![300Hz](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505190118.gif)

本次仿真到此也就结束了，结果还是挺理想的。如有不对的地方，还请指出，讨论并改正，谢谢！
