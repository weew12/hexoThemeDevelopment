---
title: scrapy 学习(1)
img: /medias/blog/20190425/th.jpg
categories:
  - python
tags:
  - 爬虫
date: 2019-04-25 20:39
---

# 创建项目: 

在指定目录下打开命令行：
```bash
    scrapy startproject 项目名称
```

scrapy 会在打开的目录下创建一个scrapy项目:
![](/medias/blog/20190425/1365896104.png)
![](/medias/blog/20190425/1021292635.png)

# 项目结构如下：

![](/medias/blog/20190425/2745446237.png)

在spider文件夹下创建一个爬虫文件 stackoverflowspider.py:
![](/medias/blog/20190425/689951459.png)

# 编写代码:  

```python
import scrapy
      
class StackOverflowSpider(scrapy.Spider):
  name = 'stackoverflow'
  start_urls = ['http://stackoverflow.com/questions?sort=votes']

  def parse(self, response):
    for href in response.css('.question-summary h3 a::attr(href)'):
      full_url = response.urljoin(href.extract())
      yield scrapy.Request(full_url, callback=self.parse_question)

  def parse_question(self, response):
    yield {
      # 提取返回结果中的标题列表的内容
      'title': response.css('#question-header > h1 > a::text').extract()[0],

      'votes': response.css(
        '#question > div.post-layout > div.votecell.post-layout--left > div > div::text'
      ).extract()[0],

      'body': response.css(
        '#question > div.post-layout > div.postcell.post-layout--right > div.post-text'
      ).extract()[0],

      'tags': response.css(
        '#question > div.post-layout > div.postcell.post-layout--right'
        ' > div.post-taglist.grid.gs4.gsy.fd-column > div::text').extract(),

      'link': response.url,
    }
```

命令行运行：

```bash
     scrapy runspider StackOverflowSpider.py -o test.json
```

得到输出json数据文件：
![](/medias/blog/20190425/1439914177.png)

内容如下：
![](/medias/blog/20190425/2174043839.png)