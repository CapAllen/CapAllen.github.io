---
title: 一个简单的去水印小工具
categories:
  - 工作
tags:
  - Python
  - OpenCV
keywords:
- ['OpenCV','去水印']
abbrlink: df000511
date: 2019-05-30 09:33:08
---

## 概述

今年阳光高考公示的自主招生名单变成了图片格式，还加了水印，（洪主编说“今年阳光高考变坏了”:joy:），确实是变坏了，无形中给我增添了不少工作:angry:，那么要转为Excel，就要进行文字识别，就需要先把烦人的水印去除掉。

本文分为两部分：

- 基于OpenCV的图片水印去除
- 将python脚本封装为可执行exe程序

<!--more-->

## 去除水印

先放原图：

![VMVnde.png](https://s2.ax1x.com/2019/05/30/VMVnde.png)

观察上图，可以得到如下结论：

- 图片本身就是灰度图，也就是**各个像素的RGB是相等的**；

- 水印的位置并不固定，没办法针对某一位置单独处理；
- 水印颜色统一；
- 水印颜色与文字颜色相差较多，**水印颜色要比文字颜色浅**；
- ~~让我做PS是不可能的，因为还有上百张这样的图片~~。

所以，我们可以从颜色上入手，直接用OpenCV中的[*Image Thresholding*](https://docs.opencv.org/trunk/d7/d4d/tutorial_py_thresholding.html)函数，选择介于文字与水印灰度中间的某一值作为**阈值**，然后将大于该阈值的值替换为255(白色)，小于该阈值的值替换为0(黑色)，这样便可以把图片中所有水印（包括表头的阴影）都去除掉了。

### 颜色拾取

为了选择阈值，我们得先看一下文字和水印的RGB，我是用的[Colors Pro](http://www.pc0359.cn/downinfo/20589.html)（随便下一个颜色识别器或者用Office/PS等都可以）。

![VMevsP.png](https://s2.ax1x.com/2019/05/30/VMevsP.png)

![VMmGsx.png](https://s2.ax1x.com/2019/05/30/VMmGsx.png)

识别结果如上：

文字的灰度介于50~140之间；水印及阴影的灰度介于220-240之间，所以我们可以选择200作为阈值。

### 关键代码

```python
cv2.threshold(img, 200, 255, cv2.THRESH_BINARY)[1]
```

去除水印后的效果：

![VMne1A.png](https://s2.ax1x.com/2019/05/30/VMne1A.png)

去水印的速度非常快（不到一秒一张图），效果也很不错，不仅去除了水印，还把原来偏灰色的文字变成了黑色，更方便下一步的文字识别工作。

但，同事也有这种需求，就索性封装一个可执行文件exe给他备用，本来以为很简单的事儿，折腾了我一整个晚上:joy:，下面分享下我封装python脚本时踩的坑。

## 程序封装

参考stack**overflow**上[a-good-python-to-exe-compiler](https://stackoverflow.com/questions/14165398/a-good-python-to-exe-compiler)的答案，大家都在推荐[Pyinstaller](http://www.pyinstaller.org/)，所以就是它了。

Pyinstaller的使用方法非常简单，只需

```python
#安装
pip install pyinstaller
```

然后在命令行中进入到目标脚本的文件夹下，输入

```python
#-F为附加的参数，效果为只输出单个的exe文件，拷给基友用更方便
pyinstaller -F your-script-name.py
```

就可以在生成的`hist`文件夹下找到封装好的exe文件了:smile:。

然而事情的真相是，各种`Permission Denied`，网上的解决方法是`用管理员模式运行`，对于我这电脑来说**然并卵**。折腾了一晚上，终于通过安装了下面两个库解决了：

- 重新安装pywin32，参考：[如何下载和安装pywin32(亲测有效)](https://blog.csdn.net/qq_34369025/article/details/53687900)
- 下载安装了vc++2018运行库的合集（因为以前大学打游戏，常被这个问题困扰，这次死马当活马医，居然医活了。。。）

## 总结

这个去水印小工具，只能针对类似文中的这种灰度图进行水印处理，如果你想去除彩色图片的水印，请打开PS，然后用Shift+F5盘它！

最后附上这个去水印小工具：[链接](https://share.weiyun.com/5zFHjUF)， 密码：r38ev8

使用方法：

- 自行搜索安装vc++2018运行库合集

- 把exe复制到你要处理图片的文件夹
- 打开，输入阈值灰度
- 回车，处理好的图片会自动储存在`removed`文件夹下

运行效果：

![VMJkrQ.gif](https://s2.ax1x.com/2019/05/30/VMJkrQ.gif)

## 参考

- [*Image Thresholding*](https://docs.opencv.org/trunk/d7/d4d/tutorial_py_thresholding.html)
- [a-good-python-to-exe-compiler](https://stackoverflow.com/questions/14165398/a-good-python-to-exe-compiler)
- [Pyinstaller](http://www.pyinstaller.org/)
- [如何下载和安装pywin32(亲测有效)](https://blog.csdn.net/qq_34369025/article/details/53687900)



<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" style="float:left" /></a>

本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。

