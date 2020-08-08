# 预测性维护相关资料

## 引言

对预测性维护相关资料进行整理，便于查阅。

<!--more-->

## 预测性维护工作流

[使用数字孪生体进行预测性维护](https://ww2.mathworks.cn/company/newsletters/articles/predictive-maintenance-using-a-digital-twin.html)

![预测性维护工作流](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505203712.jpg)

结合`Simulink`仿真和预测性维护工具箱(`Predictive Maintenance Toobbox`)

- 构建数字孪生体
- 获取数据
- 数据预处理
- 创建预测模型
- 部署算法

预测性维护主要包括两个方向：故障诊断和剩余寿命(RUL)预测。

[Data Ensembles for Condition Monitoring and Predictive Maintenance](https://ww2.mathworks.cn/help/predmaint/ug/data-ensembles-for-condition-monitoring-and-predictive-maintenance.html)

所需数据主要包括三类：

- 系统正常运行
- 系统在故障状态下运行
- 系统运行的寿命记录（从正常到故障的数据）

[小迈步之人工智能（二）：大数据跌进人工智能的坑，预测性维护如何实现和落地](https://ww2.mathworks.cn/videos/predictive-maintenance-with-digital-twins-1585581198614.html?elqsid=1587568085803&potential_use=Education)

主要内容包括：

- 利用数字孪生仿真目标对象，并产生仿真数据
- 利用数据进行预处理和特征提取
- 利用机器学习技术构建设备的故障诊断和剩余寿命预测模型
- 利用 App Designer 生成用户界面

## 故障诊断工作流相关

[Predictive Maintenance in Hydraulic Pump](https://ww2.mathworks.cn/matlabcentral/fileexchange/65605-predictive-maintenance-in-hydraulic-pump)

[Multi-Class Fault Detection Using Simulated Data](https://ww2.mathworks.cn/help/predmaint/ug/multi-class-fault-detection-using-simulated-data.html)

[Analyze and Select Features for Pump Diagnostics](https://ww2.mathworks.cn/help/predmaint/ug/analyze-and-select-features-for-pump-diagnostics.html)

- 导入带标签的数据
- 对数据进行预处理
- 对数据进行探索和特征提取
- 选择合适的机器学习分类模型
  - 决策树
  - SVM
  - 朴素贝叶斯分类器
  - 最近邻分类器
  - 集成分类器
    - 提升树
    - 装袋树
    - ...
  - ...

## 寿命预测工作流相关

[Wind Turbine High-Speed Bearing Prognosis](https://ww2.mathworks.cn/help/predmaint/ug/wind-turbine-high-speed-bearing-prognosis.html)

### Data Import

数据导入主要通过 `fileEnsembleDatastore` 函数来完成。

### Data Exploration

对初始数据进行初步的探索，包括一些基础特征的变化等等。主要通过各类统计图的观察来初步分析。

### Feature Extraction

基于前一步数据探索后的结果对需要、有价值的特征进行提取，主要包括以下特征：

- 时域(time-domain)
  - `Mean`
  - `Std`
  - `Skewness`
  - `Kurtosis`
  - `Peak2Peak`
  - `RMS`
  - `CrestFactor`
  - `ShapeFactor`
  - `ImpulseFactor`
  - `MarginFactor`
  - `Energy`
  - ...
- 频域
  - 谱峰(spectral kurtosis)
    - `SKMean`
    - `SKStd`
    - `SKSkewness`
    - `SKKurtosis`
    - ...

关于信号频域分析方法的理解，参见：[信号频域分析方法的理解（频谱、能量谱、功率谱、倒频谱、小波分析） - Mr.括号的文章 - 知乎](https://zhuanlan.zhihu.com/p/34989414)

### Feature Postprocessing

提取出的特征通常包含噪声，因此需要进行平滑处理。

这里采用的是移动均值平滑(moving mean smothing)，对应的 matlab 函数为 `movmean` 。该方法会引入信号的时间延迟，可以通过在 RUL 预测中选择适当的阈值来减轻延迟影响。

对于时间序列的平滑处理主要包括两种方法：移动平均法(Moving average, MA)和指数平滑法(Exponential Smoothing, ES)，具体可参考：

- [Moving average and exponential smoothing models](https://people.duke.edu/~rnau/411avg.htm)
- [移动平均法（Moving average，MA）指数平滑法（Exponential Smoothing，ES）](https://blog.csdn.net/tz_zs/article/details/78341306)
- [Introduction to Time Series Analysis](https://www.itl.nist.gov/div898/handbook/pmc/section4/pmc4.htm)

### Training Data

创建训练集。通常利用现有数据的部分数据作为训练集。

### Feature Importance Ranking

对数据特征的重要性进行排序。这里选择的是单调性(monotonicity)来度量这些特征的重要性。对应的 matlab 函数为 `monotonicity`。

### Dimension Reduction and Feature Fusion

利用主成分分析(PCA)对特征进行降维，在进行 PCA 之前，需要对特征数据进行归一化处理。

可通过观察主成分之间的关系，选择合适的健康指标。

### Fit Exponenential Degradation Models for Remaining Useful Life (RUL) Estimation

观察健康指标的变化趋势选择合适的衰退模型，这里使用的是指数衰退模型(Exponenential Degradation Models)。利用 `predictRUL` 和 `update` 方法对 RUL 进行预测，并实时更新参数分布。

![real-time RUL estimation](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505203746.gif)

[RUL Estimation Using RUL Estimator Models](https://ww2.mathworks.cn/help/predmaint/ug/rul-estimation-using-rul-estimator-models.html)

使用 RUL 预测模型的工作流：

1. 选择合适的 RUL 预测模型
2. 利用历史数据训练该模型，使用 `fit` 命令
3. 利用同种类型组件的历史数据预测待估计 RUL 的测试组件，使用 `predictRUL` 命令，同时还可以利用该测试组件已产生的数据对预测模型进行更新，使用 `update` 命令

RUL 预测模型家族：

![rul_families](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505203801.png)

### Performance Analysis

$\alpha-\lambda$ 图用于预后性能分析(prognosis performance analysis)，其中 $\alpha$ 界限设为 $20\%$。估计的 RUL 介于真实 RUL 的 $\alpha$ 界限之间的概率被计算作为模型的性能度量：

$$
{\rm Pr}(r^*(t)-\alpha r^*(t)<r^*(t)+\alpha r^*(t)|\Theta(t))
$$

其中 $r(t)$ 在 $t$ 时刻估计的 RUL，$r^*(t)$ 表示在 $t$ 时刻真实的 RUL，$\Theta(t)$ 是 $t$ 时刻估计的模型参数。

![alpha-lambda plot](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505203817.png)

![alpha20 probability](https://gitee.com/kivenc/chaos/raw/master/upload_images/20200505203829.png)

## matlab 相关方法

### 编程建模

[编程建模基础知识](https://ww2.mathworks.cn/help/simulink/ug/approach-modeling-programmatically.html)

- 加载模型
  - `load_system(<modelname>)`
- 创建模型并指定参数设置
  - `new_model(<modelname>)`
  - `set_param(<modelname>, <Variable>, <Value>)`
- 打开模型时通过编程方式加载变量
  - `set_param(<modelname>, 'PreloadFcn', <variable=value>)`
- 以编程方式添加和连接模块
  - `add_block`
  - `add_line`
  - `delete_line`
  - ...
- 通过编程方式命名信号
- 自动排列模型布局
  - `Simulink.BlockDiagram.arrangeSystem`
  - `Simulink.BlockDiagram.routeLine`
- 在多个窗口中打开同一个模型
  - `open_system(<modelname>, 'window')`
- 获取 Simulink 标识符
  - `Simulink.ID.getSID(<model/block>)`
- 以编程方式指定颜色

控制仿真状态：

``` matlab
set_param(<modelname>, 'SimulationCommand', <command>)
```

`<command>` 可以是 `start`、`pause`、`continue`、`stop`。

> 如果使用 `start` 参数启动仿真，则必须使用 `stop` 参数来停止仿真。

`<command>` 还可以是：

- `update`：在仿真运行时动态更新以更改的工作区变量
- `WriteDataLogs`：将所有数据记录变量写入基础工作区

检查仿真的状态：

``` matlab
get_param(<modelname>, 'SimulationStatus')
```

将返回 `stopped`、`initialzing`、`running`、`paused`、`compiled`、`updating`、`terminating` 或 `external`。

### 常用 matlab 特性

对于大数据，防止数据溢出内存，可以使用 `tall` 类型，推迟运算，使用 `gather` 函数进行运算获取结果。具体参见：[Deferred Evaluation of Tall Arrays](https://ww2.mathworks.cn/help/matlab/import_export/deferred-evaluation-of-tall-arrays.html)

`timer`：定时器。

### 生成和使用模拟数据集合

[Generate and Use Simulated Data Ensemble](https://ww2.mathworks.cn/help/predmaint/ug/generate-and-use-simulated-data-ensemble.html)

[Manage System Data](https://ww2.mathworks.cn/help/predmaint/manage-system-data.html)

#### 载入模型

``` matlab
mdl = 'TransmissionCasingSimplified';
open_system(mdl)
```

#### 生成模拟数据集合

``` matlab
toothFaultValues  = -2:0.5:0; % 5 ToothFaultGain values

% 设置多组仿真
for ct = numel(toothFaultValues):-1:1
    tmp = Simulink.SimulationInput(mdl);
    tmp = setVariable(tmp,'ToothFaultGain',toothFaultValues(ct));
    simin(ct) = tmp;
end

mkdir Data
location = fullfile(pwd,'Data');
[status,E] = generateSimulationEnsemble(simin,location);
```

#### 模拟数据集合存储

``` matlab
ensemble = simulationEnsembleDatastore(location)

% ensemble.DataVariables
% ensemble.SelectedVariables
```

#### 读取数据

``` matlab
ensemble.SelectedVariables = ["Vibration";"SimulationInput"];
data1 = read(ensemble)
% data1=1×2 table
%         Vibration               SimulationInput
%     _________________    ______________________________
%
%     {581x1 timetable}    {1x1 Simulink.SimulationInput}

data2 = read(ensemble)
% data2=1×2 table
%         Vibration               SimulationInput
%     _________________    ______________________________
%
%     {594x1 timetable}    {1x1 Simulink.SimulationInput}

% 可以设置一次性读取数据的大小
ensemble.ReadSize = 2;

ensemble.SelectedVariables = ["Vibration";"ToothFault"];
data3 = read(ensemble)

% data3=2×2 table
%         Vibration        ToothFault
%     _________________    __________
%
%     {581x1 timetable}      false
%     {594x1 timetable}      false
```

`read(<ensemble>)` 函数类似于 Python 中的 `next`，模拟数据集合相当于 Pyhon 中的生成器。

可通过 `reset(<ensemble>)` 来将整体数据置于未读状态，从而能够从头读取数据。

#### 追加集合成员数据

``` matlab
reset(ensemble);
sT = false;
while hasdata(ensemble)
    data = read(ensemble);
    SimInputVars = data.SimulationInput{1}.Variables;
    TFGain = SimInputVars.Value;
    sT = (abs(TFGain) < 0.1);
    writeToLastMemberRead(ensemble,'ToothFault',sT);
end
```
