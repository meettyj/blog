---
layout: post
title:   Wireshark (2) - 对IP协议的分析
category: jekyll
description: A free and open source packet analyzer used for Internet Protocol analysis.
---

<br />

# 实验流程一

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-1.png" alt="Wireshark-3-1"/></div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-2.png" alt="Wireshark-3-1"/></div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-3.png" alt="Wireshark-3-1"/></div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-4.png" alt="Wireshark-3-1"/></div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-5.png" alt="Wireshark-3-5"/></div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-6.png" alt="Wireshark-3-6"/></div><br />

# 相关问题及认识

### 你主机的IP地址是什么？

- 172.20.42.158。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-7.png" alt="Wireshark-3-7"/></div><br />

### 在IP数据包头中，上层协议（upper layer）字段的值是什么？

- 是1。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-8.png" alt="Wireshark-3-8"/></div><br />

### IP头有多少字节？该IP数据包的净载为多少字节？并解释你是怎样确定该IP数据包的净载大小的？

- Ip头有20字节，该ip数据包的净载为56-20=36字节，因为该ip数据包的总长为56字节，总长减去头部即为净载。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-9.png" alt="Wireshark-3-9"/></div><br />

### 该IP数据包分片了吗？解释你是如何确定该IP数据包是否进行了分片？

- 该ip数据包未分片，因为其标志位置为0x00且分片偏移量置为0。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-10.png" alt="Wireshark-3-10"/></div><br />


# 实验流程二

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-11.png" alt="Wireshark-3-11"/></div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-12.png" alt="Wireshark-3-12"/></div><br />

### 你主机发出的一系列ICMP消息中IP数据报中哪些字段总是发生改变？ 

- 他们的seq、ttl、Frame、Identification、校验和总是在发生改变。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-13.png" alt="Wireshark-3-13"/></div><br />

### 哪些字段必须保持常量？哪些字段必须改变？为什么？

- 以下字段必须保持常量：Version, Source IP, Destination IP。
    以下字段必须改变：identification, Header checksum。
    源ip地址和目的ip地址是不能变的，要不就不是该ip数据包了。标识字段唯一地标识主机发送的每一份数据报，而校验和是根据前面的数据进行计算，当然会改变。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-14.png" alt="Wireshark-3-14"/></div><br />

### 描述你看到的IP数据包Identification字段值的形式？

- 每一个ip数据包的标志位均不一样，且相邻的ip数据包标志位差1。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-15.png" alt="Wireshark-3-15"/></div>
<div align='center'>（1）</div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-16.png" alt="Wireshark-3-16"/></div>
<div align='center'>（2）</div><br />


# 实验流程三

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-17.png" alt="Wireshark-3-17"/></div><br />

### Identification字段和TTL字段的值是什么？

- 0xa827、60。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-18.png" alt="Wireshark-3-18"/></div><br />

### 最近的路由器（第一跳）返回给你主机的ICMP Time-to-live exceeded消息中这些值是否保持不变？为什么？

- 保持不变，均为60，因为线路固定路由器固定，自然捕获的相关ip数据包ttl也固定。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-19.png" alt="Wireshark-3-19"/></div><br />


# 实验流程四

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-20.png" alt="Wireshark-3-20"/></div><br />

### 该消息是否被分解成不止一个IP数据报？ 

- 是，该消息被分片成两个ip数据包。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-21.png" alt="Wireshark-3-21"/></div><br />

### 观察第一个IP分片，IP头部的哪些信息表明数据包被进行了分片？IP头部的哪些信息表明数据包是第一个而不是最后一个分片？该分片的长度是多少？

- Flags被置为0x01，（more fragments）。Offset不为0，所以不是最后一个分片。该分片的长度是534。
验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-22.png" alt="Wireshark-3-22"/></div><br />


# 实验流程五

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-23.png" alt="Wireshark-3-23"/></div><br />

### 原始数据包被分成了多少片？ 

- 原始数据包被分成了3片。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-24.png" alt="Wireshark-3-24"/></div><br />

### 这些分片中IP数据报头部哪些字段发生了变化？ 

- 数据包头部中有如下字段发生了变化：Total Length ，Flags，Fragment offset，Head checksum。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3-25.png" alt="Wireshark-3-25"/></div><br />

<br />



