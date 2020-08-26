---
title: scrapy--1
img: /medias/blog/20190423/scrapylogo.png
categories:
  - Python
tags:
  - 爬虫
date: 2019-04-23 22:18
---

# 入门案例

    学习目标
    创建一个Scrapy项目
    定义提取的结构化数据(Item)
    编写爬取网站的 Spider 并提取出结构化数据(Item)
    编写 Item Pipelines 来存储提取到的Item(即结构化数据)

## 一. 新建项目(scrapy startproject)

在开始爬取之前，必须创建一个新的Scrapy项目。进入自定义的项目目录中，运行下列命令：  

```bash
scrapy startproject mySpider
```

其中,mySpider 为项目名称，可以看到将会创建一个 mySpider 文件夹，目录结构大致如下：

下面来简单介绍一下各个主要文件的作用：

```bash
mySpider/
    scrapy.cfg
    mySpider/
        __init__.py
        items.py
        pipelines.py
        settings.py
        spiders/
            __init__.py
            ...
```

这些文件分别是:  
> scrapy.cfg: 项目的配置文件。  
> mySpider/: 项目的Python模块，将会从这里引用代码。  
> mySpider/items.py: 项目的目标文件。  
> mySpider/pipelines.py: 项目的管道文件。  
> mySpider/settings.py: 项目的设置文件。  
> mySpider/spiders/: 存储爬虫代码目录。  

## 二、明确目标(mySpider/items.py)

我们打算抓取 [http://www.itcast.cn/channel/teacher.shtml ](http://www.itcast.cn/channel/teacher.shtml )网站里的所有讲师的姓名、职称和个人信息。

打开 mySpider 目录下的 items.py。

Item 定义结构化数据字段，用来保存爬取到的数据，有点像 Python 中的 dict，但是提供了一些额外的保护减少错误。

可以通过创建一个 scrapy.Item 类， 并且定义类型为 scrapy.Field 的类属性来定义一个 Item（可以理解成类似于 ORM 的映射关系）。

接下来，创建一个 ItcastItem 类，和构建 item 模型（model）。

```python
import scrapy

class ItcastItem(scrapy.Item):
   name = scrapy.Field()
   title = scrapy.Field()
   info = scrapy.Field()

```

## 三、制作爬虫 (spiders/itcastSpider.py)

爬虫功能要分两步：

### 1. 爬数据

在当前目录下输入命令，将在mySpider/spider目录下创建一个名为itcast的爬虫，并指定爬取域的范围：

```bash
scrapy genspider itcast "itcast.cn"
```

打开 mySpider/spider目录里的 itcast.py，默认增加了下列代码:

```python
import scrapy

class ItcastSpider(scrapy.Spider):
    name = "itcast"
    allowed_domains = ["itcast.cn"]
    start_urls = [
        'http://www.itcast.cn/',
    ]

    def parse(self, response):
        pass
```

其实也可以由我们自行创建itcast.py并编写上面的代码，只不过使用命令可以免去编写固定代码的麻烦

要建立一个Spider， 你必须用scrapy.Spider类创建一个子类，并确定了三个强制的属性 和 一个方法。

name = "" ：这个爬虫的识别名称，必须是唯一的，在不同的爬虫必须定义不同的名字。

allow_domains = [] 是搜索的域名范围，也就是爬虫的约束区域，规定爬虫只爬取这个域名下的网页，不存在的URL会被忽略。

start_urls = () ：爬取的URL元祖/列表。爬虫从这里开始抓取数据，所以，第一次下载的数据将会从这些urls开始。其他子URL将会从这些起始URL中继承性生成。

parse(self, response) ：解析的方法，每个初始URL完成下载后将被调用，调用的时候传入从每一个URL传回的Response对象来作为唯一参数，主要作用如下：

负责解析返回的网页数据(response.body)，提取结构化数据(生成item)
生成需要下一页的URL请求。
将start_urls的值修改为需要爬取的第一个url

> start_urls = ("http://www.itcast.cn/channel/teacher.shtml",)

修改parse()方法

```python
def parse(self, response):
    filename = "teacher.html"
    open(filename, 'w').write(response.body)
```

然后运行一下看看，在mySpider目录下执行：

```bash
scrapy crawl itcast
```
