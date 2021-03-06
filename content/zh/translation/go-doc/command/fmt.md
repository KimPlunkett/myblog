---
title: "go fmt"
authors: [kiki]
tags: [go-command]
categories: [translation]
draft: false
---

- [命令](#%e5%91%bd%e4%bb%a4)
- [gofmt 的参数](#gofmt-%e7%9a%84%e5%8f%82%e6%95%b0)

## 命令

- go 代码有标准的风格。`go fmt file_name.go` 命令可以格式化写好的代码文件
- 开发工具里面一般都带了保存时候自动格式化功能，这个功能其实在底层就是调用了 `go fmt`
- 使用 `go fmt` 命令，其实是调用了 `gofmt`，而且需要参数 `-w`，否则格式化结果不会写入文件。`gofmt -w -l src` 可以格式化整个项目

## gofmt 的参数

| 参数 | 描述 |
| --- | --- |
| -l | 显示需要格式化的文件 |
| -w | 把改写后的内容直接写入到文件中，而不是作为结果打印到标准输出 |
| -r | 添加形如 “a[b:len(a)] -> a[b:]” 的重写规则，方便做批量替换 |
| -s | 简化文件中的代码 |
| -d | 显示格式化前后的 diff 而不是写入文件，默认是 false |
| -e | 打印所有的语法错误到标准输出。如果不使用此标记，则只会打印不同行的前10个错误 |
| -cpuprofile | 支持调试模式，写入相应的 cpufile 到指定的文件 |
