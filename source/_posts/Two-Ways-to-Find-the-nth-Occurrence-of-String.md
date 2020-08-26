---
title: Python中查找字符串中第n次出现某字符的位置
categories:
  - 工作
tags:
  - Python
keywords:
  - - 字符串
    - 查找字符
abbrlink: 7329
date: 2019-07-23 11:12:49
---

## 概述

在处理字符串时，有时需要从字符串中提取出第n次出现某字符的位置，比如说想在字符串`'abcdabdcsas'`中找到第2次出现`'ab'`的索引，但`Python String`提供的`find`函数只能`Return the lowest index in S where substring sub is found`，所以，自己动手，丰衣足食:joy:

本文分为两部分：

- 解决上述问题的两种方法及运行效率对比
- 延伸：
  - 出现某字符的全部索引
  - 最后一次出现某字符的索引

<!--more-->

## 两种方法

- 常规思路

先用`find`函数查找得到第一次出现的索引，然后将字符串在该索引处做切片，再进行查找，以此类推，直到查询到第n次。

```python
def finding_nemo_1(String,Substr,times): 
    '''
    在String中查找Substr出现第times次的索引。
    '''
    s = time.time()
    nemo = 0
    for i in range(1,times+1):
        nemo = String.find(Substr,nemo) + 1
        if nemo == 0:
            return -1
    return (nemo-1, time.time()-s)
```

- 利用`split`切分和列表

将String按照Substr切分times次，代码如下所示，计算很简单。

```python
def finding_nemo_2(String,Substr,times): 
    s = time.time()
    String_list = String.split(Substr,times)
    nemo = len(String) - len(String_list[-1]) - 1
    return (nemo, time.time()-s)
```

我们对以上两种方法进行了对比，如下图所示，第二种方法的优势随着字符串长度的增长而更加明显。

<iframe src="Method_Compare.html" width="1200px" height= "550px" name="Method_Compare" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>  

第二种方法，巧就巧在将运行效率低的`for`循环替换成了`split`，在对长度为250e6的字符串进行查找时，运算时间缩短了近77%。

## 延伸

很容易将以上两种方法改写成`获取某字符出现的全部索引`函数。

```python
#方法一
def finding_all_nemo_1(String,Substr): 
    start = 0
    end = len(String) - 1
    nemo = 0
    nemo_list = []
    while nemo != -1:
        nemo = String.find(Substr,start,end)
        nemo_list.append(nemo)
        start = nemo + 1       
    nemo_list.remove(-1)
    
    return nemo_list

#方法二
def finding_all_nemo_2(String,Substr):
    count = 0
    nemo_list = []
    String_list = String.split(Substr)

    for i in range(len(String_list)-1):
        element = String_list[i]
        count += len(element)
        nemo = i + count
        nemo_list.append(nemo)
    return nemo_list
```

那获取某字符串最后一次出现的位置，有没有更便捷的方法呢？

- 注意：find可以很快的找到第一次出现的位置

所以，我们将字符串反转，再用find就可以很容易求得索引了。

```python
last_index = -(String[::-1].find(Substr) + 1)
```

如果你也有其他的方法，欢迎留言，一起讨论~



<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" style="float:left" /></a>

本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。