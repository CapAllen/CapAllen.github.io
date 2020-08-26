---
title: Anaconda&Jupyter Notebook配置
categories:
  - DAND
tags:
  - 环境配置
abbrlink: 3fb6ecae
date: 2018-12-02 07:30:09
---

很多同学对Anaconda和Jupyter Notebook的安装与配置有疑惑，这里对课程中的[选修：配置Anaconda和Jupyter Notebook](https://classroom.udacity.com/nanodegrees/nd002-cn-basic-vip/parts/e566ad37-6119-4448-a6bc-7ade73ef3992)作为一个补充。

# Anaconda

## 安装

点击[下载链接](https://www.anaconda.com/download/)，先选择你的操作系统，然后选择`Python 3.7 version`版本下载。

![Fu1fsg.png](https://s1.ax1x.com/2018/12/02/Fu1fsg.png)

安装过程中，记得这里的两个框，都勾选上。

![Fu1Ids.png](https://s1.ax1x.com/2018/12/02/Fu1Ids.png)



---

## 常用软件

安装完Anaconda之后，Windows用户可以通过`开始`-`所有程序`找到`Anaconda`的文件夹，在文件夹下有三个程序是你经常会用到的，分别是：

**Anaconda Prompt**：这里就是Anaconda的控制台，你进行[第三方包的管理](https://classroom.udacity.com/nanodegrees/nd002-cn-basic-vip/parts/e566ad37-6119-4448-a6bc-7ade73ef3992/modules/2424424b-f71a-4c80-a34c-9a1f47e35cd5/lessons/c6a12f2e-63f2-4007-a2c3-dd3e5f06f3cb/concepts/9310a67f-9b23-449e-8949-4312dc718ef9)和[编程环境的管理](https://classroom.udacity.com/nanodegrees/nd002-cn-basic-vip/parts/e566ad37-6119-4448-a6bc-7ade73ef3992/modules/2424424b-f71a-4c80-a34c-9a1f47e35cd5/lessons/c6a12f2e-63f2-4007-a2c3-dd3e5f06f3cb/concepts/14783a82-d656-436b-bc3f-12f7abe03529)都是在这里进行。但是现阶段用不到。

**Jupyter Notebook**：使用非常非常频繁的web文档，在项目三和项目四中都会用到。

**Spyder**：python的IDE(集成开发环境)，在项目二中会用到。

![Fu3BlT.png](https://s1.ax1x.com/2018/12/02/Fu3BlT.png)

# Jupter Notebook

notebook的使用技巧在教室内说得很详细了，这里只补充一点，那就是如何在你工作的文件夹下使用notebook。

### 1. 进入到工作文件夹

### 2.按住`Shift`键，然后右击，选择“在此处打开命令窗口”

![Fu3LtI.png](https://s1.ax1x.com/2018/12/02/Fu3LtI.png)

### 3.输入jupyter notebook，回车

![Fu3j9P.png](https://s1.ax1x.com/2018/12/02/Fu3j9P.png)

### 4.notebook即会在你的默认浏览器中弹出，目录即为该路径下的文件

**注意：**不要关掉弹出notebook的命令窗口，这是将notebook与你电脑内的python链接的纽带。

![Fu89BQ.png](https://s1.ax1x.com/2018/12/02/Fu89BQ.png)

# Spyder

我们将会在项目二中用到Spyder这个软件，这个软件也没什么操作难度，所以课程内没有讲解。

我在这里给大家具体讲一下吧。

## 打开

Windows用户依次点击`开始`-`所有程序`-`Anaconda`-`Spyder`即可。

## 界面

![Fu8rCt.png](https://s1.ax1x.com/2018/12/02/Fu8rCt.png)

如上，我们可以把Spyder的界面划分为5个部分，分别为：

**菜单栏**：就是一些新建、打开、运行、终止等等操作，自行摸索

**选择工作区**：这里可以选择工作区路径

**代码编辑区**：这里就是你写代码的地方

**程序变量/文件路径查看区**：通过选择箭头标注的选项卡，可以显示代码中的变量，或者是当前路径下的文件

**控制台**：这里就是显示你代码运行结果的地方，如果代码运行卡住了，可以通过点该区域右上角的`■`终止运行。

## 使用流程

先确定工作路径，然后新建脚本，开始编写代码、运行（快捷键F5）、调试。

做完项目二，大家就能熟练操作啦，不要担心~



