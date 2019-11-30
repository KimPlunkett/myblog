---
title: "SM3 密码杂凑算法"
authors: [kiki]
tags: [sip]
categories: [blog]
draft: false
---

## 算法描述

- 长度 len (len < 2^64) 比特的消息 message, 经过填充、迭代压缩、输出选裁, 输出 256 比特 (64 byte) 的杂凑值

## 应用

- 数字签名和验证
- 消息认证码的生成与验证
- 随机数的生成

## 参考

- GMT 0004-2012 SM3密码杂凑算法
