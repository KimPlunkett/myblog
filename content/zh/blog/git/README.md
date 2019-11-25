---
title: "git 教程"
authors: [kiki]
tags: [git]
categories: [blog]
draft: false
---

- [git 安装](#git-%e5%ae%89%e8%a3%85)
- [git 配置](#git-%e9%85%8d%e7%bd%ae)
- [git 帮助](#git-%e5%b8%ae%e5%8a%a9)
- [git 基础](#git-%e5%9f%ba%e7%a1%80)
  - [git 仓库、工作目录、暂存区域、文件状态](#git-%e4%bb%93%e5%ba%93%e5%b7%a5%e4%bd%9c%e7%9b%ae%e5%bd%95%e6%9a%82%e5%ad%98%e5%8c%ba%e5%9f%9f%e6%96%87%e4%bb%b6%e7%8a%b6%e6%80%81)
  - [初始化版本库](#%e5%88%9d%e5%a7%8b%e5%8c%96%e7%89%88%e6%9c%ac%e5%ba%93)
    - [新建版本库](#%e6%96%b0%e5%bb%ba%e7%89%88%e6%9c%ac%e5%ba%93)
    - [克隆版本库](#%e5%85%8b%e9%9a%86%e7%89%88%e6%9c%ac%e5%ba%93)
    - [版本库目录 .git](#%e7%89%88%e6%9c%ac%e5%ba%93%e7%9b%ae%e5%bd%95-git)
    - [blob 对象](#blob-%e5%af%b9%e8%b1%a1)
    - [tree 对象](#tree-%e5%af%b9%e8%b1%a1)
    - [commit 对象](#commit-%e5%af%b9%e8%b1%a1)
  - [常用 git 命令](#%e5%b8%b8%e7%94%a8-git-%e5%91%bd%e4%bb%a4)
- [git 服务器](#git-%e6%9c%8d%e5%8a%a1%e5%99%a8)
- [参考网站](#%e5%8f%82%e8%80%83%e7%bd%91%e7%ab%99)
- [好玩的网站](#%e5%a5%bd%e7%8e%a9%e7%9a%84%e7%bd%91%e7%ab%99)

## git 安装

- [Linux](https://git-scm.com/download/linux)

```sh
# Fedora
sudo yum install git
# Debian
sudo apt-get install git
sudo apt-get install gitk git-gui
# 源码安装,[下载地址](http://git-scm.com/download)
sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
export GIT_VER=2.0.0
wget https://www.kernel.org/pub/software/scm/git/git-$GIT_VER.tar.gz
tar -zxf git-$GIT_VER.tar.gz
pushd git-$GIT_VER
make prefix=/usr/local all
sudo make prefix=/usr/local install
popd
```

- Windows
  - [msysgit](http://git-scm.com/download/win)
  - Git 图形化操作程序, [TortoiseGit](https://tortoisegit.org/)
  - [GitHub for Windows](http://windows.github.com/)
- Mac
  - 安装 Xcode 后自动装上 Git
  - [使用图形化的Git 安装工具Git OS X](https://sourceforge.net/projects/git-osx-installer/)

## git 配置

- 参考[git_config](git_config.md)

## git 帮助

- 有三种命令可以找到 git 命令的使用手册
  - `git help <verb>`
  - `git <verb> --help`
  - `man git-<verb>`
    - `git help config`            # 查看 config 命令的手册

## git 基础

### git 仓库、工作目录、暂存区域、文件状态

- git仓库：git 用来保存项目的元数据和对象数据库的地方
- 工作目录：对项目的某个版本独立提取出来的内容.  是从 git 仓库的压缩数据库中提取出来的文件, 放在磁盘上使用或修改
- 暂存区域(索引)：一个文件, 保存了下次将提交的文件列表信息, 一般在 Git 仓库目录中
- 基本的 Git 工作流程如下：
  - 在工作目录中修改文件
  - 暂存文件, 将文件的快照放入暂存区域
  - 提交更新, 找到暂存区域的文件, 将快照永久性存储到 git 仓库目录
- 文件状态：![git-文件的状态变化周期](gitfilestatus.png "git-文件的状态变化周期")
  - 已提交状态(commited)：git 目录中保存着的特定版本文件
  - 已暂存状态(staged)：作了修改并已放入暂存区域
  - 工作目录下的每一个文件都属于已跟踪(tracked)或未跟踪(untracked),已跟踪的文件状态可处于未修改(unmodified)、已修改(modified)或已放入暂存区(staged)

### 初始化版本库

#### 新建版本库

```sh
# 确定版本库目录
mkdir dirname
pushd dirname
# 生成 .git 目录以及其下的版本历史记录文件, push 时易出现冲突
git init
# 创建一个裸仓库, 只保存git历史提交的版本信息, 不允许用户在上面进行各种 git 操作
git init --bare
```

#### 克隆版本库

- 自动将其添加为远程仓库并默认以 “origin” 为简写
- 自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支

```sh
# 1 使用 ssh 协议克隆
# 1.1 默认本地仓库名字和远程仓库相同
git clone git@github.com:tensorflow/tensorflow.git
# 在当前目录下创建 tensorflow 目录, 并在这个目录下初始化一个 .git 文件夹,
# 从远程仓库拉取下所有数据放入 .git 文件夹, 然后从中读取最新版本的文件的拷贝
# 1.2 自定义本地仓库的名字克隆
git clone git@github.com:tensorflow/tensorflow.git MyTensorflow
# 2 使用 HTTPS 协议克隆
git clone https://github.com/tensorflow/tensorflow.git
```

#### 版本库目录 .git

| 文件(夹)名 | 描述 |
| --- | --- |
| HEAD | 指向最新提交 |
| config | 项目的配置信息, git config 命令会改动它 |
| description | 项目的描述信息 |
| hooks | 系统默认钩子脚本目录 |
| index | 索引文件，记录统计版本库的每个文件，如大小、创建时间和最后修改时间，对比当前统计信息和索引确定文件是否被修改 |
| logs | 各个 refs 的历史信息 |
| objects | git 本地仓库的所有对象(commits, trees, blobs, tags) |
| refs | 标识项目里的每个分支指向了哪个提交(commit) |

#### blob 对象

- git 基于“内容寻址”：文件不是按照文件名存储，而是按照包含内容的哈希值，存在一个叫“blob 对象”的文件中
- 可以把文件内容的哈希值看做一个唯一 ID
- blob 对象存储文件内容，与文件名无关

#### tree 对象

- tree 对象：一组包含文件类型、文件名和哈希值的数据
- tree 对象存储文件名

#### commit 对象

- commit 对象存储提交信息及其创建的日期和时间
- commit 对象是原子性的，即一个提交不会部分地记录变更

### 常用 git 命令

- 添加文件参考[git_add](./git_add.md)
- 防止文件误添加
  - 修改 .gitignore
  - 修改 .git/info/exclude
  - 格式规范参考[git_ignore](./git_ignore.md)
- 提交更新参考[git_commit](./git_commit.md)
- 跟踪状态参考[git_status](./git_status.md)
- 提交日志参考[git_log](./git_log.md)
- 移除文件参考[git_rm](./git_rm.md)
- 移动文件参考[git_mv](./git_mv.md)
- 远程仓库参考[git_remote](./git_remote.md)
- 推送数据参考[git_push](./git_push.md)
- 标签参考[git_tag](./git_tag.md)
- 分支参考[git_branch](./git_branch.md)
- 变基参考[git_rebase](./git_rebase.md)
- 版本比较参考[git_diff](./git_diff.md)
- 撤消操作
  - `git commit --amend`  # 尝试重新提交
  - `git reset HEAD f1`   # 取消暂存文件 f1
  - `git checkout -- f1`  # 撤消之前对文件 f1 所做的修改
  - 参考[git_reset](./git_reset.md)

## git 服务器

- 协议
  - 本地协议: 远程版本库就是硬盘内的另一个目录. 常见于团队每一个成员都对一个共享的文件系统(例如一个挂载的 NFS)拥有访问权, 或者比较少见的多人共用同一台电脑的情况
    - `git clone /opt/git/project.git`      # Git 会尝试使用硬链接（hard link）或直接复制所需要的文件
    - `git clone file:///opt/git/project.git`

## 参考网站

- [Git Magic](http://www-cs-students.stanford.edu/~blynn//gitmagic/)
- [git-scm](https://git-scm.com/book/en/v2)

## 好玩的网站

- [github blog](https://github.blog/)