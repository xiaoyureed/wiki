---
title: blockchain
tags:
---

https://github.com/rust-bitcoin/rust-bitcoin

<!-- more -->

<!-- TOC -->

- [语言](#语言)
- [区块链简介](#区块链简介)
    - [三元悖论](#三元悖论)
    - [开源库](#开源库)
- [比特币](#比特币)
- [密码学](#密码学)
- [p2p 网络](#p2p-网络)
- [高可用](#高可用)
- [高性能](#高性能)

<!-- /TOC -->



# 语言

- cpp : 密码学库成熟

    bitcoin, zcash(基于隐私的, 从 比特币改的), ethereum (以太坊)

- golang: 语法简单, 语法层面支持并发

    fabric, ethereum, ipfs

- rust: 性能强, 类型系统强, 表达能力强, 和 c 无缝交互

    Parity/Substrate (以太坊实现), zcash   (Parity 公司是 rust 区块链先行者公司)

- Haskell: 强大的类型系统   , 在编译期就可以暴露很多错误

    cardano, Dfinity

- js

- kotlin

- Python


# 区块链简介

区块链 采用密码学的方法来保证己有的数据不可篡改，采用共识算法为新增的数据达成共识 ， 这完 全是与生俱来的且去中心化的“公信力”

## 三元悖论

https://www.jianshu.com/p/934226233098



## 开源库

数据库 parity-rocksdb

大数库 numext bigint

p2p 库 rust-libp2p

vm 库 ckb-vm cita-vm

签名算法 libsm secp256k1



# 比特币

区块链鼻祖: 比特币 (a  use case for   block chain ) --- 去中心化, 防篡改的账本

08 年, 论文 -> Bitcoin: A Peer-to-Peer Electronic Cash System

原理: 

1. 用户发起交易请求, 用户节点上的 bitcoin program 发出节点交易广播
1. 矿工节点收到数据, 求解难题, 打包成区块, 发出区块广播
1. 各个节点将区块加入本地链条末尾




# 密码学

哈希算法

签名算法


# p2p 网络


# 高可用





# 高性能
