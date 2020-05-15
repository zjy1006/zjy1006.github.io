---
title: vagrant和VirtualBox
layout: post
tags:
  - 虚拟机
categories:
  - 虚拟机
cover: 'https://cdn.jsdelivr.net/gh/zjy1006/source/img/jokermen.png'
comments: false
abbrlink: 3849af02
date: 2020-04-07 08:09:19
updated:
---

# [Vagrant](https://www.vagrantup.com/)和[VirtualBox](https://www.virtualbox.org/)

## Vagrant

Vagrant是一个基于[Ruby](https://baike.baidu.com/item/Ruby/11419)的工具，用于创建和部署虚拟化开发环境。它 使用Oracle的开源VirtualBox虚拟化系统，使用 Chef创建自动化虚拟环境。

拿VirtualBox举例，VirtualBox会开放一个创建虚拟机的接口，Vagrant会利用这个接口创建虚拟机，并且通过Vagrant来管理，配置和自动安装虚拟机。

## VirtualBox

VirtualBox 是一款开源[虚拟机软件](https://baike.baidu.com/item/虚拟机软件/9003764)。VirtualBox 是由德国 Innotek 公司开发，由[Sun](https://baike.baidu.com/item/Sun/69463) Microsystems公司出品的软件，使用[Qt](https://baike.baidu.com/item/Qt)编写，在 Sun 被 [Oracle](https://baike.baidu.com/item/Oracle) 收购后正式更名成 Oracle VM VirtualBox。Innotek 以 GNU General Public License (GPL) 释出 VirtualBox，并提供二进制版本及 OSE 版本的代码。使用者可以在VirtualBox上安装并且执行[Solaris](https://baike.baidu.com/item/Solaris)、[Windows](https://baike.baidu.com/item/Windows)、[DOS](https://baike.baidu.com/item/DOS/32025)、[Linux](https://baike.baidu.com/item/Linux)、OS/2 Warp、[BSD](https://baike.baidu.com/item/BSD)等系统作为客户端操作系统。已由[甲骨文公司](https://baike.baidu.com/item/甲骨文公司/430115)进行开发，是甲骨文公司xVM虚拟化平台技术的一部份。

# 我的入门

## 下载与安装

1. [vagrant官方镜像仓库](https://app.vagrantup.com/boxes/search)
2. [到官网下载Vagrant](https://www.vagrantup.com/)，下载好了一路安装即可（当然也可以选择安装其他盘，默认是c盘）。安装完重启电脑即可
   1. 验证是否安装成功：在cmd窗口输入：vagrant
   2. 如果出现命令行提示，证明安装成功。
3. 再去下载[VirtualBox](https://www.virtualbox.org/)，同样是一路安装即可。

## 用Vagrant给VirtualBox创建虚拟机

1. 只需两行简单命令：

   ```
   $ vagrant init centos/7
   $ vagrant up
   ```
   1. 在用户目录下初始化一个 Vagrantfile 的配置文件![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200515230949.png)
   2. 接下来再用第二行命令：如果没有镜像会进行下载centos/7，并且会挂载到VirtualBox上并启动。下载速度慢可以用迅雷。

2. 连接虚拟机用

   ```
   $ vagrant ssh
   ```

3. 退出虚拟机用

   ```
   exit
   ```

## 虚拟机网络设置

![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200515232502.png)



![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200515232940.png)

什么是网络地址转换-端口转发

![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200515233106.png)

- 所以我们希望可以给虚拟机一个固定的ip地址，可以跟我们的电脑进行ping通，这样的话我们虚拟机当中装好一个软件，我们直接那它的ip地址访问就可以了。
- 因为我们用的是vagrant创建的虚拟机。有着vagrantfile的配置文件。就可以直接改固定的ip地址。当然也可以改虚拟机的网卡设置（太麻烦）
  - ![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200515233827.png)
  - ![](https://gitee.com/zhou_jin_yuan/blogimage/raw/master/img/20200515233959.png)
- 修改好了重新启动虚拟机即可。
  - 查看ip用：ip addr
  - 再相互ping一下



