---
layout: post
title:   Wireshark (1) - 对HTTP协议的分析
category: jekyll
description: A free and open source packet analyzer used for Hypertext Transfer Protocol analysis.
---

<br />

在对ntop软件所产生的结果分析完后，我们可以通过Wireshark得到更进一步的分析，目标研究网址为<a href="http://hitgs.hit.edu.cn/">http://hitgs.hit.edu.cn/</a>。

首先我们先对其HTTP协议进行分析，过程如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/1.png" alt="Wireshark-1"/></div><br />

因为是先开始抓包再输入网址，所以每次的抓包结果都会在开始显示一大堆的SSDP，即使是在筛选框中输入了HTTP情况下，一行行的m-search还是在结果中占据了很大的篇幅，这不禁让我产生了以下疑问：

### SSDP是什么？m-search是什么？

- 经过多方面的寻求资料，我得到了如下结果：<br />
　　SSDP是 Simple Service Discovery Protocol的缩写，其中文翻译是简单服务发现协议，这是一种应用层协议。<br />
　　其主要用途是提供了在局部网络里面发现设备的机制。即控制点（也就是接受服务的客户端）可以通过使用简单服务发现协议，根据自己的需要查询在自己所在的局部网络里面提供特定服务的设备。设备（也就是提供服务的服务器端）也可以通过使用简单服务发现协议，向自己所在的局部网络里面的控制点声明它的存在。<br />
　　而m-search则是来源于当控制点（客户端）接入网络的时候，它可以向一个特定的多播地址的SSDP端口使用M-SEARCH方法发送“ssdp:discover”消息。而多播地址则一般使用239.255.255.250和UDP端口号1900。如下图所示：<br />

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/2.png" alt="Wireshark-2"/></div><br />

- 下面附上其典型的设备查询请求消息格式，便于直观理解：<br />
        `M-SEARCH * HTTP/1.1`<br />
        `HOST: 239.255.255.250:1900`  <br />
        `MAN: "ssdp:discover"`<br />
        `MX: seconds to delay response`<br />
        `ST: search target`<br /><br />
- 各字段含义解释如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/3.png" alt="Wireshark-3"/></div><br />

而当设备接收到查询请求并且查询类型（ST字段值）与此设备匹配时，设备必须向多播地址239.255.255.250:1900回应响应消息。而此处没有收到相关报文，因此先记录下相关内容方便以后学习：<br />
- 其典型的消息格式如下：<br />
`HTTP/1.1 200 OK`<br />
`CACHE-CONTROL: max-age = seconds until advertisement expires`<br />
`DATE: when reponse was generated`<br />
`EXT:`<br />
`LOCATION: URL for UPnP description for root device`<br />
`SERVER: OS/Version UPNP/1.0 product/version`<br />
`ST: search target`<br />
`USN: advertisement UUID`<br />

- 各HTTP协议头的含义简介如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/4.png" alt="Wireshark-4"/></div><br />

# 相关认识及问题解答

### 你的浏览器运行的是HTTP1.0，还是HTTP1.1？你所访问的服务器所运行HTTP协议的版本号是多少？

- 浏览器运行的是HTTP1.1。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/5.png" alt="Wireshark-5"/></div><br />

- 版本号是4。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/6.png" alt="Wireshark-6"/></div><br />

### 你的浏览器向服务器指出它能接收何种语言版本的对象？

- 可以接受简体中文、英文两者语言的版本。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/7.png" alt="Wireshark-7"/></div><br />

不难发现，里面出现了q=0.8、q=0.3这样的数值，这让我不禁有了如下疑问：

### q又是什么呢？

- 简单来说，语言可以携带一个q值，来表示用户对该语言的喜好程度，这个值在 0 - 1 之间，默认为 1。
- 我也找到了相关的RFC文档，附上网址如下，就不对对其进行无意义的复制粘贴了。

<a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html">https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html</a>

### 从服务器向你的浏览器返回的状态代码是多少？

- 收到的状态代码是200。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/8.png" alt="Wireshark-8"/></div><br />

### 分析你的浏览器向服务器发出的第一个HTTP GET请求的内容，在该请求报文中，是否有一行是：IF-MODIFIED-SINCE？

- 没有IF-MODIFIED-SINCE，因为是第一次访问该页面，客户端发请求时，请求头中没有If-Modified-Since标签。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/9.png" alt="Wireshark-9"/></div><br />

### 分析服务器响应报文的内容，服务器是否明确返回了文件的内容？如何获知？

- 因为有三次握手原则，故服务器对第一次请求建立连接的请求报文并没有返回文件内容，而下一次的响应报文则携带了内容。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/10.png" alt="Wireshark-10"/></div>
<div align='center'>（1）第一次响应报文</div>

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/11.png" alt="Wireshark-11"/></div>
<div align='center'>（2）第二次响应报文（携带数据）</div><br />

### 分析你的浏览器向服务器发出的较晚的“HTTP GET”请求，在该请求报文中是否有一行是：IF-MODIFIED-SINCE？如果有，在该首部行后面跟着的信息是什么？

- 对于研究网址（<a href="http://hitgs.hit.edu.cn/">http://hitgs.hit.edu.cn/</a>）的请求报文一直没有找到IF-MODIFIED-SINCE信息，猜测这可能跟该网站的更新有关，故当我测试网站换至新浪首页（<a href="http://www.sina.com.cn">http://www.sina.com.cn</a>）时，如愿以偿的在get请求报文中找到了IF-MODIFIED-SINCE信息。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/12.png" alt="Wireshark-12"/></div><br />

### 服务器对较晚的HTTP GET请求的响应中的HTTP状态代码是多少？服务器是否明确返回了文件的内容？

- 状态代码是304 not modifie。服务器也并没有返回文件内容，只是返回了一个HTTP头部。之所以这样做是因为在客户端已经有缓冲的文件的情况下，向服务器发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告诉客户，原来缓冲的文档还可以继续使用。这样就不用把相关数据重新传输到客户端，从而达到节省带宽的目的。验证结果示意图如下：

<div align='center'>
<img src="{{site.baseurl}}/assets/img/Wireshark/13.png" alt="Wireshark-13"/></div><br />
























