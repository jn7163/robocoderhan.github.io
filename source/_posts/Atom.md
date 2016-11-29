---
title: Atom的使用与配置
tags:
- Atom
categories:
- 使用手册
---

[Atom官网](https://atom.io/)

<!--more-->

### 快捷键

`Ctrl + Shift + p`: 打开命令输入框

### Atom && Markdown

Atom自带了Markdown语法高亮和预览功能，通过`Ctrl+Shift+m`可以打开预览窗口，但是不支持数学公式预览，可以通过以下方式修改：

#### 预览

* 安装预览插件

        Ctrl + Shift + p
        settings view: install packages and themes
        markdown-preview-plus

* 禁用Atom自带的预览插件

        Ctrl + Shift + p
        settings view view installed packages
        markdown-preview

* 查看效果

    markdown预览：`Ctrl + Shift + m`，数学公式预览：`Ctrl + Shift + x`

#### 导出PDF

* 安装导出插件

        Ctrl + Shift + p
        settings view: install packages and themes
        markdown-themeable-pdf

* 使用

        Ctrl + Shift + e

### [Atom && LaTeX](https://www.zhihu.com/question/19954023)

* Tex介绍

[TeX、LaTeX、TeXLive 小结](http://blog.csdn.net/dbzhang800/article/details/6820659)

* 安装latexmk

        sudo apt-get install latexmk

* 安装Atom插件

语言高亮： language-latex

编译： latex （Ctrl + Alt + B 执行编译）

PDF 预览： pdf-view

自动补齐： latexer

* 使用

        ctrl-alt-b  latex:build
        ctrl-alt-s  latex:sync
        ctrl-alt-c  latex:clean

**通过这种方式直接用中文会出错，而且好像通过xelatex的方式也不行，只能默认的pdflatex。好像安装完整的TeXLive可以解决，但是暂时还用不到，等用到了再尝试以下的方式解决。**

[TeXLive](http://tug.org/texlive/)

[ubuntu 64位下安装 texlive2015 并设置 ctex 中文套装](http://blog.csdn.net/song0601013ndsc/article/details/48203311)

[ubuntu 下安装 texlive 并设置 ctex 中文套装](http://www.cnblogs.com/lienhua34/p/3675027.html)

[TeXLive百度云链接](http://pan.baidu.com/s/1dDnnhvz)
