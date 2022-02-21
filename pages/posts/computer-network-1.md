---
title: 计算机网络基础
date: 2022/02/21 10:00
description: 开始复习并整理一些计算机网络的知识
tag: 计算机网络
author: Mowtwo
---

# 计算机网络基础

## 什么是计算机网络

> [Chapter 1: What is a Network? (usf.edu)](https://fcit.usf.edu/network/chap1/chap1.htm)
>
> A network consists of two or more computers that are linked in order to share resources (such as printers and CDs), exchange files, or allow electronic communications. The computers on a network may be linked through cables, telephone lines, radio waves, satellites, or infrared light beams.

## 局域网与因特网

局域网是一种范围大小不定区域网络模式，现代路由器组成的最小内网也可以算是一种局域网。

其实不管多大的网络组成，本质都跟局域网是类似的结构，但是在不同的地方使用了网关，路由器，交换机形成新的网络节点，每个节点又是一个单独的网络，这种网络模式被称为互联网(**i**nternet)。

而全世界范围内所有（常言情况下，不包括各类保密网络，比如军事专用网络）的网络节点组成的巨大的网络结构被称之为因特网（Internet），即为一种特定名称的互联网。

因特网更为特殊的一点是，在因特网内的网络的基础连接与通信方式也是相同的，这部分被称之为协议（protocol）。

而因特网所使用的常用协议为HTTP，HTTP也是目前世界上最主流的互联网的通信协议之一，目前HTTP已经升级到3.0版本，而目前正常流通的版本为HTTP/2，并且在其之上还有一种基于SSL技术进行加密的协议，被称之为HTTPS。

## HTTP协议

HTTP，即为超文本传输协议（HyperText Transfer Protocol）缩写，是一种基于`请求（Request）->应答（Response）`模式的互联网通信协议，其完整协议组成为TCP/IP。

而TCP/IP所支持的协议还有很多扩展，下面列举部分协议与其对应功能：

- TCP（Transmission Control Protocol，传输控制协议）：负责应用程序（application）之间的通信
- IP（Internet Protocol，网络间协议）：负责主机（host）之间的通信
- UDP（User Data Protocol，用户数据协议）：区别于TCP的一种无连接通信协议，功能与TCP类似
- DHCP（Dynamic Host Configuration Protocol，动态主机配置协议）：一般用与主机在路由模式下动态分配IP
- ICMP（Internet Control Message Protocol，因特网报文协议）：是IP协议中的一个组成部分，实际上TCP/IP协议在传输时使用的就是报文

虽然协议众多，而且在功能上也是多种多样，但是在起源上，基本上都是一致的。

## OSI七层模型与TCP

![img](https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/20160825152622511)

OSI即为Open System Interconnection Reference（开放式通信系统互联参考模型），有ISO（International Organization for Standardization，国际标准化）组织规定。

> FTP（File Transfer Protocol）：文件传输协议
>
> SMTP（Simple Mail Transfer Protocol）：简单邮件传输协议