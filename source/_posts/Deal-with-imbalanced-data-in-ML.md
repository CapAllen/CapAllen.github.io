---
title: 机器学习中不平衡数据的预处理
categories:
  - 工作
tags:
  - Python
  - Machine Learning
  - Data Preprocessing
keywords:
- ['机器学习','数据预处理','不平衡数据']
abbrlink: 783b84fb
date: 2019-04-19 09:33:08
---

## 概述

最近在处理自主招生的数据，对于某一个确定的高校来说，录取的人数远远小于未录取的人数，换言之，就是录取类的数据量远小于未录取类的数据量，这就是**不平衡数据**，虽然在机器学习中不平衡数据的处理不是难点，但这也是我们不得不去考虑的问题，那在本篇文章中，我们便来一起探讨下有哪些**处理不平衡数据的技巧**。

<!--more-->

## 什么是不平衡数据

不平衡数据，顾名思义，就是指在收集到的数据中各个分类之比并非为`1:1`，在对不平衡数据的研究中，普遍认为不平衡意味着少数类所占比例在10%到20%之间，但实际上，这种现象可能会更严重，比如说：

1. 每年有大约2%的信用卡用户存在欺诈；
2. 某种情况的医学筛查，比如说美国的艾滋病得病率约为0.4%；
3. 磁盘驱动器的故障率每年约1%；
4. 在线广告的转化率约在10<sup>-3</sup>至10<sup>-6</sup>之间；
5. 文章开头提到的高校录取率，比如某985院校的自主招生审核通过率在0.2%左右。

对于这种不平衡的数据来说，如果不进行数据预处理就应用机器学习算法进行训练，那么得到的模型只需要把所有的结果都预测为多数类那边，就能获得很高的准确率（比如说，对所有的学生都判定为审核未通过，那么准确率会达到1-0.2%=99.8%），但是这其实一点用都没有。

平衡数据大概像是这样：

![](https://www.svds.com/wp-content/uploads/2016/08/clean.png)

不平衡数据大概像是这样：

![](https://www.svds.com/wp-content/uploads/2016/08/messy.png)

## 处理不平衡数据

处理不平衡数据的思路比较简单，那就是想办法让数据平衡，我们可以简单得分为以下几类：

- 更改数据集中各分类数据的量，使他们比例匹配——常用方法有采样、数据合成；
- 更改数据集中各分类数据的权重，使他们的量与权重之积匹配——常用方法为加权；
- 不修改数据集，而是在思路上将不平衡数据训练问题转化为一分类问题或者异常检测问题（少数类就像是存在于多数类中的异常值）。

> 更多更详细研究范畴的分类可以查看这篇论文[A Survey of Predictive Modelling under Imbalanced Distributions](#jump)

那接下来，我们分别看一下各个方法在python中的具体实施。

### 采样

采样就是通过减少多数类数量的**下采样（Undersampling）**或增加少数类数量的**上采样（Oversampling）**方式，实现各类别平衡。针对两种采样方式，依赖不同的方法，比如说 随机、模型融合等，就会生成很多解决方案，可以看这篇综述[Learning from Imbalanced Data](http://www.ele.uri.edu/faculty/he/PDFfiles/ImbalancedLearning.pdf)的总结。

![](https://www.svds.com/wp-content/uploads/2016/08/ImbalancedClasses_fig5.jpg)

#### 代码实现

有一个叫做`imbalanced-learn`的库，是专门针对不平衡数据设计的，其中包含很多上采样和下采样的函数，[官方文档](https://imbalanced-learn.readthedocs.io/en/stable/index.html)在这里。

示例：

```python
from collections import Counter
from sklearn.datasets import make_classification
from imblearn.under_sampling import RandomUnderSampler 
from imblearn.over_sampling import RandomOverSampler

#生成不平衡数据
X, y = make_classification(n_classes=2, class_sep=2,
 weights=[0.1, 0.9], n_informative=3, n_redundant=1, flip_y=0,
n_features=20, n_clusters_per_class=1, n_samples=1000, random_state=10)
print('Original dataset shape %s' % Counter(y))
```

`Original dataset shape Counter({1: 900, 0: 100})`

```python
#随机下采样
rus = RandomUnderSampler(random_state=42)
X_res, y_res = rus.fit_resample(X, y)
print('Resampled dataset shape %s' % Counter(y_res))
```

`Resampled dataset shape Counter({0: 100, 1: 100})`

```python
#随机上采样
ros = RandomOverSampler(random_state=42)
X_res, y_res = ros.fit_resample(X, y)
print('Resampled dataset shape %s' % Counter(y_res))
```

`Resampled dataset shape Counter({0: 900, 1: 900})`

### 数据合成

随机上采样会反复出现一些样本，而导致过拟合；随机下采样则会造成一定程度的特征丢失，虽然这种方式比较简单，但现在计算机的计算能力越来越高，可接受的算法复杂度也越来越高，所以我们应该主要考虑模型训练的效果。

数据合成则是利用已有样本生成更多样本，其中常用的包括：

- `SMOTE`，利用KNN生成新数据；

- `SMOTEC`，可以合成分类数据，但数据集中至少要包含一条连续数据；

  > 如果数据集中全是分类数据的话，可以增加一列全为1的intercept列作为连续数据，合成数据之后，再将该列删除即可。

- `BorderlineSMOTE`，与SMOTE的区别是，只为那些周围大部分是大众样本的小众样本生成新样本（因为这些样本往往是边界样本）；

#### 代码实现

```python
from imblearn.over_sampling import SMOTE

#SMOTE合成
sm = SMOTE(random_state=42)
X_res, y_res = sm.fit_resample(X, y)
print('Resampled dataset shape %s' % Counter(y_res))
```

`Resampled dataset shape Counter({0: 900, 1: 900})`

### 更改权重

更改权重就是针对不同类别的数据设置不同的分错代价，即提高少数类分错的代价或降低多数类分错的代价，最终使各类别平衡。

常用的机器学习训练方法中，很多都提供了权重设置参数`class_weight`，可以手动设置该参数，但一般情况下只需要将其设置为`balanced`即可，模型会自动按照如下公式更新权重：

![EpOrss.png](https://s2.ax1x.com/2019/04/19/EpOrss.png)

#### 代码实现

```python
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier

#逻辑回归平衡权重
clf_A = LogisticRegression(random_state=0, class_weight='balanced')
#随机森林平衡权重
clf_B = RandomForestClassifier(random_state=0, n_jobs=-1, class_weight="balanced")
```

### 一分类

这种方法比较适合极不平衡数据，或数据量比较小的数据集。

主要方法为`OneClassSVM`，官方文档[在这里](https://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html)

#### 代码实现

```python
from sklearn import svm
clf = svm.OneClassSVM(nu=0.2, kernel="rbf", gamma=0.1)
```

> 异常检测我没怎么用过，所以就不赘述了，感兴趣可以戳这个链接[Novelty and Outlier Detection](https://scikit-learn.org/stable/modules/outlier_detection.html#outlier-detection)

## 总结

我们可以依据数据量的大小和是否平衡将数据集分为四类，即平衡的大数据集，不平衡的大数据集，平衡的小数据集，不平衡的小数据集。最简单的就是平衡的大数据集，能达到非常高的准确率，最难的就是不平衡的小数据集，除了在平衡上下功夫之外，还需要很多诸如收集数据、特征工程之类的工作，但这并不是本篇文章的重点。

下面根据个人经验谈下针对如上几种数据集，如何选择文中涉及的这些方法：

- 数据量较小的情况考虑用数据合成的方法，依据特征的类型（分类&连续数值）选择合适的方法。
- 数据量还可以，但类别之间数量相差悬殊的时候考虑用一分类或者异常检测的方法。
- 数据量还可以，而且类别之间数量相差不是特别悬殊的情况，考虑用采样或者更改权重的方法。

具体数据量的大小可以参考下图：

![](https://scikit-learn.org/stable/_static/ml_map.png)

## 参考

1. [Learning from Imbalanced Classes](https://www.svds.com/learning-imbalanced-classes/)
2. <span id="jump">[A Survey of Predictive Modelling under Imbalanced Distributions](https://arxiv.org/pdf/1505.01658.pdf)</span>



<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" style="float:left" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。

