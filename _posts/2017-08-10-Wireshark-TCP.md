---
layout: post
title:   Wireshark (2) - 对TCP协议的分析
category: jekyll
description: A free and open source packet analyzer used for Transmission Control Protocol analysis.
---

<br />

# 实验流程

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-1.png" alt="Wireshark-2-1"/></div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-2.png" alt="Wireshark-2-2"/></div><br />

# 相关问题及认识

### 向gaia.cs.umass.edu服务器传送文件的客户端主机的IP地址和TCP端口号是多少？

- 客户端主机的ip地址是172.20.33.206。端口号是6970。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-3.png" alt="Wireshark-2-3"/></div><br />

### Gaia.cs.umass.edu服务器的IP地址是多少？对这一连接，它用来发送和接收TCP报文的端口号是多少？

- 服务器的ip地址是128.119.245.12。其用来接收报文的端口号是80。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-4.png" alt="Wireshark-2-4"/></div><br />

### 客户服务器之间用于初始化TCP连接的TCP SYN报文段的序号（sequence number）是多少？在该报文段中，是用什么来标示该报文段是SYN报文段的？

- 序号是0。是通过将Flags置为0x002（即将除SYN以外的对应列均置为0，而SYN相应列置为1来实现的）。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-5.png" alt="Wireshark-2-5"/></div><br />

### 服务器向客户端发送的SYNACK报文段序号是多少？该报文段中，Acknowledgement字段的值是多少？Gaia.cs.umass.edu服务器是如何决定此值的？在该报文段中，是用什么来标示该报文段是SYNACK？

- 报文段序号是 0。Acknowledgement number是1。这是对第一个SYN请求报文的响应报文，而前一个请求报文的Seq是0。故服务器返回ACK=1表示收到Seq是0的请求报文并期待接受Seq是1的客户端报文。通过将标志位置为0x012（即Ack列及Syn列置为1）来标识其为SYNACK报文。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-6.png" alt="Wireshark-2-6"/></div><br />

### 你能从捕获的数据包中分析出tcp三次握手过程吗？

- 

