---
title: Airbnb in Beijing isn't as Cool Enough as it's Advertised
categories:
  - Udacity
tags:
  - Project
  - Data Analysis Report
abbrlink: e9a23558
date: 2019-05-27 09:33:08
---

![](https://w1nnersclub.com/wp-content/uploads/2018/08/the-great-wall-of-china.jpg)

> Shortly after moving to San Francisco in October 2007, roommates and former schoolmates [Brian Chesky](https://en.wikipedia.org/wiki/Brian_Chesky) and [Joe Gebbia](https://en.wikipedia.org/wiki/Joe_Gebbia) could not afford the rent for their loft apartment. Chesky and Gebbia came up with the idea of putting an [air mattress](https://en.wikipedia.org/wiki/Air_mattress) in their living room and turning it into a bed and breakfast. The goal at first was just "to make a few bucks".
>
> ​                                                                                                                                                         —— Airbnb Wiki

**A‌i‌r‌b‌n‌b‌,‌ ‌I‌n‌c‌.‌**, is an  [online marketplace](https://en.wikipedia.org/wiki/Online_marketplace) and [hospitality service](https://en.wikipedia.org/wiki/Hospitality_service) [brokerage](https://en.wikipedia.org/wiki/Brokerage) company, It has a cool brand story, with hosts sharing their extra living space, guests living for a fee, and allowing guests to experience real local life instead of booking a hotel.  It sounds really cool, but  what is the real situation in Beijing? Let's check it out from two parts:

- Listings in Beijing
- Hosts in Beijing

<!--more-->

> In this blog, the data is provided by [Inside Airbnb](http://insideairbnb.com/), and it contains all informations of 25921 listings in Beijing.

## Listings in Beijing

In this part ,we will focus on **prices** and **numbers** of listings in different times and different locations to learn about Airbnb's development in Beijing.

### How does the price of Airbnb change over time?



<iframe src="Meanprice.html" width="1200px" height= "550px" name="meanprice" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>

- Overall, prices of listings in Beijing are gradually stabilizing.
- In general, prices will rise in May and October each year which corresponds to Labor Day and National Day vocation.

### What's the difference in listings prices in all districts of Beijing?



<iframe src="priceindistrict.html" width="1200px" height= "550px" name="priceindistrict" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>      

- Huairou District has the most expensive middle price and the maximum variance. It's really surprising that the first chair of price is not Dongcheng District where Tiananmen Square located.
- Fangshan District has the most cheap middle price and Tongzhou District has the minimum variance.

### How does the number of Airbnb change over time and districts?



<iframe src="numberofbj.html" width="1200px" height= "550px" name="numberofbj" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>      

- As we can see in the figure above, the number of listings in Beijing has been keeping increase from 2010, and the increasing speed is getting more and more fast. So, Airbnb business in Beijing is seems good.
- The increasing speed will be slow down in the first half of the year, and will speed up in the second half of the year.



<iframe src="number of districts in beijing.html" width="1200px" height= "550px" name="numberofdistricts" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>      

- Chaoyang District(朝阳区) is far ahead in total number and growth rate. As of January 2019, the total number of listings in Chaoyang District accounted for 40% of the total number in Beijing.

## Hosts in Beijing

In this part , we will focus on **host introduction** ,**type** and **service** to look deep inside the hosts of Airbnb in Beijing.

### What's the favorite word of hosts to describe themselves ?

![VesI3t.png](https://s2.ax1x.com/2019/05/28/VesI3t.png)

I made a word cloud with `host_about`. As we can see, most hosts like to put a label "like to make friends" ," welcome friends all over the word" or "like to travel" on themselves. It seems to be a good match for Airbnb's slogan.

Now that the hosts are describing themselves so enthusiastic, what are they actually like?

### Are they all personal hosts?

<iframe src="hostlistingscount.html" width="1200px" height= "550px" name="hostlistingscount" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>   

> In the box plot, I set outliers when it's over 1.5 times of IQR.

Almost a half of hosts have more than 2 listings,  some of them are apartment companies, some of them are sublessors , so we can see that there are some people investing or starting a business in Airbnb with listings. 

<iframe src="hoststype.html" width="1200px" height= "550px" name="hoststype" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>     

> Maybe we should call hosts with 2-5 listings sublessors, and more than 5 listings should be called professional hosts, and the others are called personal host.

There are 56% of hosts are personal hosts, but they only account for 19.5% of listings. Conversely,  14.5% of hosts are professional hosts, but they account for nearly 55% of listings. I have no idea what is the difference between this kind of listings and hotels, except for not safe maybe.

### How's the hosts' service?

<iframe src="serviceofhosts.html" width="1200px" height= "550px" name="serviceofhosts" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>     

> We chose `Proportion of within an hour`,`Host Response Rate` and `Average Score` to judge the pros and cons of service.     

As shown in the plot above, the personal host has the lowest `Proportion of within an hour` and `Host Response Rate`, but has the highest `Average Score`. Meanwhile, the professional hosts has the exact opposite characteristics.

Maybe a personal host need to work or do something else which makes he doesn't have enough time to response guests timely, but he can help guests experience a real local life which makes he get a high review score. 

## Can We Predict the Price?

I made some attempts, but the results were not satisfactory. Although the following characteristics have the greatest impact on the price.

<iframe src="Importances.html" width="1200px" height= "550px" name="Importances" 
scrolling="No"  noresize="noresize" frameborder="0" id="topFrame" allowtransparency=”yes”></iframe>    

Top five importances of features are shown in the plot above, and we can say the more guests a listing can accommodate, the higher price it can be.~~(It's like crap.)~~

## Conclusion

If you have any plans to travel to Beijing recently and you want to book a listing through Airbnb, please note these suggestions:

- Please avoid the May and October;
- Maybe you can choose the listing in Chaoyang District, which has the most number of listings and relatively fair prices;
- If you want to experience a real local life in Beijing ,please choose the personal host.

Finally, I hope you'll be **welcomed home** by Airbnb :). 

