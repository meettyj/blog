---
layout: post
title:  ISCSI & SCSI
category: jekyll
description: ISCSI(Internet Small Computer System Interface) & SCSI(Small Computer System Interface).
---

<br />

# ISCSI
ISCSI is an Internet Protocol (IP)-based storage networking standard for linking data storage facilities. It provides block-level access to storage devices by carrying SCSI commands over a TCP/IP network. iSCSI is used to facilitate data transfers over intranets and to manage storage over long distances. It can be used to transmit data over local area networks (LANs), wide area networks (WANs), or the Internet and can enable location-independent data storage and retrieval.
<br />


# SCSI
SCSI is a set of standards for physically connecting and transferring data between computers and peripheral devices. The SCSI standards define commands, protocols, electrical and optical interfaces. SCSI is most commonly used for hard disk drives and tape drives, but it can connect a wide range of other devices, including scanners and CD drives, although not all controllers can handle all devices. The SCSI standard defines command sets for specific peripheral device types; the presence of "unknown" as one of these types means that in theory it can be used as an interface to almost any device, but the standard is highly pragmatic and addressed toward commercial requirements.
<br />

![SCSI]({{site.baseurl}}/assets/img/SCSI.png)
<br /><br />

# Difference
SCSI 结构基于客户/服务器模式，其通常应用环境是：设备互相靠近，并且这些设备由 SCSI 总线连接。iSCSI 的主要功能是在 TCP/IP 网络上的主机系统（启动器 initiator）和存储设备（目标器 target）之间进行大量数据的封装和可靠传输过程。此外，iSCSI 提供了在 IP 网络封装 SCSI 命令，且运行在 TCP 上。
