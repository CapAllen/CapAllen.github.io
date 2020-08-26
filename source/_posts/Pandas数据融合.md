---
title: 每周导学-第六周导学-Pandas数据融合
categories:
  - DAND-VIP
tags:
  - lesson guide
  - pandas
abbrlink: f58a0bac
date: 2018-08-21 12:30:09
---

> It's not about where your starting point is, but your end goal and the journey that will get you there.

在P2阶段的末尾，课程中涉及到了一个新的pandas函数——merge，有些同学对merge的用法存在疑惑，尤其是查了一下**Pandas如何做数据合并**之后，发现除了merge之外，还有concat、还有join，还有append，更是一脸懵了，那这期导学呢，我们就来一起梳理一下pandas数据合并的知识点，同学们不要慌，除了代码之外，还嵌入了DataFrame的图片方便大家理解。当然，看完之后还是要自己多练习才行！加油！

# merge

## 官方文档

### 文档链接

[pandas.DataFrame.merge](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.merge.html)

### 常用参数解读

- right：合并的另一个DataFrame

- how：数据融合的方式，包括：left、right、outer和inner，默认是inner

  > merge的连接方式与SQL对比

  > | 合并方式 | SQL JOIN           | 描述                    |
  > | -------- | ------------------ | ----------------------- |
  > | `left`   | `LEFT OUTER JOIN`  | 只用左表的key去匹配链接 |
  > | `right`  | `RIGHT OUTER JOIN` | 只用右表的key去匹配链接 |
  > | `outer`  | `FULL OUTER JOIN`  | 两表中key的并集         |
  > | `inner`  | `INNER JOIN`       | 两表中key的交集         |

- on：进行合并的共有列的名字，用到这个参数的时候一定要保证左表和右表的共有列都有相同的列名
- left_on：左表对齐的列，可以是列名，也可以是和dataframe同样长度的arrays。
- right_on：右表对齐的列，可以是列名，也可以是和dataframe同样长度的arrays。
- left_index/ right_index: 布尔值，如果是True的话，以index作为key
- sort：布尔值，会根据dataframe合并的keys按字典顺序(abcd...这种)排序，默认为True，设置为False可以提高运算速率。

## 合并方法

- 一对一 : 联接两个 DataFrame 的key唯一

- 多对一 : 将一个DataFrame的唯一key连接到另一个 DataFrame 中的一个或多个key

    - 多对多 : keys 对 keys

### 实例

- 一对一

  > 默认连接方式为inner，即取交集

```
In [1]: left = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
   ....:                      'A': ['A0', 'A1', 'A2', 'A3'],
   ....:                      'B': ['B0', 'B1', 'B2', 'B3']})
   ....: 

In [2]: right = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
   ....:                       'C': ['C0', 'C1', 'C2', 'C3'],
   ....:                       'D': ['D0', 'D1', 'D2', 'D3']})
   ....: 

In [3]: result = pd.merge(left, right, on='key')
```

![_images/merging_merge_on_key.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_on_key.png) 

- 多对多（默认内链接）

```
In [4]: left = pd.DataFrame({'key1': ['K0', 'K0', 'K1', 'K2'],
   ....:                      'key2': ['K0', 'K1', 'K0', 'K1'],
   ....:                      'A': ['A0', 'A1', 'A2', 'A3'],
   ....:                      'B': ['B0', 'B1', 'B2', 'B3']})
   ....: 

In [5]: right = pd.DataFrame({'key1': ['K0', 'K1', 'K1', 'K2'],
   ....:                       'key2': ['K0', 'K0', 'K0', 'K0'],
   ....:                       'C': ['C0', 'C1', 'C2', 'C3'],
   ....:                       'D': ['D0', 'D1', 'D2', 'D3']})
   ....: 

In [6]: result = pd.merge(left, right, on=['key1', 'key2'])
```

![_images/merging_merge_on_key_multiple.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_on_key_multiple.png) 

- 左链接

```
In [7]: result = pd.merge(left, right, how='left', on=['key1', 'key2'])
```

![_images/merging_merge_on_key_left.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_on_key_left.png) 

- 右链接

```
In [8]: result = pd.merge(left, right, how='right', on=['key1', 'key2'])
```

![_images/merging_merge_on_key_right.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_on_key_right.png) 

- 外链接

```
In [9]: result = pd.merge(left, right, how='outer', on=['key1', 'key2'])
```

![_images/merging_merge_on_key_outer.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_on_key_outer.png) 

- 利用index链接

  这里就要用到`left_index`和`right_index`参数了，实例如下：

  ```
  In [10]: result = pd.merge(left, right, left_index=True, right_index=True, how='outer')
  ```

  ![_images/merging_merge_index_outer.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_index_outer.png) 


### 重复列的处理

当你进行合并时，key有重复的话，会额外输出`行数X重名列数`的数据，不仅结果错误，还会导致电脑运行速度巨慢，所以，合并前一定要检查是否有重复列名，当然你可以直接比对两表的列名，或者选择如下方法：

```
In [10]: left = pd.DataFrame({'A' : [1,2], 'B' : [1, 2]})

In [11]: right = pd.DataFrame({'A' : [4,5,6], 'B': [2, 2, 2]})

In [12]: result = pd.merge(left, right, on='B', how='outer', validate="one_to_one")
...
MergeError: Merge keys are not unique in right dataset; not a one-to-one merge
```

当有重名列的时候，就会报错，那么如果想继续合并的话，只需把`validate`参数改为`"one_to_many"`。

```
In [13]: pd.merge(left, right, on='B', how='outer', validate="one_to_many")
Out[13]: 
   A_x  B  A_y
0    1  1  NaN
1    2  2  4.0
2    2  2  5.0
3    2  2  6.0
```

如果你想在合并后，把两表中重名的列加上后缀的话，可以使用`suffixes`参数

```
In [27]: left = pd.DataFrame({'k': ['K0', 'K1', 'K2'], 'v': [1, 2, 3]})

In [28]: right = pd.DataFrame({'k': ['K0', 'K0', 'K3'], 'v': [4, 5, 6]})

In [29]: result = pd.merge(left, right, on='k', suffixes=['_l', '_r'])
```

![_images/merging_merge_overlapped_suffix.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_overlapped_suffix.png) 

### `indicator`参数

在merge里，还有一个有意思的参数叫`indicator`，可以输入布尔值（默认为False），当你设置为True的时候，会在你合并的表中，增加一列`_merge`，元素则表示了合并后的数据来自左表还是右表或者二者皆有。

看示例：

```
In [14]: df1 = pd.DataFrame({'col1': [0, 1], 'col_left':['a', 'b']})

In [15]: df2 = pd.DataFrame({'col1': [1, 2, 2],'col_right':[2, 2, 2]})

In [16]: pd.merge(df1, df2, on='col1', how='outer', indicator=True)
Out[16]: 
   col1 col_left  col_right      _merge
0     0        a        NaN   left_only
1     1        b        2.0        both
2     2      NaN        2.0  right_only
3     2      NaN        2.0  right_only
```

或者也可以输入一个字符串作为列名：

```
In [17]: pd.merge(df1, df2, on='col1', how='outer', indicator='indicator_column')
Out[17]: 
   col1 col_left  col_right indicator_column
0     0        a        NaN        left_only
1     1        b        2.0             both
2     2      NaN        2.0       right_only
3     2      NaN        2.0       right_only
```

# join

## 官方文档

### 文档链接

[pandas.DataFrame.join](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.join.html)

### 常用参数解读

- **other** : DataFrame , 或者是有列名的Series
- **on** : 也就是merge的key，可以输入key或者keys，默认为**index**
- **how** : left、right、outer、inner，默认为**left**

## 合并方法

join是一种快速合并方法，所需参数较少，默认以index作为链接键。

### 实例

- 最常用

```
In [18]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
   ....:                      'B': ['B0', 'B1', 'B2']},
   ....:                      index=['K0', 'K1', 'K2'])
   ....: 

In [19]: right = pd.DataFrame({'C': ['C0', 'C2', 'C3'],
   ....:                       'D': ['D0', 'D2', 'D3']},
   ....:                       index=['K0', 'K2', 'K3'])
   ....: 

In [20]: result = left.join(right)
```

![_images/merging_join.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_join.png) 

- 外连接

```
In [21]: result = left.join(right, how='outer')
```

![_images/merging_join_outer.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_join_outer.png) 

- 内连接

```
In [22]: result = left.join(right, how='inner')
```

![_images/merging_join_inner.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_join_inner.png) 

- 键为列与index

```
In [23]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
   ....:                      'B': ['B0', 'B1', 'B2', 'B3'],
   ....:                      'key': ['K0', 'K1', 'K0', 'K1']})
   ....: 

In [24]: right = pd.DataFrame({'C': ['C0', 'C1'],
   ....:                       'D': ['D0', 'D1']},
   ....:                       index=['K0', 'K1'])
   ....: 

In [25]: result = left.join(right, on='key')
```

![_images/merging_join_key_columns.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_join_key_columns.png) 

- 键为多列与多index

```
In [26]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
   ....:                      'B': ['B0', 'B1', 'B2', 'B3'],
   ....:                      'key1': ['K0', 'K0', 'K1', 'K2'],
   ....:                      'key2': ['K0', 'K1', 'K0', 'K1']})
   ....: 

In [27]: index = pd.MultiIndex.from_tuples([('K0', 'K0'), ('K1', 'K0'),
   ....:                                   ('K2', 'K0'), ('K2', 'K1')])
   ....: 

In [28]: right = pd.DataFrame({'C': ['C0', 'C1', 'C2', 'C3'],
   ....:                    'D': ['D0', 'D1', 'D2', 'D3']},
   ....:                   index=index)
   ....: 
In [29]: result = left.join(right, on=['key1', 'key2'])
```

![_images/merging_join_multikeys.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_join_multikeys.png) 

- 键为单index与多重index

```
In [30]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
   ....:                      'B': ['B0', 'B1', 'B2']},
   ....:                      index=pd.Index(['K0', 'K1', 'K2'], name='key'))
   ....: 

In [31]: index = pd.MultiIndex.from_tuples([('K0', 'Y0'), ('K1', 'Y1'),
   ....:                                   ('K2', 'Y2'), ('K2', 'Y3')],
   ....:                                    names=['key', 'Y'])
   ....: 

In [32]: right = pd.DataFrame({'C': ['C0', 'C1', 'C2', 'C3'],
   ....:                       'D': ['D0', 'D1', 'D2', 'D3']},
   ....:                       index=index)
   ....: 

In [33]: result = left.join(right, how='inner')
```

![_images/merging_join_multiindex_inner.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_join_multiindex_inner.png) 

> 你也可以用merge来实现：
>
> ```
> In [34]: result = pd.merge(left.reset_index(), right.reset_index(),
>    ....:       on=['key'], how='inner').set_index(['key','Y'])
>    ....: 
> ```
>
> - 键为两个多重index
>
> 这里用join就不能实现了，但是你可以用merge，方法跟上面merge的用法类似
>
> ```
> In [35]: index = pd.MultiIndex.from_tuples([('K0', 'X0'), ('K0', 'X1'),
>    ....:                                    ('K1', 'X2')],
>    ....:                                     names=['key', 'X'])
>    ....: 
> 
> In [36]: left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
>    .....:                      'B': ['B0', 'B1', 'B2']},
>    .....:                       index=index)
>    .....: 
> 
> In [37]: result = pd.merge(left.reset_index(), right.reset_index(),
>    .....:                   on=['key'], how='inner').set_index(['key','X','Y'])
>    .....: 
> ```
>
> ![_images/merging_merge_two_multiindex.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_two_multiindex.png) 

### 重复列的处理

```
In [38]: left = pd.DataFrame({ 'v': [1, 2, 3]},index = ['K0', 'K1', 'K2'])

In [39]: right = pd.DataFrame({ 'v': [4, 5, 6]},index = ['K0', 'K0', 'K3'])

In [40]: result = left.join(right, lsuffix='_l', rsuffix='_r')
```

![_images/merging_merge_overlapped_multi_suffix.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_merge_overlapped_multi_suffix.png) 

### 多表的连接

```
In [41]: right2 = pd.DataFrame({'v': [7, 8, 9]}, index=['K1', 'K1', 'K2'])

In [42]: result = left.join([right, right2])
```

![_images/merging_join_multi_df.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_join_multi_df.png) 

# concat

## 官方文档

### 文档链接

[pandas.concat](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html#pandas.concat)

### 常用参数解读

- objs:：series或者dataframe  
- axis： 需要合并连接的轴，0表示行，1表示列   
- join：inner或者outer，默认为outer 

## 合并方法

### 实例

- 纵向连接相同列名的表

```
In [1]: df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
   ...:                     'B': ['B0', 'B1', 'B2', 'B3'],
   ...:                     'C': ['C0', 'C1', 'C2', 'C3'],
   ...:                     'D': ['D0', 'D1', 'D2', 'D3']},
   ...:                     index=[0, 1, 2, 3])
   ...: 

In [2]: df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
   ...:                     'B': ['B4', 'B5', 'B6', 'B7'],
   ...:                     'C': ['C4', 'C5', 'C6', 'C7'],
   ...:                     'D': ['D4', 'D5', 'D6', 'D7']},
   ...:                      index=[4, 5, 6, 7])
   ...: 

In [3]: df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
   ...:                     'B': ['B8', 'B9', 'B10', 'B11'],
   ...:                     'C': ['C8', 'C9', 'C10', 'C11'],
   ...:                     'D': ['D8', 'D9', 'D10', 'D11']},
   ...:                     index=[8, 9, 10, 11])
   ...: 

In [4]: frames = [df1, df2, df3]

In [5]: result = pd.concat(frames)
```

![_images/merging_concat_basic.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_basic.png) 

你也可以设置`key`参数，来区分数据来自哪张表

```
In [6]: result = pd.concat(frames, keys=['x', 'y', 'z'])
```

![_images/merging_concat_keys.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_keys.png) 

- 行拼接（默认外连接）

```
In [8]: df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
   ...:                     'D': ['D2', 'D3', 'D6', 'D7'],
   ...:                     'F': ['F2', 'F3', 'F6', 'F7']},
   ...:                    index=[2, 3, 6, 7])
   ...: 

In [9]: result = pd.concat([df1, df4], axis=1, sort=False)
```

![_images/merging_concat_axis1.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_axis1.png) 

- 行拼接（设置内连接）

```
In [10]: result = pd.concat([df1, df4], axis=1, join='inner')
```

![_images/merging_concat_axis1_inner.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_axis1_inner.png) 

- 行拼接（只筛选出某一表的index）

```
In [11]: result = pd.concat([df1, df4], axis=1, join_axes=[df1.index])
```

![_images/merging_concat_axis1_join_axes.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_axis1_join_axes.png) 

- 无视index的合并

  当数据表的index没有实际意义时，可以使用`ignore_index`参数，来重置合并后的数据集index

```
In [15]: result = pd.concat([df1, df4], ignore_index=True)
```

![_images/merging_concat_ignore_index.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_ignore_index.png) 

- 合并DataFrame与Series

```
In [17]: s1 = pd.Series(['X0', 'X1', 'X2', 'X3'], name='X')

In [18]: result = pd.concat([df1, s1], axis=1)
```

![_images/merging_concat_mixed_ndim.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_mixed_ndim.png) 

当然你也可以使用`ignore_index`参数，将列名重置

```
In [21]: result = pd.concat([df1, s1], axis=1, ignore_index=True)
```

![_images/merging_concat_series_ignore_index.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_concat_series_ignore_index.png) 

- 合并若干Series

```
In [22]: s3 = pd.Series([0, 1, 2, 3], name='foo')

In [23]: s4 = pd.Series([0, 1, 2, 3])

In [24]: s5 = pd.Series([0, 1, 4, 5])

In [25]: pd.concat([s3, s4, s5], axis=1)
Out[25]: 
   foo  0  1
0    0  0  0
1    1  1  1
2    2  2  4
3    3  3  5
```

你可以添加`keys`参数，来给合并后的数据集添加列名

```
In [26]: pd.concat([s3, s4, s5], axis=1, keys=['red','blue','yellow'])
Out[26]: 
   red  blue  yellow
0    0     0       0
1    1     1       1
2    2     2       4
3    3     3       5
```

# append

append其实就是concat的一个便捷函数，实现列的拼接

```
In [12]: result = df1.append(df2)
```

![_images/merging_append1.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_append1.png) 

- 当两表列名不一致时

```
In [13]: result = df1.append(df4)
```

![_images/merging_append2.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_append2.png) 

> 你可以添加参数`ignore_index = True`，来重置合并数据集的index
>
> ```
> In [16]: result = df1.append(df4, ignore_index=True)
> ```
>
> ![_images/merging_append_ignore_index.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_append_ignore_index.png) 

- 合并多个表

```
In [14]: result = df1.append([df2, df3])
```

![_images/merging_append3.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_append3.png) 

- DataFrame添加Series

```
In [34]: s2 = pd.Series(['X0', 'X1', 'X2', 'X3'], index=['A', 'B', 'C', 'D'])

In [35]: result = df1.append(s2, ignore_index=True)
```

![_images/merging_append_series_as_row.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_append_series_as_row.png) 

# 其他

## 合并两表中的值

你以后也许会在工作中遇到这种情况，你的团队中针对同一情况统计了两份数据，但是这两份数据都有一些缺失值，那么如何让他们相互“弥补”这些缺失值呢？

试试[pandas.DataFrame.combine_first](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.combine_first.html#pandas.DataFrame.combine_first)

```
In [43]: df1 = pd.DataFrame([[np.nan, 3., 5.], [-4.6, np.nan, np.nan],
   .....:                    [np.nan, 7., np.nan]])
   .....: 

In [44]: df2 = pd.DataFrame([[-42.6, np.nan, -8.2], [-5., 1.6, 4]],
   .....:                    index=[1, 2])
   .....: 
In [45]: result = df1.combine_first(df2)
```

![_images/merging_combine_first.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_combine_first.png) 

但是，此方法只是利用右表中的值替换了左表中的NaN。

另外，你还可以试试[pandas.DataFrame.update](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.update.html#pandas.DataFrame.update)，这次替换的不仅仅是NaN值，还有其他所有能在右表中找到的相同位置的值。

```
In [46]: df1.update(df2)
```

![_images/merging_update.png](https://pandas.pydata.org/pandas-docs/stable/_images/merging_update.png) 



# 总结

- merge主要用来根据key对数据集进行合并；
- join相当于简化版的merge，key默认为index，所以你如果想按照index对数据集合并时，join就是你的首选；
- concat其实是concatenate的简称，翻译过来是“连接”，所以，如果你想对数据集进行横向连接（axis = 1）或者纵向链接时，记得想起来用concat；
- append相当于是简化版的concat，用得频次比concat要多得多。

希望能通过这篇导学，给大家扫除Pandas数据融合的疑惑，当然还是那句话，**Learning by doing**，光看不练可不行哦，打开你的Jupyter Notebook，试试看！