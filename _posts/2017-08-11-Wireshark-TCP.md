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

- Tcp三次握手过程图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-7.png" alt="Wireshark-2-7"/></div><br />

### 包含HTTP POST命令的TCP报文段的序号是多少？

- 序号是1。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-8.png" alt="Wireshark-2-8"/></div><br />

### 如果将包含HTTP POST命令的TCP报文段看作是TCP连接上的第一个报文段，那么该TCP连接上的第六个报文段的序号是多少？是何时发送的？该报文段所对应的ACK是何时接收的？

- 5186。是在 Jun 1,2016 17:59:29.145097000。其对应的ACK是在Jun 1,2016 17:59:29.397316000被接受的。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-9.png" alt="Wireshark-2-9"/></div>
<div align='center'>（1）</div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-10.png" alt="Wireshark-2-10"/></div>
<div align='center'>（2）</div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-11.png" alt="Wireshark-2-11"/></div>
<div align='center'>（3）</div><br />

### 前六个TCP报文段的长度各是多少？

- 859、1514、1514、1514、1514、1514。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-12.png" alt="Wireshark-2-12"/></div><br />

### 在整个跟踪过程中，接收端公示的最小的可用缓存空间是多少？限制发送端的传输以后，接收端的缓存是否仍然不够用？

- 241。仍然不够用。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-13.png" alt="Wireshark-2-13"/></div><br />

### 在跟踪文件中是否有重传的报文段？进行判断的依据是什么？ 

- 有重传的报文段。判断依据如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-14.png" alt="Wireshark-2-14"/></div><br />

### TCP连接的throughput (bytes transferred per unit time)是多少？请写出你的计算过程。

- 节选从Jun  1, 2016 17:59:28.889301000到Jun  1, 2016 17:59:29.900686000的分组进行相关统计。其中分组长度有如下数值：
`859：1`<br />
`56：22`<br />
`81：1`<br />
`1514：52`<br />
`354：1`<br />
`66：2`<br />
相加求和再做除法得throughput为5297664 bytes/s。
计算过程及起始、终止分组如下图所示：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-15.png" alt="Wireshark-2-15"/></div>
<div align='center'>（1）</div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-16.png" alt="Wireshark-2-16"/></div>
<div align='center'>（2）</div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2-17.png" alt="Wireshark-2-17"/></div>
<div align='center'>（3）</div><br />









