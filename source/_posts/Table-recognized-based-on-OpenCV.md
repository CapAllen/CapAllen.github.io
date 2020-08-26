---
title: 基于OpenCV的表格识别
categories:
  - 工作
tags:
  - Python
  - OpenCV
keywords:
- ['OpenCV','表格识别','Python']
abbrlink: '10258898'
date: 2019-05-31 09:33:08
---

## 概述

上一篇博客中，我们把图片中的水印去除掉，并且加深了字体的颜色，之后我对图片的大小进行了统一，甚至我还专门给他们都加上了[参照字段](https://ai.baidu.com/iocr/#/helpcenter)，分别尝试了[百度AIP](https://ai.baidu.com/)的表格识别服务和[Face++](https://www.faceplusplus.com.cn/)的自定义模板文字识别服务，可能是因为图片的分辨率较低，而且文字较密集的缘故，最终得到的结果都不尽如人意，错误率非常高，所以准备尝试下先将图片按照行列进行分割，之后再逐个去识别的方法，结果却出乎我的意料。

<!--more-->

## 识别表格

要想对图片中的表格进行精准切分，就必须要先对表格进行识别，然后再根据行列相交点的位置进行切分。

### 读取中文路径的图片

```python
def cv_imread(filepath):
    cv_img = cv2.imdecode(np.fromfile(filepath,dtype=np.uint8),-1)
    return cv_img
```

### 将图片转为黑底白字

> 因为后续的`膨胀`是对高亮部分（白色）进行处理，所以就需要把我们感兴趣的部分——表格线转为白色

```python
#转为二值图，矩阵中只有0和255
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
#似曾相识？和上一篇中去水印的函数一样，只不过这里的Thresh方法选择的是THRESH_BINARY_INV，与THRESH_BINARY效果刚好相反，200以上转为0（黑色）,200以下转为255（白色）
img_thresh = cv2.threshold(img_gray, 200, 255, cv2.THRESH_BINARY_INV)[1]
```

### 查看效果

```python
cv2.imshow("Image",img_thresh)
cv2.waitKey(0)
```

![VlsaSe.png](https://s2.ax1x.com/2019/05/31/VlsaSe.png)

### 识别

```python
rows,cols=img_thresh.shape
#可以把这个变量理解为精度，数值越大，识别的就越精细(下面会有示例图)
precision = 20
#识别横线
#膨胀和腐蚀用的卷积核，值为1，shape为cols//precision列，1行
kernel  = cv2.getStructuringElement(cv2.MORPH_RECT,(cols//precision,1))
#腐蚀
eroded = cv2.erode(img_thresh,kernel,iterations = 1)
#膨胀
row_lines = cv2.dilate(eroded,kernel,iterations = 1)
#识别竖线
kernel = cv2.getStructuringElement(cv2.MORPH_RECT,(1,rows//precision))
eroded = cv2.erode(img_thresh,kernel,iterations = 1)
col_lines = cv2.dilate(eroded,kernel,iterations = 1)
```

识别的关键就在于`腐蚀`与`膨胀`函数。**腐蚀**即为让卷积核在二值化的图片上滑动并进行卷积计算，只有在卷积核范围内图片中的所有值均为255时才会得到255的结果，其余情况均得到0；而**膨胀**刚好相反，在卷积核范围内只要有一个值为255，那么就会得到255的结果。

所以，在我们进行表格中的**行识别**时，就需要选择**尽可能长（很多列）和矮（一行）**这种形状的卷积核，才能契合长长的**横线**，然后把比较短的文字腐蚀掉。看下面的示例：

> 示例图片的shape为(369,1000)

**`precision=20`时**，卷积核的shape为(50,1)，识别得到的结果：

![VlfBIs.png](https://s2.ax1x.com/2019/05/31/VlfBIs.png)

这时候已经把所有行都识别出来了，因为不会有任何一个字的长度可以超过50个像素，那么现在我们把卷积核的shape调整到(5,1)再来看下效果。

**`precision=200`时**，识别得到的结果：

![VlfzFA.png](https://s2.ax1x.com/2019/05/31/VlfzFA.png)

可以发现，很多字中的`横`都没有被腐蚀掉。

### 筛选交点位置坐标

我们筛选出那些在行和列都等于255的点，就可以得到所有交点了。

```python
intersections = cv2.bitwise_and(row_lines,col_lines)
```

![VlhI0g.png](https://s2.ax1x.com/2019/05/31/VlhI0g.png)

```python
#筛选出白点的索引（坐标）
ys,xs = np.where(bitwiseAnd>0)
#为了避免在一个交点位置存在多个连续像素为255的值，所以我们需要按照距离对这些点进行筛选
x_list,y_list=[],[]

#先排序
xs=np.sort(xs)
#筛选出距离大于20像素的点
for i in range(len(xs)-1):
    if(xs[i+1]-xs[i]>20):
        x_list.append(xs[i])
#添加上最后一个位置的点
x_list.append(xs[i])

#y_list的处理方式相同
```

这样我们就把所有交点的坐标都筛选出来了。

## 截取图片并识别

按照交点坐标截取单元格后，再转换为jpg或者是png格式的二进制编码，利用[百度aip的通用文字识别API](https://ai.baidu.com/tech/ocr/general)进行识别，获取结果。

```python
#按照交点坐标截取单元格
i = 1
#加减int的原因是将表格线隐藏
ROI = img_thresh[y_list[i]+3:y_list[i+1]-3,x_list[0]+3:x_list[1]-3] 
#必须要先用 jpg或者png转码，再转化为二进制，要不然百度aip识别不了
_,encoded_img = cv2.imencode('.jpg',ROI)
img_bytes = encoded_img.tobytes()
result = client.basicGeneral(img_bytes)
result['words_result'][0]['words']
```

![Vl5pKf.png](https://s2.ax1x.com/2019/05/31/Vl5pKf.png)

最后再利用Pandas将得到的结果以此储存到DataFrame中，导出Excel，收工！

![VlIapn.png](https://s2.ax1x.com/2019/05/31/VlIapn.png)

## 总结

- 识别方法很笨，虽然正确率较高，但是识别速度较慢；
- 依赖百度文字识别服务。

下一步尝试：

- Google开源OCR[tesseract-ocr](https://github.com/tesseract-ocr)
- 基于[yolo3](https://github.com/pjreddie/darknet.git) 与[crnn](https://github.com/meijieru/crnn.pytorch.git)  实现中文自然场景文字检测及识别的[chineseocr](https://github.com/chineseocr/chineseocr)
- 基于tensorflow、keras/pytorch的OCR中文文字识别[CHINESE-OCR](https://github.com/xiaofengShi/CHINESE-OCR)

## 参考

- [*Image Thresholding*](https://docs.opencv.org/trunk/d7/d4d/tutorial_py_thresholding.html)
- [OpenCV学习笔记-腐蚀和膨胀](https://blog.csdn.net/qq_36387683/article/details/80479793)



<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" style="float:left" /></a>

本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。

