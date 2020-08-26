---
title: 每周导学-第八周-FBI枪支数据分析
categories:
  - DAND-VIP
tags:
  - lesson guide
  - pandas
abbrlink: dc92c261
date: 2018-09-08 12:30:09
---

> Stop being afraid of what could go wrong and start being positive about what could go right.

Hi，同学们，经过了前两周的学习，我们掌握了数据分析的基本流程、Pandas在数据分析各个流程中的基本应用以及使用matplotlib&Pandas进行可视化的技巧，那么本周我们就真刀实枪地找一套数据集练练手。本周的导学有两期，分别选用了项目三中的两个数据集（**TMDb电影数据和FBI枪支数据**）进行分析，只是分享思路和方法，起一个抛砖引玉的作用，大家选择其他的数据集也可以举一反三，如果有什么棘手的问题随时微信联系我就OK~

本周开始，我们就进入到了**项目三(P3)阶段**，本阶段总共包含**四周**，在这一个月内，我们要对**数据分析入门**进行学习，学习数据分析思维，掌握Python数据分析及可视化方法，并使用所学知识完成**项目三：探索数据集**，尝试着自己完成整个数据分析的流程，得到一些饶有兴趣的结论，你一定会非常有成就感哒！那么以下便是这四周的学习安排：

| 时间      | 学习重点       | 对应课程                     |
| --------- | -------------- | ---------------------------- |
| 第1周     | 数据分析过程-1 | 数据分析过程&案例研究-1      |
| 第2周     | 数据分析过程-2 | 案例研究-1&案例研究-2        |
| **第3周** | **完成项目**   | **项目：探索数据集**         |
| 第4周     | 项目修改与通过 | 修改项目、查缺补漏、休息调整 |

<font size = '3' color = 'red'>**！！看这里！！**：在P3课程里面安排了SQL的高阶课程，但是因为在项目三中并不会涉及到SQL知识，所以为了保证大家学习的连贯性，在完成前两周的课程之后，就开始项目。至于**！！SQL的高阶知识，大家可以放在课程通关后进行选修！！**； </font>

本阶段可能是个挑战，请一定要**保持自信**，请一定要坚持**学习和总结**，如果遇到任何**课程问题**请参照如下顺序进行解决：

- 先自行查找问题答案（注意提取关键词），参考：谷歌/百度搜索、[菜鸟教程](http://www.runoob.com/python3/python3-tutorial.html)、[CSDN](https://www.csdn.net/)、[stack**overflow**](https://stackoverflow.com/)、[**Python for Data Analysis, 2nd Edition** ](https://github.com/CapAllen/DAND_VIP_Class/blob/master/%E6%8B%93%E5%B1%95%E5%8F%82%E8%80%83/Python%20for%20Data%20Analysis%2C%202nd%20Edition.pdf)、[Python Cookbook](http://python3-cookbook.readthedocs.io/zh_CN/latest/)
- 若问题未解决，请将**问题**及其**所在课程章节**发送至微信群，并@助教即可

饭要一口一口吃，路要一步一步走，大家不要被任务吓到，跟着导学一步一步来，肯定没问题哒！那我们开始吧！

> **注：**本着**按需知情**原则，所涉及的知识点都是在数据分析过程中必须的、常用的，而不是最全面的，想要更丰富，那就需要你们课下再进一步的学习和探索！

# 本周目标

- 在[此处](https://github.com/udacity/new-dand-basic-china/blob/master/%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90%E5%85%A5%E9%97%A8/%E9%A1%B9%E7%9B%AE-%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE%E9%9B%86/%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE%E9%9B%86%20-%20%E5%A4%87%E9%80%89%E6%95%B0%E6%8D%AE%E9%9B%86.md)挑选和你确认过眼神的数据集，并对其进行数据分析和探索，得出你自己的结论，然后进行提交。

# 学习计划

| 时间       | 学习资源            | 学习内容           |
| ---------- | ------------------- | ------------------ |
| 周二       | 微信群 - 每周导学   | 预览每周导学       |
| 周三、周四 | Udacity - Classroom | 项目三             |
| 周五       | 微信/Classin - 1V1  | 课程难点           |
| 周六       | Classin - 优达日    | 本周学习总结、答疑 |
| 周日       | 笔记本              | 总结沉淀           |
| 周一       | 自主学习            | 查漏补缺           |

# 项目准备

## 环境准备

强烈建议大家完成本地环境的搭建，在本地完成此项目。搭建本地环境的方法请参考[Anaconda的安装与配置](https://classroom.udacity.com/nanodegrees/nd002-cn-basic-vip/parts/e566ad37-6119-4448-a6bc-7ade73ef3992)一节，完成后你将获得本项目中会用到的关键软件：**Spyder**和**Jupyter Notebook**。

## 文件准备

- 项目文件：依次进入到**项目：探索数据集** --> **3.实战项目**中，下载项目文件的ipynb格式。

[![Pz6nds.md.png](https://s1.ax1x.com/2018/09/03/Pz6nds.md.png)](https://imgchr.com/i/Pz6nds)

- 数据集选择：在[此处](https://github.com/udacity/new-dand-basic-china/blob/master/%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90%E5%85%A5%E9%97%A8/%E9%A1%B9%E7%9B%AE-%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE%E9%9B%86/%E6%8E%A2%E7%B4%A2%E6%95%B0%E6%8D%AE%E9%9B%86%20-%20%E5%A4%87%E9%80%89%E6%95%B0%E6%8D%AE%E9%9B%86.md)选择你感兴趣的数据集并下载至与**项目文件相同的文件夹**。（若下载失败，请微信联系我索取）

- 本地打开：在项目文件的文件夹下，按住`Shift`键，右击选择`在此处打开命令窗口`，输入`jupyter notebook`，待打开本地页面之后，选择项目文件打开，之后就请开始你的表演。

  ![](https://ws2.sinaimg.cn/large/9150e4e5ly1fg4xzdddjhj20b407u74k.jpg)

## 方法准备

项目文件中已经给大家列好了基本流程，所以请在开始项目之前，一定要**先整体浏览一遍项目文件**，着重看一下：

- 项目流程
- 每一个流程中你需要做哪些工作
- 每一个流程中的提示

要记得，数据分析过程不是一蹴而就的，是螺旋上升接近目标的过程，所以一定要保持耐心，对照着[项目评估准则](https://review.udacity.com/#!/rubrics/306/view)一步步完成。

另外，一开始你的notebook会显得很乱没有章法，那在**提交之前最好能再修改一个整理好的版本**。

# 项目流程（FBI枪支数据）

## 数据集说明

该数据来自联邦调查局 (FBI) 的全国即时犯罪背景调查系统 (NICS)。NICS 用于确定潜在买家是否有资格购买枪支或爆炸物。枪支店可以进入这个系统，以确保每位客户没有犯罪记录或符合资格购买。该数据已经收纳了来自 
census.gov 的州级数据作为补充数据。[NICS 数据](https://d17h27t6h515a5.cloudfront.net/topher/2017/November/5a0a4db8_gun-data/gun-data.xlsx)在一个 xlsx 文件格式的一个表格中，它包含了按照月份(month)、州 (state) 、类型 (type) 统计的武器调查数量 (the number of firearm checks) ;美国的[人口普查数据](https://d17h27t6h515a5.cloudfront.net/topher/2017/November/5a0a554c_u.s.-census-data/u.s.-census-data.csv) (U.S. census data) 储存在一个 csv 文件中。它包含了州级的几个变量，每个州的大多数变量在 2016 年只有一个数据点，但一些变量有一年以上的数据。

## 提出问题

虽说本数据集有两个数据文件，但围绕的关键词只有一个，那就是**枪支**，所以提出问题时，也一定要围绕着该关键词进行提问。问题示例：

- 就本数据集统计而言，哪个州的枪支总量增长最高？该州的增长速率如何？  
- 就本数据集统计而言，全美整体购买枪支总量及各种类的趋势是什么？  
- 2016年哪个州的人均拥有枪支量最高？  
- 结合2016年各州的人口普查数据，是否有哪项数据与人均拥有枪支量线性相关？

## 导入拓展包

一般来说，就导入如下几个拓展包就够了，当然如果后续操作中还需导入其他包的话，再补充即可。

```python
# 用这个框对你计划使用的所有数据包进行设置
#   导入语句。
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
import seaborn as sns

# 务必包含一个‘magic word’，以便将你的视图与 notebook 保持一致。
%matplotlib inline
#关于更多信息，请访问该网页：
#   http://ipython.readthedocs.io/en/stable/interactive/magics.html

```

## 数据整理

### 导入数据

```python
#导入枪支数据
df_gun = pd.read_excel('gun_data.xlsx')
#导入人口普查数据,这里有一点需要注意，若数据集中全部是包含有千位分隔符的数据，可以在read_csv中添加参数"thousands = ','"。
df_census = pd.read_csv('U.S. Census Data.csv')
```

### 评估数据

#### 枪支数据

- 浏览数据

```python
df_gun.head()
```

这里要注意一点，因为数据集的列数较多，jupyter会自动折叠中间的部分列，导致我们预览数据时总是看不完整，这时候你可以尝试下：

```python
#col_numbers设置为不小于你数据集列数的某一值
pd.set_option('max_columns',col_numbers)
```

同理，也可以用'max_rows'设置最大显示行数。

- 列及数据类型

```python
df_gun.info()
```

```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 12485 entries, 0 to 12484
Data columns (total 27 columns):
month                        12485 non-null object
state                        12485 non-null object
permit                       12461 non-null float64
permit_recheck               1100 non-null float64
handgun                      12465 non-null float64
long_gun                     12466 non-null float64
other                        5500 non-null float64
multiple                     12485 non-null int64
admin                        12462 non-null float64
prepawn_handgun              10542 non-null float64
prepawn_long_gun             10540 non-null float64
prepawn_other                5115 non-null float64
redemption_handgun           10545 non-null float64
redemption_long_gun          10544 non-null float64
redemption_other             5115 non-null float64
returned_handgun             2200 non-null float64
returned_long_gun            2145 non-null float64
returned_other               1815 non-null float64
rentals_handgun              990 non-null float64
rentals_long_gun             825 non-null float64
private_sale_handgun         2750 non-null float64
private_sale_long_gun        2750 non-null float64
private_sale_other           2750 non-null float64
return_to_seller_handgun     2475 non-null float64
return_to_seller_long_gun    2750 non-null float64
return_to_seller_other       2255 non-null float64
totals                       12485 non-null int64
dtypes: float64(23), int64(2), object(2)
memory usage: 2.6+ MB
```

纵览了一下，各列的数据类型基本没什么问题，唯一有问题的是`month`列，应该是`datetime`类型。整个数据集就是由时间、州名和各式各样的枪支数据组成的。

- 查看各列的缺失值情况

```python
#这里我们计算的缺失比例
df_gun.isnull().sum()/df_gun.shape[0]
```

```python
month                        0.000000
state                        0.000000
permit                       0.001922
permit_recheck               0.911894
handgun                      0.001602
long_gun                     0.001522
other                        0.559471
multiple                     0.000000
admin                        0.001842
prepawn_handgun              0.155627
prepawn_long_gun             0.155787
prepawn_other                0.590308
redemption_handgun           0.155386
redemption_long_gun          0.155467
redemption_other             0.590308
returned_handgun             0.823789
returned_long_gun            0.828194
returned_other               0.854626
rentals_handgun              0.920705
rentals_long_gun             0.933921
private_sale_handgun         0.779736
private_sale_long_gun        0.779736
private_sale_other           0.779736
return_to_seller_handgun     0.801762
return_to_seller_long_gun    0.779736
return_to_seller_other       0.819383
totals                       0.000000
dtype: float64
```

可见数据集的缺失情况非常严重，这时候需要你直接运行`df_gun`去观察全部数据，这样才能有据猜想数据缺失的原因。简要通览数据后，我们发现近年的数据缺失不大，猜想之前的统计中没有对枪支进行详细分类或者是统计不全导致的，而且就枪支数量而言，缺失数据较少的permit、handgun、long_gun和other等列占绝对优势，所以我们完全可以只筛选最有代表性的这几列进行分析。

> 因为筛选后的数据跟之前相差较大，为了提高后续步骤的效率，可以在此先进行数据清理
>
> ```python
> #因为此次筛选的数据列是间隔的，所以调用了numpy的r_函数生成列表，按照列序号筛选间隔的列。
> df_gun = df_gun.iloc[:,np.r_[0:3,4,5,7,8,26]].dropna()
> ```
>
> 之后，还需对最后一列`totals`进行修正
>
> ```python
> df_gun['totals'] = df_gun['permit']+df_gun['handgun']+df_gun['long_gun']+df_gun['multiple']+df_gun['admin']
> ```

- 查看重复值

```python
#检查是否有数据重复
sum(df_gun.duplicated())
```

```python
0
```

无重复数据。

- 查看state列的唯一值数

```python
df_gun['state'].nunique()
```

```python
55
```

美国一共有50个州，多出来的5个是什么鬼？是不是有缩写的州名导致的重复？

```python
df_gun['state'].value_counts()
```

```python
New Mexico              227
California              227
South Carolina          227
Arkansas                227
Kansas                  227
New Jersey              227
West Virginia           227
Texas                   227
Maryland                227
District of Columbia    227
New Hampshire           227
Alaska                  227
Mississippi             227
Alabama                 227
Vermont                 227
Illinois                227
North Dakota            227
Rhode Island            227
Pennsylvania            227
Oklahoma                227
Kentucky                227
Florida                 227
North Carolina          227
Utah                    227
Idaho                   227
Connecticut             227
Missouri                227
Nebraska                227
Tennessee               227
Massachusetts           227
Nevada                  227
Arizona                 227
Hawaii                  227
Indiana                 227
Oregon                  227
Michigan                227
Minnesota               227
Ohio                    227
New York                227
Wyoming                 227
Iowa                    227
Maine                   227
Guam                    227
Wisconsin               227
South Dakota            227
Puerto Rico             227
Georgia                 227
Washington              227
Delaware                227
Louisiana               226
Virginia                226
Montana                 226
Colorado                226
Mariana Islands         218
Virgin Islands          214
Name: state, dtype: int64
```

结果一目了然，不仅有美国各州，还包括了Mariana Islands 、Virgin Islands 等地，感兴趣可以去搜了一下，都是老美宣誓主权的一些群岛。

#### 人口普查数据

评估的步骤差不多，所以只罗列一些需要注意的点。

```python
df_census.info()
```

```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 85 entries, 0 to 84
Data columns (total 52 columns):
Fact              80 non-null object
Fact Note         28 non-null object
Alabama           65 non-null object
Alaska            65 non-null object
Arizona           65 non-null object
Arkansas          65 non-null object
California        65 non-null object
Colorado          65 non-null object
Connecticut       65 non-null object
Delaware          65 non-null object
Florida           65 non-null object
Georgia           65 non-null object
Hawaii            65 non-null object
Idaho             65 non-null object
Illinois          65 non-null object
Indiana           65 non-null object
Iowa              65 non-null object
Kansas            65 non-null object
Kentucky          65 non-null object
Louisiana         65 non-null object
Maine             65 non-null object
Maryland          65 non-null object
Massachusetts     65 non-null object
Michigan          65 non-null object
Minnesota         65 non-null object
Mississippi       65 non-null object
Missouri          65 non-null object
Montana           65 non-null object
Nebraska          65 non-null object
Nevada            65 non-null object
New Hampshire     65 non-null object
New Jersey        65 non-null object
New Mexico        65 non-null object
New York          65 non-null object
North Carolina    65 non-null object
North Dakota      65 non-null object
Ohio              65 non-null object
Oklahoma          65 non-null object
Oregon            65 non-null object
Pennsylvania      65 non-null object
Rhode Island      65 non-null object
South Carolina    65 non-null object
South Dakota      65 non-null object
Tennessee         65 non-null object
Texas             65 non-null object
Utah              65 non-null object
Vermont           65 non-null object
Virginia          65 non-null object
Washington        65 non-null object
West Virginia     65 non-null object
Wisconsin         65 non-null object
Wyoming           65 non-null object
dtypes: object(52)
memory usage: 34.6+ KB
```

发现一个奇怪的事情，基本上所有列数据都在65行之后没有了，肯定有什么猫腻，可以用tail()或者直接打开文件查看（因为数据量很小），发现后面几行都是对数据统计的一些说明，没有实际意义，直接删除即可。而且在该数据中只有50个州的数据。



### 清理数据

> 清理数据就是对上一步中发现的问题进行清理，并且删除无关列，最终得到的是一个方便你进行分析及可视化的干净数据。
>
> **清理前记得备份数据**

- 删除无关列

  建议使用drop函数

  ```python
  #删除列
  df_census.drop('Fact Note',axis = 1,inplace = True)
  #删除行
  df_census.drop(np.arange(64,85),inplace = True)
  ```

- 数据融合

  想要找出人口普查的数据与枪支数据之间的关系，肯定要把这两个数据进行融合。

  融合前，我们先观察一下人口普查数据中到底有哪些，发现每个州的大部分变量都只在2016年有记录点，所以我们就要分别从人口普查数据和枪支数据中筛选出2016年的数据，再以州作为键（key）进行合并。

  **筛选数据**

  ```python
  #先筛选出人口普查数据中2016年的数据，并将Fact列作为索引
  df_census_2016 = df_census[df_census.Fact.str.contains('2016')].set_index('Fact')
  #转秩
  df_census_2016 = df_census_2016.transpose()
  #筛选2016年的枪支数据,并按照州分组计算全年均值
  df_gun_2016 = df_gun[df_gun['month'].str.contains('2016')].groupby('state').mean()
  ```

  **处理千位分隔符和百分号**

  在人口普查数据中数字中均包含有千位分隔符或者是百分号，这会给我们之后进行分析带来不便，处理掉他们，并且转换为浮点型数据。

  ```python
  #先将有千位分隔符的列转为float类型
  for i in [ 'Population estimates, July 1, 2016,  (V2016)','Housing units,  July 1, 2016,  (V2016)', 'Building permits, 2016']:
      df_census_2016[i] = df_census_2016[i].str.split(',')
      df_census_2016[i] = df_census_2016[i].str.join('')
      df_census_2016[i] = df_census_2016[i].astype(float)
  ```

  百分号的处理方式同上类似。

  **数据融合**

  我们在之前的导学中总结过Pandas做数据融合的方法，就此项目而言，两个数据以索引（Index，也就是各州的州名）为键进行融合，最简单的方法就是用join()函数了。如果你忘了，可以点[这里](http://www.capallen.top/dand-vip/2018/08/21/%E7%AC%AC%E5%85%AD%E5%91%A8-2-%E6%95%B0%E6%8D%AE%E8%9E%8D%E5%90%88/)复习一下。

  ```python
  df_2016 = df_gun_2016.join(df_census_2016)
  ```

  好，至此我们的数据就算是整理完毕了，接下来就是探索性分析及可视化阶段，这里没有什么特别需要注意的地方，大家就随性而为。

## 探索性数据分析

在这个阶段你可以随意做可视化，尽可能多得去探索你想解决的问题。**在探索阶段，作图可以不用很规范，你自己知道是什么意思就可以**，而且可能会有些探索的问题结果意义不大，所以**在提交项目时，要进行择优保留并对其润色修饰**。

- 探索不同年份，美国或者是不同州的枪支数量变化就使用df_gun数据集；
- 探索不同州的人口普查数据与枪支持有量之间的关系就使用我们之后整理的df_2016数据集。

## 得出结论

对你整理好的可视化图像进行总结，结论要有根有据但也不要只是停留在`描述`，可以进行适当的猜测。

# 最后

- 导学只是参考和分享，我是还原了自己在做项目时的思路历程，这并不是提交项目时应有的样子。
- 记得提交前一定要再重新审视一遍，调整代码，注释，修缮可视化及结论。
- 经过P3的历练，相信你能更自信，也更能有成就感，坚持！！
- 加油！！！