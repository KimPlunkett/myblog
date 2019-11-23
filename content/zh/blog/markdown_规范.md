---
title: "Markdown 规范"
subtitle: ""
summary: ""
authors: [kiki]
tags: [markdown]
categories: [blog]
date: 2019-11-10T16:22:30+08:00
lastmod: 2019-11-10T16:22:30+08:00
featured: false
draft: false

image:
  caption: ""
  focal_point: ""
  preview_only: false

projects: []
---

- [编辑器](#%e7%bc%96%e8%be%91%e5%99%a8)
- [基本技巧](#%e5%9f%ba%e6%9c%ac%e6%8a%80%e5%b7%a7)
  - [标题](#%e6%a0%87%e9%a2%98)
  - [代码](#%e4%bb%a3%e7%a0%81)
  - [粗斜体](#%e7%b2%97%e6%96%9c%e4%bd%93)
  - [换行](#%e6%8d%a2%e8%a1%8c)
  - [链接](#%e9%93%be%e6%8e%a5)
  - [列表](#%e5%88%97%e8%a1%a8)
    - [普通无序列表](#%e6%99%ae%e9%80%9a%e6%97%a0%e5%ba%8f%e5%88%97%e8%a1%a8)
    - [普通有序列表](#%e6%99%ae%e9%80%9a%e6%9c%89%e5%ba%8f%e5%88%97%e8%a1%a8)
    - [列表嵌套](#%e5%88%97%e8%a1%a8%e5%b5%8c%e5%a5%97)
  - [表格](#%e8%a1%a8%e6%a0%bc)
  - [引用](#%e5%bc%95%e7%94%a8)
    - [普通引用](#%e6%99%ae%e9%80%9a%e5%bc%95%e7%94%a8)
    - [引用嵌套引用](#%e5%bc%95%e7%94%a8%e5%b5%8c%e5%a5%97%e5%bc%95%e7%94%a8)
    - [引用嵌套列表](#%e5%bc%95%e7%94%a8%e5%b5%8c%e5%a5%97%e5%88%97%e8%a1%a8)
    - [引用嵌套代码块](#%e5%bc%95%e7%94%a8%e5%b5%8c%e5%a5%97%e4%bb%a3%e7%a0%81%e5%9d%97)
  - [图片](#%e5%9b%be%e7%89%87)
  - [分隔符](#%e5%88%86%e9%9a%94%e7%ac%a6)
- [高级技巧](#%e9%ab%98%e7%ba%a7%e6%8a%80%e5%b7%a7)
  - [行内 HTML 元素](#%e8%a1%8c%e5%86%85-html-%e5%85%83%e7%b4%a0)
  - [符号转义](#%e7%ac%a6%e5%8f%b7%e8%bd%ac%e4%b9%89)
  - [公式](#%e5%85%ac%e5%bc%8f)
  - [脚注](#%e8%84%9a%e6%b3%a8)
- [文档规范](#%e6%96%87%e6%a1%a3%e8%a7%84%e8%8c%83)

## 编辑器

- Mac
  - [Mou](http://25.io/mou/)
- Windows
  - [MarkdownPad](http://www.markdownpad.com/)
  - [MarkPad](http://code52.org/MarkPadRT/)
- Linux
  - [ReText](https://github.com/retext-project/retext)
  - Vim+[Vimwiki](http://xbeta.info/vimwiki.htm)
- 在线编辑器
  - [markable](https://markable.in/)
  - [dillinger](https://dillinger.io/)
- 浏览器插件
  - MaDe(chrome)
  - [马克飞象](https://maxiang.io/)
- 高级应用
  - [Sublime Text 2](http://www.sublimetext.com/2) + [MarkdownEditing](http://ttscoff.github.io/MarkdownEditing/) / [教程](https://lucifr.com/2012/07/12/markdownediting-for-sublime-text-2/)

## 基本技巧

### 标题

Markdown 支持两种标题的语法，类 [Setext](http://docutils.sourceforge.net/mirror/setext.html) 和类 [atx](http://www.aaronsw.com/2002/atx/) 形式  
类 atx 形式则是在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶(可以在行尾加上 #)

```md
# 一级标题 #
## 二级标题 ##
### 三级标题 ###
#### 四级标题 ####
```

类 Setext 形式是用底线的形式，利用 = (最高阶标题)和 - (第二阶标题)(任何数量的 = 和 - 都可以有效果)

```md
一级标题
============
二级标题
------------
```

### 代码

- 行内代码：用反引号标记行内代码，如
  - `function_name()`
  - ``包含反引号`的代码``
- 代码段，通常编辑器根据代码片段适配合适的高亮方法
  - 可以用[三个`]包裹一段代码，并指定一种语言
  
    ```cpp
    int test()
    {
      return 0;
    }
    ```
  
  - 也可以使用 4 空格或是 1 个制表符缩进，再贴上代码，实现相同的的效果  
  
    int test()
    {
      return 0;
    }
  
  - 如果不需要代码高亮，可以用下面的语法禁用
  
    ```nohighlight
    int test()
    {
      return 0;
    }
    ```

### 粗斜体

Markdown 使用星号和底线作为标记强调字词的符号  
*斜体*  _斜体_  
**粗体**  __粗体__  
***粗斜体*** ___粗斜体___

### 换行

另起一行，只需要在当前结尾加2个空格  
这样就会另起一行

空出一行，即可新起一个段落

行尾加斜线，\
也可实现换行

### 链接

- 行内式链接
  - 文字链接：方括号(链接名称)+圆括号(链接地址) [链接名称](http://链接地址)
    - [google](https://translate.google.com/)
    - [gmail](https://mail.google.com/mail/u/0/)
  - 自动链接：Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，尖括号+(链接地址)
    - <https://translate.google.com/>
    - <https://mail.google.com/mail/u/0/>
- 参考式链接：在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记，然后在文件的任意处为标记变量赋值(网址)
  - 隐式链接标记：可以省略指定链接标记，这种情形下，链接标记会视为等同于链接文字
  - 这个链接用 Google 作为网址变量 [Google][]
  - 这个链接用 yahoo 作为网址变量 [Yahoo!][yahoo]
  - 链接标记的内容定义的形式为：
    - 方括号(前面可以选择性地加上至多三个空格来缩进)，里面输入链接文字
    - 接着一个冒号
    - 接着一个以上的空格或制表符
    - 接着链接的网址(链接网址也可以用尖括号包起来)
    - 选择性地接着 title 内容，可以用单引号、双引号或是括弧包着

[Google]: http://www.google.com/ "Hi, Google"
[yahoo]: http://www.yahoo.com/ "Hi, Yahoo"

### 列表

#### 普通无序列表

```md
- 无序列表，文本前使用[减号+空格]
+ 无序列表，文本前使用[加号+空格]
* 无序列表，文本前使用[星号+空格]
```

#### 普通有序列表

1. 列表前使用[数字+英文句点+空格]
2. 可以自动添加数字
3. 数字不对显示的时候回自动调整
4. 文档开始如果出现‘1986. blabla’要写成‘1986\. blabla’

5) 列表前使用[数字+)+空格]
6) 可以自动添加数字
7) 数字不对显示的时候回自动调整
8) 文档开始如果出现‘1986. blabla’要写成‘1986\. blabla’

#### 列表嵌套

1. 列出所有元素：
    - 无序列表元素 A
        1. 元素 A 的有序子列表
    - 前面加 4 个空格或 1 个制表符
2. 列表里的多段换行：  
    前面必须加 4 个空格或 1 个制表符，  
    这样换行，整体的格式不会乱
3. 列表里引用：

    > 前面空一行  
    > 仍然需要在 > 前面加 4 个空格或 1 个制表符

4. 列表里代码段：

    前面 4 个空格或 1 个制表符，之后按三个`代码语法

        或者直接空 8 个空格或 2 个制表符，
        引入代码块

### 表格

默认：左对齐(col1)

| col1 | col2 | col3 |
| --------- |:--------: | -----: |
| col1 | col2 | col3 |
| col1,col1 | col2,col2 | col3,col3 |
| col1,col1,col1 | col2,col2,col2 | col3,col3,col3 |

### 引用

#### 普通引用

> 引用前使用[大于号+空格]  
换行可以不加  
>
> 空行和新起一行需要加上

#### 引用嵌套引用

> 最外层引用
> > 多一个[大于号+空格]嵌套一层引用
> > > 可以嵌套很多层

#### 引用嵌套列表

> - 这是引用里嵌套的一个列表
> - 还可以有子列表
>   - 子列表需要从[减号、加号、星号]之后延后 4 个空格或 1 个制表符开始

#### 引用嵌套代码块

>     同样的，在前面加 4 个空格或 1 个制表符形成代码块
>  

```code
或者使用三个`形成代码块
```

### 图片

- 跟链接的方法区别在于前面加了个感叹号。行内式的图片语法 感叹号+方括号(图片名称)+圆括号(图片链接地址/图片相对路径)：  
  ![图片名称](http://media.52poke.com/wiki/0/0d/025Pikachu.png "皮卡丘__1")

  ![皮卡丘__2](natsume.jpg)

  ![皮卡丘__2][pika]

[pika]: natsume.jpg

### 分隔符

在新起一行输入三个减号、星号、底线，即可实现分割线。当前后都有段落时，请空出一行。  

段落1

---

段落2

---

段落3

## 高级技巧

### 行内 HTML 元素

目前只支持段内 HTML 元素效果，包括 kbd/b/i/em/sup/sub/br 等。现不建议使用 HTML 元素
  
- 键位显示： 使用 `<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd>` 重启电脑
- 代码块：使用 pre/code 标签
- 粗斜体：`<b>粗体</b>`

### 符号转义

在符号前加反斜杠可以避免被转义。如：  

    \_不想这里的文本变斜体\_  
    \*\*不想这里的文本被加粗\*\*

Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

\\   反斜线  
\`   反引号  
\*   星号  
\_   底线  
\{\}  花括号  
\[\]  方括号  
\()  括弧  
\#   井字号  
\+   加号  
\-   减号  
\.   英文句点  
\!   惊叹号  

---

### 公式

当需要在编辑器中插入数学公式时，可以使用两个美元符包裹 TeX 或 LaTeX 格式的数学公式来实现。提交后，问答和文章页会根据需要加载 Mathjax 对数学公式进行渲染。如：

$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a}. $$

$$ x \href{why-equal.html}{=} y^2 + 1 $$

---

### 脚注

Markdown 官网[^markdown]

[^markdown]: <https://daringfireball.net/projects/markdown/>

## 文档规范

- 标题用\#，右边的\#可不加
- 行内代码
  - 三个反引号
  - 制表符
  - code 标签
- 代码段
  - 三个反引号
  - 制表符
  - pre/code 标签
