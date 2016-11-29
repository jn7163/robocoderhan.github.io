---
title: 定制Ubuntu主题
tags:
- Linux
categories:
- 使用手册
---

### 安装主题与系统增强管理工具

Unity Tweak Tool

<!--more-->

### 安装主题

* numix

        sudo add-apt-repository ppa:numix/ppa
        sudo apt-get update && sudo apt-get install numix-gtk-theme

* paper

        sudo add-apt-repository ppa:snwh/pulp
        sudo apt-get update
        sudo apt-get install paper-icon-theme
        sudo apt-get install paper-gtk-theme
        sudo apt-get install paper-cursor-theme

* Yosembiance

        sudo apt-add-repository ppa:bsundman/themes
        sudo apt-get update
        sudo apt-get install yosembiance-theme

### 安装图标

* numix

        sudo apt-add-repository ppa:numix/ppa
        sudo apt-get update
        sudo apt-get install numix-icon-theme-circle
        sudo apt-get install numix-gtk-theme numix-icon-theme numix-icon-theme-circle numix-wallpaper-saucy

* moka/feba

        sudo add-apt-repository ppa:moka/daily
        sudo apt-get update

  sudo apt-get install faba-icon-theme faba-mono-icons

* elementary

        sudo add-apt-repository ppa:noobslab/icons
        sudo apt-get update
        sudo apt-get install elementary-icons

**注：如果提示没有这个包，可以搜索相关关键词，下载相应的包，然后创建并放入相应的文件夹**

        mkdir ~/.themes
        mkdir ~/.icons

通过这种方式建立的是当前用户下的隐藏文件夹，`ctrl+h`显示隐藏文件夹，或者直接放入`/user/share/themes`文件夹

### 安装小工具

* 系统性能监视

        sudo apt-get install indicator-multiload

* 在某一文件目录下右键打开终端

        sudo apt-get install nautilus-open-terminal
        nautilus -q
        # 重新加载文件管理器
