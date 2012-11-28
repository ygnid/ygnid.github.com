---
layout: post
title: "FreeBSD in VirtualBox"
date: 2012-11-23 17:00
comments: true
categories: FreeBSD
---

前几天在VirtualBox里装了个FreeBSD 9.0-RELEASE amd64，发现以前遇到的问题现在还存在，具体问题如下：

* **启动到GDM后键盘无响应**：这据说是Hal的Bug导致的，解决方案是在xorg.conf里加入以下设置

		<code hear>

* **启动时sendmail提示hostname不能识别**：这是由于/etc/rc.conf设置的hostname不符合规范，正确的写法是xxx.yyy，也就是其中应至少有一个"."。注意，修改了hostname以后，/etc/hosts也要做相应修改。