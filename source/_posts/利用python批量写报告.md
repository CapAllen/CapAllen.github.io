---
title: 利用python批量出报告
categories:
  - 工作
tags:
  - Python
  - 文档处理
abbrlink: 9d1af314
date: 2019-04-04 07:30:09
---

# 概述

最近单位需要批量输出报告，好在这些报告的整体模板相同，只有一些跟用户相关的信息需要替换。几千份的重复工作，还是交给Python去处理吧。

<!-- more -->

本文的技术路径为：利用`docxtpl`对word模板文件进行变量替换，然后利用` ` 将word转换为pdf。

本文代码应用环境如下：

```
* Windows 7
* Python 3.7
* docxtpl-0.5.17

```

# Word文档处理

## 安装

- 主要用`docxtpl`库实现。[官方文档](https://docxtpl.readthedocs.io/en/latest/)

- 直接`pip install docxtpl`即可，会自动安装依赖库：`docx`和`jinja2`。

## 模板文档准备

如下所示，将需要进行变量替换的位置用两对大括号括起来，并在其中添加`变量名`，这是后续能通过python识别替换的关键。

![A23KQx.png](https://s2.ax1x.com/2019/04/04/A23KQx.png)

### 表格

> 表格的变量设置比较麻烦，可以查看[官方github](https://github.com/elapouya/python-docx-template/tree/master/tests)，里面有很多示例代码，随用随取。

设置表格变量的时候，有两个关键，一个是`行` ，一个是`列`，其中行用字段`tr`(row)表示，列用字段`tc`(column)表示。

如下表中所示，我们通过构建列表循环 和 调用字典key的方式，对表格中的各个项进行填充。

![A2GUqf.png](https://s2.ax1x.com/2019/04/04/A2GUqf.png)

## 使用示例

```python
from docxtpl import DocxTemplate,InlineImage
from docx.shared import Mm
import jinja2

#读取模板文档
tpl = DocxTemplate('自主招生大数据报告模板.docx')

#替换word中的变量
#字典中的key为变量名，value为要替换的值
context = { 
    'stu_id':123,
    'stu_phone':13612341234,
    'stu_province':'河北',
    'stu_grade':'高二',
    'stu_wenli':'理科',
    'stu_school':'南宫中学',
    'stu_university':'西安交通大学',
    'stu_prize_1':'化学省一',
    'image_1':InlineImage(tpl,'E:/wallpapers/1.jpg',width=Mm(120)),
    'items' : [
        {'university' : '西安交通大学', 'major' : '电气工程,能源动力','year':2018, 'prize' : '数学省一','nums':200 },
        {'university' : '复旦大学', 'major' : '金融学', 'year':2018,'prize' : '数学省三','nums':20 },
        {'university' : '北京理工大学', 'major' : '兵器学', 'year':2018,'prize' : '物理省二','nums':2 },
    ],
}

#输出
tpl.render(context)
tpl.save('output/自主招生报告.docx')
```

# Word转PDF

通过Windows Com组件（win32com），调用Word服务（Word.Application），实现Word到PDF文件的转换。因此，要求该Python程序需要在有Word服务（可能至少要求2007版本）的Windows机器上运行。

> 参考：[批量转换Word文件为PDF文件](https://blog.csdn.net/san1156/article/details/77885995)

```python
import win32com.client

#WdSaveFormat指令，17表示转为PDF
#更多信息可以查看：https://docs.microsoft.com/zh-cn/office/vba/api/word.wdsaveformat
wdFormatPDF = 17
word = win32com.client.Dispatch('Word.Application')
#打开文档，绝对地址
doc = word.Documents.Open('F:\\0 zizhuzhaosheng\\docs\\test.docx')
#输出文档路径，绝对地址
doc.SaveAs('F:\\0 zizhuzhaosheng\\docs\\test.pdf', FileFormat=wdFormatPDF)
#退出
doc.Close()
word.Quit()
```



<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" style="float:left" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。

