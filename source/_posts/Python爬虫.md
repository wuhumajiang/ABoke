---
title: Python爬虫
date: 2022-10-17 09:56:14
tags: 学习
cover: https://qiansen.oss-cn-hangzhou.aliyuncs.com/Python爬虫.jpg
category: 学习
---

#### requests使用

import  requests

**get请求**
requests.get(url,params=data,headers=headers，proxies=proxies)
定制参数
参数使用params传递
参数无需urlencode编码
不需要请求对象的定制
请求资源路径中？可加可不加

**post请求**
requests.post(url = post_url,headers=headers,data=data，proxies=proxies)

总结：

（1）post请求 是不需要编解码的
（2）post请求的参数是data
（3）不需要请求对象的定制

**get和post区别？**
    1： get请求的参数名字是params post请求的参数的名字是data
    2： 请求资源路径后面可以不加?
    3： 不需要手动编解码
    4： 不需要做请求对象的定制

**请求方法请求服务器响应文件response的属性以及类型**
    response.text : 获取网站源码
    response.encoding ：访问或定制编码方式
    response.url ：获取请求的url
    response.content ：响应的字节类型
    response.status_code ：响应的状态码
    response.headers ：响应的头信息

#### xpath使用(pycharm)

 1.安装lxml库
 pip  install  ‐i  https://pypi.tsinghua.edu.cn/simple  lxml
 2.导入lxml.etree
 from lxml import etree

 3.etree.parse() 解析本地文件
 html_tree = etree.parse(‘XX.html’)
 4.etree.HTML() 服务器响应文件
 html_tree = etree.HTML(response)
 4.html_tree.xpath(xpath路径)

xpath基本语法：
1.路径查询
    //：查找所有子孙节点，不考虑层级关系
    / ：找直接子节点
2.谓词查询
    div是前面的路径
    //div[@id]
    //div[@id="maincontent"]
3.属性查询
    //@class	#查找名叫class的所有属性
    如//title[@lang]：选取所有拥有名为 lang 的属性的 title 元素。
    //title[@lang='eng']：选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。
4.模糊查询
    //div[contains(@id, "he")]
    //div[starts‐with(@id, "he")]
5.内容查询
    //div/h1/text()
6.逻辑运算
    //div[@id="head" and @class="s_down"]
    //title | //price

#### 代理池

代理池
可以写一个代理池proxies_pool=[{}，{}]	键值对中写代理IP
用proxies = random.choice(proxies_pool)进行随机选择

#### 读取的文件(图片、文本)保存

with  open(file_path,  选项参数)  as  f:

​	f.write(文本内容)

选项参数决定了打开文件的模式：只读，写入，追加等。所有可取值见如下的完全列表。这个参数是非强制的，默认文件访问模式为只读(r)。

| 模式 | 描述                                                         |
| :--- | ------------------------------------------------------------ |
| r    | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
| rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。 |
| r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。 |
| w    | 打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |