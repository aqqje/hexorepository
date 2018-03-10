---
title: python学习八
date: 2018-01-22 21:04:34
categories:
	- python
tags:
	- python


---

- 中国大学排名定向爬虫

<!-- more -->

---------

## 中国大学排名定向爬虫
```
#CrawUnivRankingA.py
import requests
from bs4 import BeautifulSoup
import bs4
 
def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return ""

def fillUnivList(ulist, html):
    soup = BeautifulSoup(html, "html.parser")
    for tr in soup.find('tbody').children:
        if isinstance(tr, bs4.element.Tag):
            tds = tr('td')
            ulist.append([tds[0].string, tds[1].string, tds[3].string])
 
def printUnivList(ulist, num):
    tplt = "{0:^10}\t{1:{3}^10}\t{2:^10}"
    print(tplt.format("排名","学校名称","总分",chr(12288)))
    for i in range(num):
        u=ulist[i]
        print(tplt.format(u[0],u[1],u[2],chr(12288)))
     
def main():
    uinfo = []
    url = 'http://www.zuihaodaxue.cn/zuihaodaxuepaiming2016.html'
    html = getHTMLText(url)
    fillUnivList(uinfo, html)
    printUnivList(uinfo, 20) # 20 univs
main()
```

结果如下：

```
======== RESTART: F:\python文件\MOCC_python网络爬虫与信息提取\china_unwocity.py ========
    排名    	　　　学校名称　　　	    总分    
    1     	　　　清华大学　　　	   95.9   
    2     	　　　北京大学　　　	   82.6   
    3     	　　　浙江大学　　　	    80    
    4     	　　上海交通大学　　	   78.7   
    5     	　　　复旦大学　　　	   70.9   
    6     	　　　南京大学　　　	   66.1   
    7     	　中国科学技术大学　	   65.5   
    8     	　哈尔滨工业大学　　	   63.5   
    9     	　　华中科技大学　　	   62.9   
    10    	　　　中山大学　　　	   62.1   
    11    	　　　东南大学　　　	   61.4   
    12    	　　　天津大学　　　	   60.8   
    13    	　　　同济大学　　　	   59.8   
    14    	　北京航空航天大学　	   59.6   
    15    	　　　四川大学　　　	   59.4   
    16    	　　　武汉大学　　　	   59.1   
    17    	　　西安交通大学　　	   58.9   
    18    	　　　南开大学　　　	   58.3   
    19    	　　大连理工大学　　	   56.9   
    20    	　　　山东大学　　　	   56.3                    
```

------------------

[中国大学排名定向爬虫](https://www.icourse163.org/learn/BIT-1001870001?tid=1002236011#/learn/content?type=detail&id=1002993608&cid=1003503395)

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
