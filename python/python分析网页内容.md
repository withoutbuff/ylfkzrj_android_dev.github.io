##先随便说点开场白吧，废话比较多，没有耐心看的观众，可以直接跳到下面核心主题。

写完发现格式不对，有空再研究研究吧，讲的比较细，也算是对自己代码的回顾了。本人初学小白，欢迎大神指点。

接下来就是确定获取方式了，这种简单的程序，我觉得最适合用脚本语言。记得之前Python爬虫课程的广告满天飞，就想到还是用Python写个小程序吧。

**所以需要的知识就是：**

1.python语法

2.几个python库的使用方式

3.HTML和Javascript的简单阅读分析

感觉python用户起来挺方便的，类库加载非常简单，只需要用pip工具安装一下，再代码里import就可以直接用了。如果是java，总觉得要在项目里配置下脚本，才能自动下载，感觉这样导入太麻烦。

差点跑题了，这些天又重新开始使用python来写一些爬虫，主要是想完成上次未完成的一个爬虫-抓取网易云音乐的歌单。之前只是写到抓取了歌单id，这次我要把歌单里面的歌曲详细信息抓取到excel表格里。（还是下次吧）

用到的库有selenium，BeautifulSoup，xlwt，还有最重要的chromedriver。分析网页主要是静态网页，如果是动态网页就不太好处理了。

之前看过一些python爬虫的文章，大抵介绍两种方式，一种是模拟HTTP请求的方式，另一种就是使用chromedriver。

模拟HTTP请求的方式，非常复杂，各种各样的请求参数就已经很头疼了。但这还不是最麻烦的，最麻烦的是要模拟浏览器请求，怕一个不对，就被服务器拒绝访问了，我晕，那我还不如直接用浏览器访问好了。

于是chromedriver出场了，直接省去了处理HTTP请求响应两大难题，大大简化了网页抓取的难度。可以让我们把精力集中在网页标签的分析上。





#下面进入核心主题：

##我的目标是想通过控制chromedriver访问每一个页面，获取到招生信息，最后绘制成一个表格。

##1.首先创建一个浏览器句柄对象：

```

dri = webdriver.Chrome(executable_path="chromedriver.exe")

```

##2.接下来跳转到指定的URL

###这里使用的研招网的URL

```

base_url = "https://yz.chsi.com.cn/zsml/queryAction.do"

dri.get(base_url)

```

##3.下面就是要分析页面了

![image](https://upload-images.jianshu.io/upload_images/24860325-061a276206d1169c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###在这里，我们可以看到几个下拉框，这就是我们需要用python代码自动填充的控件，首先右键点击下拉列表，点击检查

![image](https://upload-images.jianshu.io/upload_images/24860325-48b54552c24d4200.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###接下来进入HTML代码

![image](https://upload-images.jianshu.io/upload_images/24860325-3d518481cf628ba0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###可以看到select标签的代码就在我们眼前，把光标放到上面，对应的网页中的标签也会有相应的选中效果响应。

###这样我们就知道这一段HTML代码，对应的是哪一块网页标签了。

###我们可以看到这个标签里，有name,id,class属性，通过这三个任意一个都可以定义到此标签，哪个方便用哪个就好了，我建议使用不会重复的属性。

####我这里使用的是name

```

select=dri.find_element_by_id('yjxkdm')

```

###接下来就是模拟用户点击和选择

####数据也是在HTML标签里的，点击一下就展开了。

![image](https://upload-images.jianshu.io/upload_images/24860325-1f990799da841962.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```

select1.click()

s=Select(select)

s.select_by_value('0812')

```

##下面就比之前稍微难一点了

###我们需要判断出当前页面位置，并且判断是否有下一页，此时就需要BS库，来分析网页的内容了

###首先还是把网页源码引用出来

```

text=dri.page_source

soup=BeautifulSoup(text,"lxml")

```

###其次获取当前页面中最大的页标

```

# 页标

index=int(2)

# 获取到最大页标

indexMax=int(2)

for listas in soup.find_all('a'):

    try:

        inta=int(0)

        inta=int(listas.string)

        if(inta>indexMax):

            indexMax=inta

    except Exception as e:

        continue

print(indexMax)

```

###好吧，时间到了，下次再补吧

以下是全部代码：

```

from selenium import webdriver

from time import sleep

from selenium.webdriver.support.ui import Select#select类，下拉菜单使用

from selenium.webdriver.support.wait import WebDriverWait#等待时间包，在限定时间内查找元素

from selenium.webdriver.common.action_chains import ActionChains#鼠标操作包

from selenium.webdriver.common.keys import Keys#键盘操作包

import time#时间包

import unittest#单元测试包

from bs4 import BeautifulSoup

import copy

import xlwt

import re

base_url = "https://yz.chsi.com.cn/zsml/queryAction.do"

# 创建一个浏览器句柄对象

dri = webdriver.Chrome(executable_path="chromedriver.exe")

# 跳转到指定url

# dri.get(base_url)

try:

        dri.get(base_url)

except Exception as e:

        with open("errorLinks.txt",'w+',encoding="UTF-8") as fFF:

            fFF.write(base_url+"\n")

            fFF.close()

        print(e)

# 选择学科

select=dri.find_element_by_id('yjxkdm')

select.click()

s=Select(select)

s.select_by_value('0812')

sleep(2)

# 选择学习方式

select1=dri.find_element_by_id('xxfs')

select1.click()

s1=Select(select1)

s1.select_by_value('1')

# 非全日制2全日制1

sleep(2)

#确定选择，开始查询

button=dri.find_element_by_name('button').click()

sleep(2)

# #获取源码

text=dri.page_source

# # 解析文件

# soup=BeautifulSoup(open('html.txt',encoding='utf-8'),"lxml")

soup=BeautifulSoup(text,"lxml")

# 页标

index=int(2)

# 获取到最大页标

indexMax=int(2)

for listas in soup.find_all('a'):

    try:

        inta=int(0)

        inta=int(listas.string)

        if(inta>indexMax):

            indexMax=inta

    except Exception as e:

        continue

print(indexMax)

fileFirstFloor='firstFloor.txt'

with open("firstLinks.txt",'w',encoding="UTF-8") as f:

    f.write("")

    f.close()

with open("secondLinks.txt",'w',encoding="UTF-8") as f:

    f.write("")

    f.close()

# 存放链接数据

# 链接

linkList=[]

# 研究生院，自划线院校，博士点

attrList=[]

# # 解析文件,抓第一页

soup=BeautifulSoup(text,"lxml")

# 循环遍历-target属性为  的标签a

for lista in soup.find_all('a',target="_blank"):

    link="https://yz.chsi.com.cn"+lista.get('href')

    # 带有=号的才是需要的链接

    if("=" in link):

        linkList.append(link)

linkList.append("\n")

index=index+1

sleep(2)

for i in range(indexMax):

    # 下一页 工学可用

    pageInput=dri.find_element_by_class_name('page-input')

    pageBtn=dri.find_element_by_class_name('page-btn')

    # 输入页码跳转

    pageInput.send_keys(index)

    pageBtn.click()

    text=text+dri.page_source

    textNow=dri.page_source

    soup=BeautifulSoup(textNow,"lxml")

# 循环遍历-target属性为  的标签a

    for lista in soup.find_all('a',target="_blank"):

        link="https://yz.chsi.com.cn"+lista.get('href')

    # 带有=号的才是需要的链接

        if("=" in link):

            linkList.append(link)

    linkList.append("\n")

    index=index+1

    if(index==int(11)):

        break

    sleep(2)

with open("firstLinks.txt",'w',encoding="UTF-8") as fl:

        for line in linkList:

            fl.write(line+"\n")

        # print(text)

        fl.close()

with open(fileFirstFloor,'w',encoding="UTF-8") as fFF:

        fFF.write(text)

        fFF.close()

# 获取第二级链接

secondLinks=[]

for link in linkList:

    try:

        dri.get(link)

    except Exception as e:

        with open("errorLinks.txt",'w+',encoding="UTF-8") as fFF:

            fFF.write(link+"\n")

            fFF.close()

        print(e)    

    # # 解析文件

    text=dri.page_source

    soup=BeautifulSoup(text,"lxml")

    # 页标

    indexS=int(2)

    # 获取到最大页标

    indexMaxS=int(1)

    for listasS in soup.find_all('a'):

        try:

            intaS=int(0)

            intaS=int(listasS.string)

            if(intaS>indexMaxS):

                indexMaxS=intaS

        except Exception as e:

            continue

    print(indexMaxS)

# 循环遍历-target属性为  的标签a

    for lista in soup.find_all('a',target="_blank"):

        link="https://yz.chsi.com.cn"+lista.get('href')

    # 带有=号的才是需要的链接

        if("=" in link):

            if("https://yz.chsi.com.cn/zsml/kzykskm.jsp?fxid=" in  link):

                    print("假链接")

            else:

                secondLinks.append(link)

    secondLinks.append("\n")

    sleep(2)

    for pageIndexSecond in range(1,indexMaxS):

         # # 解析文件

        text=dri.page_source

        soup=BeautifulSoup(text,"lxml")

        # 循环遍历-target属性为  的标签a

        for lista in soup.find_all('a',target="_blank"):

            link="https://yz.chsi.com.cn"+lista.get('href')

        # 带有=号的才是需要的链接

            if("=" in link):

                if("https://yz.chsi.com.cn/zsml/kzykskm.jsp?fxid=" in  link):

                    print("假链接")

                else:

                    secondLinks.append(link)

        secondLinks.append("\n")

        for buttonNextPageSecond in dri.find_elements_by_tag_name('a'):

            try:

                intaSaa=int(0)

                intaSaa=int(buttonNextPageSecond.get_attribute('textContent'))

                # print(buttonNextPageSecond.get_attribute('textContent'))

                if(intaSaa>pageIndexSecond):

                    buttonNextPageSecond.click()

                    break

            except Exception as e:

                print(e)

                continue

        sleep(2)

with open("secondLinks.txt",'w',encoding="UTF-8") as fFF:

    for line in secondLinks:

        if("https://yz.chsi.com.cn/zsml/kzykskm.jsp?fxid=" in  line):

            continue

        fFF.write(line+"\n")

    fFF.close()

# 专业目录数组

tableTrs=[[0]*54 for i in range(secondLinksLen) ]

count=int(-1)

for link in secondLinks:

    count=count+1

    try:

        dri.get(link)

    except Exception as e:

        with open("errorLinks.txt",'w+',encoding="UTF-8") as fFF:

            fFF.write(link+"\n")

            fFF.close()

        print(e) 

    text=dri.page_source

    soup=BeautifulSoup(text,"lxml")

    for td in soup.select(".zsml-summary"):

        counta=counta+1

        strTdContent=td.string

        if(counta>9):

            break

        elif(counta==8):

            # 留下数字

            tableTrs[count][counta]=re.findall(r"\d+\.?\d*",strTdContent)[0]

        else:

            tableTrs[count][counta]=strTdContent

    counta=counta+1

    for mark in soup.select('.zsml-bz'):

        try:

            if("备" in mark.string):

                continue

            else:

                tableTrs[count][counta]=mark.string

                break

        except Exception as e:

            print(e)

    for tda in soup.select('.sub-msg'):

        counta=counta+1

        if(counta>53):

            break

        tableTrs[count][counta]=tda.string

    if(count%50==0):

        with open("rubsh.txt",'w',encoding="UTF-8") as fFF:

            for line in tableTrs:

                s=""

                for linee in line:

                    s=s+str(linee)

                fFF.write(s+"\n")

            fFF.close()

    sleep(2)

with open("finalContent.txt",'w',encoding="UTF-8") as fFF:

        for line in tableTrs:

            s=""

            for linee in line:

                s=s+str(linee)

            fFF.write(s+"\n")

        fFF.close()

countb=0

workbook = xlwt.Workbook()

sheet = workbook.add_sheet("Sheet Name1")

for b in tableTrs:

    countc=0

    for c in b:

        sheet.write(countb,countc, c) # row, column, value

        countc=countc+1

    countb=countb+1

workbook.save('Excel_test1.xls')

```
