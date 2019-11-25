---
title: "git clean 清理工作目录"
authors: [kiki]
tags: [git]
categories: [blog]
draft: false
---

- `git clean` 从工作目录移除未被跟踪的文件，直接删除，不能从回收站找到
- `-d` 移除未跟踪的目录
- `-f` 强制移除文件或目录，如果 `clean.requireForce` 设置为 true，`git clean` 只有添加 `-f``-n``-i` 才会清理

```sh
# 移除工作目录中所有未追踪的文件以及空的目录
git clean -f -d
```

- `-n` “演示”查看将会清除的内容，不会移除任何文件或目录

```sh
# 可以打印“将要移除什么”，并未真正移除，相当于“演示”
git clean -d -n
```

- `-x` 不使用 `.gitignore` 或者 `$GIT_DIR/info/exclude` 指定的忽略规则，仍然使用 `-e` 选项指定的忽略规则
  - 可以移除所有的未跟踪文件，包括构建目录，可以用来创建一个干净的工作目录

```sh
# 移除未跟踪的文件和目录
git clean -d -x
```

- `-X` 只移除忽略的文件，可以用于保留手动创建的文件
