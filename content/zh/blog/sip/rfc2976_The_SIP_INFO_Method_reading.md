---
title: "rfc2976: The SIP INFO Method"
authors: [kiki]
tags: [sip]
categories: [blog]
draft: false
---

- INFO 消息的目的是沿着 SIP 信令通路携带应用层信息。INFO 不用来改变 SIP 呼叫的状态，也不改变 SIP 初始化的会话的状态
- 信息可以放在 INFO 消息的头或者作为消息体的一部分
- 如果找不到对应的会话，UAS 必须回复 481
- 如果接收 INFO 请求的服务可以理解消息体，但是没有处理规则，应当回复 200，可以将消息体展示给用户
- 如果接收 INFO 请求的服务不能理解消息体，也没有处理规则，应当回复 415
- INFO 请求可以被取消。收到 CANCEL 一个 INFO 的请求，UAS 如果还没有回复 INFO，应当返回 487，并不再处理对应的 INFO 请求
