---
title: 尝试代理爬取音乐资源(不怕被封ip了，美滋滋)
img: /medias/blog/201905/dasdadsadaserqwerq.jpg
categories:
  - python
tags:
  - 爬虫
date: 2019-05-15 22:18
---

## 部分代码(没来得及优化了，有点随意)：

```python
# -*- coding: utf-8 -*-

"""
	File Name：     尝试爬取
	Description :
	Author :        枫weew12
	date：          2019/5/15
	Change Activity:2019/5/15
"""

import requests
import os
import json

pro = [{'http': 'http://58.243.50.184:53281'},
		{'http': 'http://27.46.23.148:8888'},
		{'http': 'http://14.20.235.7:808'},
		{'http': 'http://124.205.155.154:9090'},
		{'http': 'http://124.205.155.150:9090'}]

class GetMusicsId(object):

	def __init__(self):
		self.musics_dic = {}
		self.song_num = 0
		self.header = {
			"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
							" AppleWebKit/537.36 (KHTML, like Gecko)"
							" Chrome/70.0.3538.110 Safari/537.36"
		}

	def open_file(self):
		with open('G:/python 资源/python project/博客站音乐爬取/musics/360062344.json', 'r', encoding='utf-8') as file_in:
			data = json.load(file_in)
			self.musics_dic = data["data"]["songs"]
			self.song_num = int(data["data"]["songListCount"])
			# print(self.musics_dic)

	def get_music(self):
		count = 1
		print('歌单包含歌曲 %d 首' % self.song_num)
		for item in self.musics_dic:

			# 使用哪个代理
			which_pro = pro[count % len(pro)]
			# 如果文件存在就跳过 否则尝试获取
			if os.path.exists('./test_musics2/' + str(item["name"]) + '(' + str(item["singer"]) + ')' + '.mp3'):
				continue
			else:
				print('./test_musics2/' + str(item["name"]) + '(' + str(item["singer"]) + ')' + '.mp3')

				try:
					print(str(item["name"]) + '(' + str(item["singer"]) + ')' + '.mp3' + '下载中......')
					response = requests.get(item["url"], proxies=which_pro)

				except requests.RequestException as err:
					print(err)
					print(str(item["name"]) + '(' + str(item["singer"]) + ')' + '.mp3' + '文件写入失败')
					continue

				try:
					with open('./test_musics2/' + str(item["name"])
								+ '(' + str(item["singer"] + ')' + '.mp3'), 'wb') as file:
						file.write(response.content)
					print(str(item["name"]) + '(' + str(item["singer"]) + ')' + '.mp3' + '文件写入成功')

				except IOError as err:
					print(err)
					print(str(item["name"]) + '(' + str(item["singer"]) + ')' + '.mp3' + '文件写入失败')
					continue

			# 计数加一
			count += 1

	def get_download_list(self):
		num = self.song_num
		detail = os.listdir('./test_musics2')
		print('下载歌曲共 %d 首' % len(detail))
		not_download = num - len(detail)
		print('全部下载成功') if not_download == 0 else print('%d 首下载失败' % not_download)
		# print(detail)

test = GetMusicsId()
test.open_file()
test.get_music()
test.get_download_list()
```

## 爬取结果：
![][1]

  [1]: /medias/blog/201905/3198111353.png

![][2]

  [2]: /medias/blog/201905/1519998907.png