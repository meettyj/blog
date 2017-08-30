---
layout: post
title:   Python Notes - 1
category: jekyll
description: Some notes for learning python.
---

<br />

# 查看python安装目录

`import sys
print(sys.path)`<br /><br />

# python 2 to 3

1. 打开windows的cmd，进入Python安装的根目录\Python27\Tools\Scripts。（2to3.py在此目录下）
2. python 2to3.py -w 要转换的文件.py<br />
【比如（python 2to3.py -w e:\pycode\perceptron.py）】<br />
生成bak文件是备份文件，可直接改后缀变成相应的python文件即可。

<br />

# Exception
<div align='center'>
<img src="{{site.baseurl}}/assets/img/pythonNotes/1.png" alt="pythonNotes-1"/></div><br />

# lambda()
<div align='center'>
<img src="{{site.baseurl}}/assets/img/pythonNotes/2.png" alt="pythonNotes-2"/></div><br />

# Reduce()
<div align='center'>
<img src="{{site.baseurl}}/assets/img/pythonNotes/3.png" alt="pythonNotes-3"/></div><br />

# range()

range()可以生成一个整数序列，再通过list()函数可以转换为list。比如range(5)生成的序列是从0开始小于5的整数。
x ** y：表示x的y次方




