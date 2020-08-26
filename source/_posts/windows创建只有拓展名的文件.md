---
title: Windows下创建只有拓展名的文件
categories:
  - DOS
tags:
  - Other Skills
abbrlink: 364b570a
date: 2018-02-01 20:30:09
---

为了能在高高的墙内顺利下载安装Atom的拓展包（[教程](https://www.v2ex.com/amp/t/369533)），需要在本地创建一个`.apmrc`文件,但是直接创建保存的话，会提示

`请输入文件名`，那就只能通过命令符来咯，摸索了下总结如下：

1.在你想创建文件的路径下，`Shift`+右击，`在此处打开命令符`;

2.逐一输入如下行，每输入完一行按一次回车，搞定！

```shell
copy con .fileext
I wanna create a file without ext.
^Z
```

其中，`.fileext`替换为你想创建的拓展名文件；

第二行替换为：你创建文件中的内容，或者可以随便输入什么，然后再用记事本打开后进行修改；

第三行可以通过`Ctrl+Z`或者`F6`进行输入。