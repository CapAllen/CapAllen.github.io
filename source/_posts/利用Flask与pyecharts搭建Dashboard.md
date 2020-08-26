---
title: 利用Flask与pyecharts搭建Dashboard
categories:
  - 工作
tags:
  - Python
  - 可视化
  - Flask
keywords:
  - DashBoard
  - Flask
  - Pyecharts
abbrlink: 29337
date: 2019-08-15 01:12:49
---
## 概述

当处理一些较为灵活的数据时，团队内不同角色的同事会有自己对数据的关注点，所以，这就要求数据分析师不能只出一个“死”报告了事儿，而需要的是一个可以让同事们去探索，去解决自己关注问题的”活“报告——[Dashboard](https://en.wikipedia.org/wiki/Dashboard_(business))。本文就一起来探讨下，利用[Flask](https://dormousehole.readthedocs.io/en/latest/)和[Pyecharts](https://pyecharts.org/#/)搭建局域网内Dashboard的方法，其中Flask用来提供Web应用框架，Pyecharts用来解决交互式可视化的需求。

<!--more-->
## 功能需求

- 可视化图像可交互
- 可以根据用户需求，从数据库中筛选不同的数据进行可视化

## Dashboard框架



<img src="https://s2.ax1x.com/2019/08/15/mE1fkd.png" alt="mE1fkd.png" border="0" width='350'/>



如上图所示，我们依据数据的流向确定了Dashboard的框架，并列出了在不同过程中的所需知识：

- 依据Dashboard中用户的需求（可选），从数据库中提取数据，并对数据进行预处理，以方便后续进行可视化；
- 利用Pyecharts对提取的数据进行可视化；
- 利用Flask将Pyecharts生成的可视化图像嵌入到HTML模板中，并利用Javascript丰富前端的动作、处理事件，利用Ajax实现前后端交互等。

## 两个关键点

- 在Flask框架中使用Pyecharts

这在Pyecharts的官方文档中写的非常详细，可以戳[Flask 模板渲染](https://pyecharts.org/#/zh-cn/web_flask?id=flask-%e6%a8%a1%e6%9d%bf%e6%b8%b2%e6%9f%93)查看示例。

- 如何处理Jinja2 和 JavaScript 语法的冲突

为了快速搭建Dashboard，免去一些前端配色、布局等烦恼，我们一般会挑选一个HTML模板，而Jinja2就是一个模板渲染引擎，它的语法中，使用一对双大括号标记变量，这与JavaScript的语法标记会有冲突，所以，如果你使用Jinja2进行模板渲染的同时使用了JavaScript，就要进行[语法冲突处理](http://greyli.com/jinja2-and-js-template/)。

## 最终效果

![mEB0bR.gif](https://s2.ax1x.com/2019/08/15/mEB0bR.gif)



![mEBg2D.gif](https://s2.ax1x.com/2019/08/15/mEBg2D.gif)

## 致谢&参考

- [Pyecharts](https://pyecharts.org/#/)给了我丰富的交互式可视化选择，详细的文档上手就会，强烈推荐！
- 李辉的[HelloFlask站点](http://helloflask.com/)，轻松入门Flask。
- [Data visualization using D3.js and Flask](https://branetheory.org/2014/12/18/data-visualization-using-d3-js-and-flask/)
- [flask框架中jinja2传递参数和html，js文件接收参数](https://blog.csdn.net/m0_38061194/article/details/78891125)
- [Echarts Demo - 多图联动](https://www.echartsjs.com/examples/editor.html?c=dataset-link)

## 源码

Github链接：[Flask+Pyecharts定制Dashboard](https://github.com/CapAllen/Dashboard)

除以上两点之外，在源码中，还实现了以下功能：

- 多个可视化图像的联动
- 多级菜单联动选择
- 前后端交互
- 表格数据排序与搜索

希望能对有相同需求的你有所帮助，欢迎留言，一起讨论！

<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" style="float:left" /></a>

本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。