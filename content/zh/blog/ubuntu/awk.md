---
title: "awk 命令"
authors: [kiki]
tags: [ubuntu, linux]
categories: [blog]
draft: false
---

- [脚本基本架构](#%e8%84%9a%e6%9c%ac%e5%9f%ba%e6%9c%ac%e6%9e%b6%e6%9e%84)
- [工作原理](#%e5%b7%a5%e4%bd%9c%e5%8e%9f%e7%90%86)
- [将外部变量传递给 awk](#%e5%b0%86%e5%a4%96%e9%83%a8%e5%8f%98%e9%87%8f%e4%bc%a0%e9%80%92%e7%bb%99-awk)

## 脚本基本架构

- `awk 'BEGIN{ print "start" } pattern{ commands } END{ print "end" }' file`
- 一个 awk 脚本通常由：`BEGIN`语句块、能够使用模式匹配的通用语句块、`END`语句块 3 部分组成
- 这三个部分是可选的。任意一个部分都可以不出现在脚本中，脚本通常是被单引号或双引号中

## 工作原理

`awk 'BEGIN{ commands } pattern{ commands } END{ commands }'`

- 第一步：执行`BEGIN{ commands }`语句块中的语句
  - `BEGIN`语句块在 awk 开始从输入流中读取行之前被执行
  - 这是一个可选的语句块，比如变量初始化、打印输出表格的表头等语句通常可以写在`BEGIN`语句块中
- 第二步：从文件或标准输入(stdin)读取一行，然后执行`pattern{ commands }`语句块，它逐行扫描文件，从第一行到最后一行重复这个过程，直到文件全部被读取完毕
  - `pattern`语句块中的通用命令是最重要的部分，它也是可选的
  - 如果没有提供`pattern`语句块，则默认执行`{ print }`，即打印每一个读取到的行，awk 读取的每一行都会执行该语句块
    - `echo -e "A line 1\nA line 2" | awk 'BEGIN{ print "Start" } { print } END{ print "End" }'`输出

    ```txt
    Start
    A line 1
    A line 2
    End
    ```

- 当使用不带参数的`print`时，它就打印当前行，当`print`的参数是以逗号进行分隔时，打印时则以空格作为定界符。在 awk 的`print`语句块中双引号是被当作拼接符使用
  - `echo | awk '{ var1="v1"; var2="v2"; var3="v3"; print var1,var2,var3; }'`输出`v1 v2 v3`
  - `echo | awk '{ var1="v1"; var2="v2"; var3="v3"; print var1"="var2"="var3; }'`输出`v1=v2=v3`
- 第三步：当读至输入流末尾时，执行`END{ commands }`语句块
  - `END`语句块在 awk 从输入流中读取完所有的行之后即被执行，比如打印所有行的分析结果这类信息汇总都是在END语句块中完成
  - 也是一个可选语句块

## 将外部变量传递给 awk

- 借助`-v`选项，可以将外部值（并非来自 stdin）传递给 awk

  ```shell
  VAR=10000
  echo | awk -v VARIABLE=$VAR '{ print VARIABLE }' ## 输出"10000"
  ```

- 另一种传递外部变量方法

  ```shell
  var1="aaa"
  var2="bbb"
  echo | awk '{ print v1,v2 }' v1=$var1 v2=$var2 ## 输出"aaa bbb"
  ```

- 当输入来自于文件时使用`awk '{ print v1,v2 }' v1=$var1 v2=$var2 filename`
- 以上方法中，变量之间用空格分隔作为 awk 的命令行参数跟随在`BEGIN`、`{}`和`END`语句块之后
