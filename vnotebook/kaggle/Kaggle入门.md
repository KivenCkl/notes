# Kaggle 入门

> 参考链接：Kaggle 入门指南 - Instante的文章 - 知乎
> https://zhuanlan.zhihu.com/p/25742261

## Data Exploration

在这一步要做的基本就是 `EDA(Exploratory Data Analysis)`，也就是对数据进行探索性的分析，从而为之后的处理和建模提供必要的结论。

通常使用 `pandas` 来载入数据，并做一些简单的可视化来理解数据。

### Visualization

通常来说 `matplotlib` 和 `seaborn` 提供的绘图功能就可以满足需求了。

比较常用的图表有：

- 查看目标变量的分布。当分布不平衡时，根据评分标准和具体模型的使用不同，可能会严重影响性能。
- 对于 Numerical Variable，可以用 `Box Plot` 来直观地查看它的分布。
- 对于坐标类数据，可以用 `Scatter Plot` 来查看它们的分布趋势和是否有离群点的存在。
- 对于分类问题，将数据根据 Label 的不同着不同的颜色绘制出来，这对 Feature 的构造很有帮助。
- 绘制变量之间两两的分布和相关度图表。

## Data Preprocessing

通常，在构造 Feature 之前，需要对比赛提供的数据集进行一些处理。通常的步骤有：

- 有时数据会分散在几个不同的文件中，需要 Join 起来
- 处理 Missing Data
- 处理 Outlier
- 必要时转换某些 Categorical Variable 的表示方式
- 有些 Float 变量可能是从未知的 Int 变量转换得到的，这个过程中发生精度损失会在数据中产生不必要的 Noise。这对 Model 可能会产生很负面的影响，需要设法去除或者减弱 Noise。

这一部分的处理策略多半依赖于在前一步中探索数据集所得到的结论以及创建的可视化图表。

### Outlier

![outlier](https://gitee.com/KivenC/chaos/raw/master/upload_images/20200808153314.jpg)

这是经过 Scaling 的坐标数据，可以发现右上角存在一些离群点，去除以后分布比较正常。

### Dummy Variables

对于 Categorical Variable，常用的做法是 `One-Hot encoding`。即对这一变量创建一组新的伪变量，对应其所有可能的取值。这些变量中只有这条数据对应的取值为 1，其他都为 0。

![dummy variables](https://gitee.com/KivenC/chaos/raw/master/upload_images/20200808153342.jpg)

要注意，当变量可能取值的范围很大（比如一共有成百上千类）时，这种简单的方法就不太适用了。

## Feature Engineering

Kaggle 比赛是“Feature 为主，调参和 Ensemble 为辅”。Feature Engineering 能做到什么程度，取决于对数据领域的了解程度。比如在数据包含大量文本的比赛中，常用的 NLP 特征就是必须的。

一般来说，当一个变量从直觉上来说对所要完成的目标有帮助，就可以将其作为 Feature。至于它是否有效，最简单的方式就是通过图表来直观感受。

![feature engineering](https://gitee.com/KivenC/chaos/raw/master/upload_images/20200808153407.jpg)

### Feature Selection

总的来说，我们应该生成尽量多的 Feature，相信 Model 能够挑出最有用的 Feature。但有时先做一遍 Feature Selection 也能带来一些好处：

- Feature 越少，训练越快
- 有些 Feature 之间可能存在线性关系，影响 Model 的性能
- 挑选出最重要的 Feature，可以将它们之间进行各种运算和操作的结果作为新的 Feature，可能带来意外的提高

Feature Selection 最实用的方法也就是看 Random Forest 训练完以后得到的 Feature Importance 了。其他有一些更复杂的算法在理论上更加 Robust，但是缺乏实用高效的实现。从原理上来讲，增加 Random Forest 中树的数量可以在一定程度上加强其对于 Noisy Data 的 Robustness。

看 Feature Importance 对于某些数据经过脱敏处理的比赛尤其重要。这可以避免浪费大把时间在琢磨一个不重要的变量的意义上。

### Feature Encoding

这里用一个例子来说明在一些情况下 Raw Feature 可能需要经过一些转换才能起到比较好的效果。

假设有一个 Categorical Variable 一共有几万个取值可能，那么创建 Dummy Variables 的方法就不可行了。这时一个比较好的方法是根据 Feature Importance 或是这些取值本身在数据中的出现频率，为最重要（比如说前 95% 的 Importance）那些值（有很大可能只有几个或是十几个）创建 Dummy Variables，而所有其他取值都归到一个“其他”类里面。

## Model Selection

准备好 Feature 以后，就可以开始选用一些常见的模型进行训练了。Kaggle 上最常用的模型基本都是基于树的模型：

- Gradient Boosting
- Random Forest
- Extra Randomized Trees

以下模型往往在性能上稍逊一筹，但是很适合作为 Ensemble 的 Base Model。

- SVM
- Linear Regression
- Logistic Regression
- Neural Networks

以上这些模型基本都可以通过 `sklearn` 来使用。

另外，`Gradient Boosting` 本身优秀的性能加上 `Xgboost` 高效的实现，使得其在 Kaggle 上广为使用。在实战中，往往会以 Xgboost 为主来建立模型并且验证 Feature 的有效性。

### Model Training

在训练时，我们主要希望通过调整参数来得到一个性能不错的模型。一个模型往往有很多参数，但其中比较重要的一般不会太多。比如对 **sklearn** 的 RandomForestClassifier 来说，比较重要的就是随机森林中树的数量 n_estimators 以及在训练每棵树时最多选择的特征数量 max_features。所以**我们需要对自己使用的模型有足够的了解，知道每个参数对性能的影响是怎样的**。

通常我们会通过一个叫做 `Grid Search` 的过程来确定一组最佳的参数。其实这个过程说白了就是根据给定的参数候选对所有的组合进行暴力搜索。

```python
param_grid = {'n_estimators': [300, 500], 'max_features': [10, 12, 14]}
model = grid_search.GridSearchCV(estimator=rfr, param_grid=param_grid, n_jobs=1, cv=10, verbose=20, scoring=RMSE)
model.fit(X_train, y_train)
```

Random Forest 一般在 max_features 设为 Feature 数量的平方根附近得到最佳结果。

这里重点讲一下 Xgboost 的调参。通常认为对它性能影响较大的参数有：

- eta：每次迭代完成后更新权重时的步长。越小训练越慢
- num_round：总共迭代的次数
- subsample：训练每棵树时用来训练的数据占全部的比例。用于防止 Overfitting
- colsample_bytree：训练每棵树时用来训练的特征的比例，类似 RandomForestClassifier 的 max_features
- max_depth：每棵树的最大深度限制。与 Random Forest 不同，Gradient Boosting 如果不对深度加以限制，最终是会 Overfit 的
- early_stopping_rounds：用于控制在 Out Of Sample 的验证集上连续多少个迭代的分数都没有提高后就提前终止训练。用于防止 Overfitting

一般的调参步骤是：

1. 将训练数据的一部分划出来作为验证集
2. 先将 eta 设得比较高（比如 0.1），num_round 设为 300~500
3. 用 Grid Search 对其他参数进行搜索
4. 逐步将 eta 降低，找到最佳值
5. 以验证集为 watchlist，用找到的最佳参数组合重新在训练集上训练。注意观察算法的输出，看每次迭代后在验证集上分数的变化情况，从而得到最佳的 early_stopping_rounds

```python
X_dtrain, X_deval, y_dtrain, y_deval = cross_validation.train_test_split(X_train, y_train, random_state=1026, test_size=0.3)
dtrain = xgb.DMatrix(X_dtrain, y_dtrain)
deval = xgb.DMatrix(X_deval, y_deval)
watchlist = [(deval, 'eval')]
params = {
    'booster': 'gbtree',
    'objective': 'reg:linear',
    'subsample': 0.8,
    'colsample_bytree': 0.85,
    'eta': 0.05,
    'max_depth': 7,
    'seed': 2016,
    'silent': 0,
    'eval_metric': 'rmse'
}
clf = xgb.train(params, dtrain, 500, watchlist, early_stopping_rounds=50)
pred = clf.predict(xgb.DMatrix(df_test))
```

注意一点，所有具有随机性的 Model 一般都会有一个 seed 或是 random_state 参数用于控制随机种子。得到一个好的 Model 后，在记录参数时务必也记录下这个值，从而能够在之后重现 Model。

### Cross Validation

Cross Validation 是非常重要的一个环节。它让你知道 Model 有没有 Overfit，是不是真的能够 Generalize 到测试集上。当你改进了 Feature 或是 Model 得到了一个更高的 CV 结果，提交之后得到的 LB 结果却变差了，一般认为这时应该相信 CV 的结果。

在数据的分布比较随机均衡的情况下，5-Fold CV 一般就足够了。如果不放心，可以提到 10-Fold。但是 Fold 越多训练也就会越慢，需要根据实际情况进行取舍。

## Ensemble Generation

Ensemble Learning 是指将多个不同的 Base Model 组合成一个 Ensemble Model 的方法。它可以同时降低最终模型的 Bias 和 Variance，从而在提高分数的同时又降低 Overfitting 的风险。

常见的 Ensemble 方法有这么几种：

- Bagging：使用训练数据的不同随机子集来训练每个 Base Model，最后进行每个 Base Model 权重相同的 Vote。也即 Random Forest 的原理
- Boosting：迭代地训练 Base Model，每次根据上一个迭代中预测错误的情况修改训练样本的权重。也即 Gradient Boosting 的原理。比 Bagging 效果好，但更容易 Overfit
- Blending：用不相交的数据训练不同的 Base Model，将它们的输出取（加权）平均。实现简单，但对训练数据利用少了
- Stacking

从理论上讲，Ensemble 要成功，有两个要素：

- Base Model 之间的相关性要尽可能的小。这就是为什么非 Tree-based Model 往往表现不是最好但还是要将它们包括在 Ensemble 里面的原因。Ensemble 的 Diversity 越大，最终 Model 的 Bias 就越低。
- Base Model 之间的性能表现不能差距太大。这其实是一个 Trade-off，在实际中很有可能表现相近的 Model 只有寥寥几个而且它们之间相关性还不低。但是实践表明即使在这种情况下 Ensemble 还是能大幅提高成绩。

### Stacking

相比 Blending，Stacking 能更好地利用训练数据。以 5-Fold Stacking 为例，它的基本原理如图：

![stacking](https://gitee.com/KivenC/chaos/raw/master/upload_images/20200808153440.jpg)

整个过程很像 Cross Validation。首先将训练数据分为 5 份，接下来一共 5 个迭代，每次迭代时，将 4 份数据作为 Training Set 对每个 Base Model 进行训练，然后在剩下一份 Hold-out Set 上进行预测。同时也要将其在测试数据上的预测保存下来。这样，每个 Base Model 在每次迭代时会对训练数据的其中 1 份做出预测，对测试数据的全部做出预测。5 个迭代都完成以后我们就获得了一个 #训练数据行数 x #Base Model 数量 的矩阵，这个矩阵接下来就作为第二层的 Model 的训练数据。当第二层的 Model 训练完以后，将之前保存的 Base Model 对测试数据的预测（因为每个 Base Model 被训练了 5 次，对测试数据的全体做了 5 次预测，所以对这 5 次求一个平均值，从而得到一个形状与第二层训练数据相同的矩阵）拿出来让它进行预测，就得到最后的输出。

```python
class Ensemble(object):
    def __init__(self, n_folds, stacker, base_models):
        self.n_folds = n_folds
        self.stacker = stacker
        self.base_models = base_models
    def fit_predict(self, X, y, T):
        X = np.array(X)
        y = np.array(y)
        T = np.array(T)
        folds = list(KFold(len(y), n_folds=self.n_folds, shuffle=True, random_state=2016))
        S_train = np.zeros((X.shape[0], len(self.base_models)))
        S_test = np.zeros((T.shape[0], len(self.base_models)))
        for i, clf in enumerate(self.base_models):
            S_test_i = np.zeros((T.shape[0], len(folds)))
            for j, (train_idx, test_idx) in enumerate(folds):
                X_train = X[train_idx]
                y_train = y[train_idx]
                X_holdout = X[test_idx]
                # y_holdout = y[test_idx]
                clf.fit(X_train, y_train)
                y_pred = clf.predict(X_holdout)[:]
                S_train[test_idx, i] = y_pred
                S_test_i[:, j] = clf.predict(T)[:]
            S_test[:, i] = S_test_i.mean(1)
        self.stacker.fit(S_train, y)
        y_pred = self.stacker.predict(S_test)[:]
        return y_pred
```

## Pipeline

Kaggle 比赛的 Workflow 还是比较复杂的。尤其是 Model Selection 和 Ensemble。理想情况下，我们需要搭建一个高自动化的 Pipeline，它可以做到：

- 模块化 Feature Transform，只需写很少的代码就能将新的 Feature 更新到训练集中
- 自动化 Grid Search，只要预先设定好使用的 Model 和参数的候选，就能自动搜索并记录最佳的 Model
- 自动化 Ensemble Generation，每隔一段时间将现有最好的 K 个 Model 拿来做 Ensemble
