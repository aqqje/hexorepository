---
title: python爬取全网站所有图片
date: 2018-02-18 15:22:17
tags: 
	- python Craw
categories:
	- python
---
{% note info %}
整体思路：1.获取网站最大page是多少?2.再遍历出所有page的url
3.再分别对每个page页进行套图的url遍历4.找出每个套图的最大page，5.对套图的每一个图片进行保存
{% endnote %}

代码：
```python
# 导入必要的库
import requests
from bs4 import BeautifulSoup
import os
import sys
import lxml
from multiprocessing import Pool, cpu_count
import time

# 设置headers头信息
header = {
	# 判断客户端的请求是Ajax请求
	'X-Requested-With': 'XMLHttpRequest',  
    # 用户代理
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 UBrowser/6.1.2107.204 Safari/537.36',
    # 反盗链
    'Referer': 'http://www.mmjpg.com'
    }

# 保存地址
path = 'D:/mmjpg/'
home_url = 'http://www.mmjpg.com'
home_page = 'http://www.mmjpg.com/home/'

# 爬取网站的最大page/也可以从网站直接得到
def getMax_page(home_url):
    try:
        # 爬取home_url
        html = requests.get(home_url, headers = header, timeout=30)
        # 状态码
        html.raise_for_status()
        # 设置编码格式
        html.encoding = html.apparent_encoding
    except:
        return home_url + " html爬取失败！"
    # 解析html.text /使用 lxml 解析器
    soup = BeautifulSoup(html.text, 'lxml')
    return soup.find('div', 'page').find('a', 'last')['href'].split('/')[-1]

# 获取每套图最大page
def getMax_pic_page(href):
    try:
        html = requests.get(href,headers = header)
        html.raise_for_status()
        html.encoding = html.apparent_encoding
    except:
        return url + " html爬取失败！"
    soup = BeautifulSoup(html.text, 'lxml')
    return int(soup.find('div', class_ = 'page').find_all('a')[-2].text)

# 对网站的每页的套图进行一一爬取
def craw_pic(max_page):
    # 遍历全站page
    for n in range(1, int(max_page) + 1):
        # 如果page是首页url='http://www.mmjpg.com'
        if n == 1:
            url = 'http://www.mmjpg.com'
        # 否则就是 home_page + str(n)
        else:
            url = home_page + str(n)
        try:
            html = requests.get(url, headers = header, timeout = 30)
            html.raise_for_status()
            html.encoding = html.apparent_encoding
        except:
            return url + " html爬取失败！"
        soup = BeautifulSoup(html.text, 'lxml')
        # 获得套图的img标签
        all_img = soup.find('div', class_='pic').find('ul').find_all('span', class_='title')
        # 遍历img标签
        for a in all_img:
            # 获得套图标题
            title = a.a.get_text()
            if(title != ''):
                print('开始爬取：' + title)
                # win 不能创建带？ 的目录
                if(os.path.exists(path + title.strip().replace('?', ''))):
                    print('目录已存在')
                    flag = 1
                else:
                    # 创建文件夹
                    os.makedirs(path + title.strip().replace('?', ''))
                    flag = 0
                # 进入目标文件夹
                os.chdir(path + title.strip().replace('?', ''))
                # 得到套图首图url
                href = a.a['href']
                if(flag == 1 and len(os.listdir(path + title.strip().replace('?', '')))):
                    print('已经保存完毕，跳过')
                    continue
                # 遍历套图的url
                for num in range(1, getMax_pic_page(href) + 1):
                    pic_page = href + '/' + str(num)
                    try:
                        html = requests.get(pic_page, headers = header, timeout = 30)
                        html.raise_for_status()
                        html.encoding = html.apparent_encoding
                    except:
                        return href + ' html爬取失败！'
                    soup = BeautifulSoup(html.text, 'lxml')
                    pic_url = soup.find('div', class_='content').find('a').img['src']
                    print('正在爬取：' + pic_url)
                    # 图片的名称
                    file_name = pic_url.split('/')[-1]
                    imghtml = requests.get(pic_url, headers = header)
                    f = open(file_name, 'wb')
                    # 保存图片
                    f.write(imghtml.content)
                    # 关闭图片
                    f.close()
                print('爬取完成：' + title)
    if n == max_page:
        print('全站爬取完成')

            
            
            
if __name__ == "__main__":
        max_page = getMax_page(home_url)
        craw_pic(max_page)
        '''
        # 多线程
        pool = Pool(processes=cpu_count())
        try:
            print("开始爬取")
            pool.map(craw_pic, max_page)
            print("爬取结束！")
        except Exception as e:
			# 失败后睡眠30s
            time.sleep(30)
            print("开始爬取")
            pool.map(craw_pic, max_page)
            print("爬取结束！")
        '''

```

[参考](http://blog.csdn.net/baidu_35085676/article/details/68958267)
