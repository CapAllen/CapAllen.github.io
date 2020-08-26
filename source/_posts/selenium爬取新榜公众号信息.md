---
title: selenium爬取新榜公众号信息
categories:
  - 工作
tags:
  - Python
  - selenium
  - 爬虫
abbrlink: 2e1fd6db
date: 2019-04-05 12:29:20
---

这两天需要对公众号的信息进行爬取，原以为只是爬取信息，而不需要怕公众号，会比较简单，所以尝试了：

- littlecodersh的[ItChat](https://github.com/littlecodersh/itchat)

  可以获取公众号的名称、功能描述以及地区，但并没有把公众号的ID以及认证公司等信息集成进去，而且必须要本人关注之后才能处理；

- [搜狗微信](https://weixin.sogou.com/)

  搜狗微信的信息倒是齐全，完全可以满足要求，但是尝试了request+BeatifulSoup还有Selenium之后发现，这个网址真的太小气了，我加了延时，用了代理，依然各种被封，或者是要求输入验证码；

- [新榜](https://www.newrank.cn/)

  只能获取已经收录的公众号信息，好在收录的数量也很多，用request会出现搜索失败，但是用Selenium还是稳的，加了延时，顺利获取了几千条数据。而且，这里还可以查到各个行业时下最热文章、阅读量等等信息。

用selenium爬取代码如下：

```python
from selenium import webdriver
# 引入Keys类包 发起键盘操作
from selenium.webdriver.common.keys import Keys
import time
import csv

def store_data(name,description,company):
    #储存信息
    writer.writerow((name,description,company))    
    
def get_id(name):
    #传入搜索关键词 并进行搜索
    driver.find_element_by_id('txt_account').clear()
    time.sleep(3)
    driver.find_element_by_id('txt_account').send_keys(name)
    driver.find_element_by_id('txt_account').send_keys(Keys.ENTER)
    time.sleep(10)
    
    #获取信息并储存
    description = driver.find_element_by_class_name('wx-description-info').text
    company = driver.find_element_by_class_name('auth-span').text
    store_data(name,description,company)
    time.sleep(30)
    
#打开浏览器
driver = webdriver.Firefox()
#打开网址
driver.get('https://www.newrank.cn/')

#爬取
error_name = []
csvfile = open('公众号信息.csv','a+')
writer = csv.writer(csvfile)
for name in id_list:
    idx = id_list.index(name)
    lenght = len(id_list)   
    
    try:        
        if idx%10 == 0:
            print(f'{idx+1}/{lenght} is working...')
        get_id(name)
    except:
        error_name.append(name)
        
csvfile.close()
```



<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" style="float:left" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。

