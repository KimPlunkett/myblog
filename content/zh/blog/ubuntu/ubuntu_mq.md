---
title: "RabbitMQ 环境搭建"
authors: [kiki]
tags: [ubuntu, linux]
categories: [blog]
draft: false
---

- [1 安装依赖](#1-%e5%ae%89%e8%a3%85%e4%be%9d%e8%b5%96)
- [2 安装 RabbitMQ](#2-%e5%ae%89%e8%a3%85-rabbitmq)
- [3 启用 RabbitMQ 管理控制台](#3-%e5%90%af%e7%94%a8-rabbitmq-%e7%ae%a1%e7%90%86%e6%8e%a7%e5%88%b6%e5%8f%b0)
  - [3.1 创建用户并设置角色](#31-%e5%88%9b%e5%bb%ba%e7%94%a8%e6%88%b7%e5%b9%b6%e8%ae%be%e7%bd%ae%e8%a7%92%e8%89%b2)
- [4 RabbitMQ 服务命令](#4-rabbitmq-%e6%9c%8d%e5%8a%a1%e5%91%bd%e4%bb%a4)
- [5 修改服务配置文件](#5-%e4%bf%ae%e6%94%b9%e6%9c%8d%e5%8a%a1%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6)

## 1 安装依赖

```sh
# 添加 erlang 源到 apt 仓库
wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
sudo dpkg -i erlang-solutions_1.0_all.deb
# 更新安装
sudo apt-get update
sudo apt-get install erlang
```

## 2 安装 RabbitMQ

```sh
# 调用官方安装脚本
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.deb.sh | sudo bash
# 添加 RabbitMQ 签名 (会出现 403 错误，可忽略不运行)
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -
# 更新并安装
sudo apt-get update  #（可忽略不运行）
sudo apt-get install rabbitmq-server
```

## 3 启用 RabbitMQ 管理控制台

启用管理插件和 STOMP 插件:

```sh
sudo rabbitmq-plugins enable rabbitmq_management rabbitmq_stomp
# 重启服务器
sudo systemctl restart rabbitmq-server
```

登录 <http://localhost:15672> web管理页面 默认提供 guest 账号(密码：guest)，但是该账号只提供 localhost 登录，所以需要单独创建用户，使用 rabbitmqctl。
用户相关命令如下：

```sh
$ sudo rabbitmqctl help | grep user
    add_user <username> <password>  # 创建用户
    delete_user <username>          # 删除用户
    change_password <username> <newpassword>  # 修改密码
    clear_password <username>                 # 清楚密码，直接登录
    authenticate_user <username> <password>   # 测试用户认证（我也不知道2333）
    set_user_tags <username> <tag> ...        # 设置用户权限 []
    list_users
    set_permissions [-p <vhost>] <user> <conf> <write> <read>
    clear_permissions [-p <vhost>] <username>
    list_user_permissions <username>
```

### 3.1 创建用户并设置角色

创建管理员用户，负责整个 MQ 的运维：

```sh
# 添加用户
sudo rabbitmqctl add_user  admin  admin
# 赋予其 administrator 角色
sudo rabbitmqctl set_user_tags admin administrator
# 为用户赋权
sudo rabbitmqctl  set_permissions -p / admin '.*' '.*' '.*'
# 查看权限
sudo rabbitmqctl list_user_permissions admin
```

## 4 RabbitMQ 服务命令

```sh
# 启动服务
# sudo service rabbitmq-server start
sudo systemctl start rabbitmq-server
# 停止服务
sudo systemctl stop rabbitmq-server
# 重启服务
sudo systemctl restart rabbitmq-server
# 检查服务状态
sudo systemctl status rabbitmq-server
```

## 5 修改服务配置文件

```sh
# 如果需要管理最大连接数，修改配置文件
sudo vim /etc/default/rabbitmq-server
```
