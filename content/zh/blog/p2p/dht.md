---
title: "分布式原理"
authors: [kiki]
tags: [p2p]
categories: [blog]
draft: false
---

- [Bittorrent协议](#bittorrent%e5%8d%8f%e8%ae%ae)
- [中心化原理](#%e4%b8%ad%e5%bf%83%e5%8c%96%e5%8e%9f%e7%90%86)
- [去中心化原理](#%e5%8e%bb%e4%b8%ad%e5%bf%83%e5%8c%96%e5%8e%9f%e7%90%86)
  - [DHT/kademlia](#dhtkademlia)
  - [NAT](#nat)
  - [IPFS/libp2p](#ipfslibp2p)
- [去中心化流媒体应用livepeer](#%e5%8e%bb%e4%b8%ad%e5%bf%83%e5%8c%96%e6%b5%81%e5%aa%92%e4%bd%93%e5%ba%94%e7%94%a8livepeer)

## Bittorrent协议

  比特流（BitTorrent）是一种内容分发协议，由布拉姆·科恩自主开发。它采用高效的软件分发系统和点对点技术共享大体积文件（如一部电影或电视节目），并使每个用户像网络重新分配结点那样提供上传服务。一般的下载服务器为每一个发出下载请求的用户提供下载服务，而BitTorrent的工作方式与之不同。分配器或文件的持有者将文件发送给其中一名用户，再由这名用户转发给其它用户，用户之间相互转发自己所拥有的文件部分，直到每个用户的下载都全部完成。这种方法可以使下载服务器同时处理多个大体积文件的下载请求，而无须占用大量带宽。

  Bittorrent 中文协议1,2,3,4是有中心化的
  [Bittorrent协议中文版一](https://blog.csdn.net/xxxxxx91116/article/details/8544365?locationnum=14)
  [Bittorrent协议中文版二](https://blog.csdn.net/xxxxxx91116/article/details/8544366)
  [Bittorrent协议中文版三](https://blog.csdn.net/xxxxxx91116/article/details/8544367)
  [Bittorrent协议中文版四](https://blog.csdn.net/xxxxxx91116/article/details/8544370)
  BitTorrent DHT 协议是扩充协议没有中心化的
  [Bittorrent DHT 去中心化协议](https://www.jianshu.com/p/ffeed4801b0e)
  关键名词

  >- peers: 在本文档中，一个 peer 可以是任何参与下载的 BitTorrent 客户端
  >- tracker 是一个响应HTTP GET请求的HTTP/HTTPS服务
  >- node 是一个dht节点

## 中心化原理

  参考 Bittorrent协议中文1,2,3,4原理tracker
    [Bittorrent协议中文版三](https://blog.csdn.net/xxxxxx91116/article/details/8544367)

## 去中心化原理

去中心化，即，不存在数据中心，每个节点的信息存储能力都是对等的，存储的内容都是一致的，账目是公开的（这个公开我也不是很理解，我认为应该是有一定的加密机制）。进行支付时，用户只需要向任意一个节点发送支付信息，网络中的各节点是对等的，它们最终会对如何记录这笔支付达成共识，将这笔支付记入一个公开账本。从这个角度说，去中心化可以提升安全性、提升效率、提升资源利用率，降低系统运行成本

### DHT/kademlia

[P2P中DHT网络介绍](https://blog.csdn.net/mergerly/article/details/7989281)

### NAT

[NAT 的四种类型](https://www.cnblogs.com/my_life/articles/1908552.html)

>- Full Cone NAT
>- Restricted Cone NAT
>- Port Restricted Cone NAT  
>- Symmetric NAT

### IPFS/libp2p

[IPFS/libp2p结构文档](https://github.com/libp2p/specs)

## 去中心化流媒体应用livepeer  

[livepeer源码](https://github.com/livepeer/go-livepeer)
