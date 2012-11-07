---
layout: page
title: "日记"
comments: true
sharing: false
footer: true
---

2012-11-6
====
### WRF预报系统Bug修正

* 修正了读取指定经纬度预报结果的bug
* 修正了业务系统使用wrf.sandbox.sh后造成的bug

### 浦口辐射测量数据与WRF预报值的对比

对比了从11月2号开始的4天，结果出我意料的好，其中一天出现了明显的辐射衰减，WRF的很好的预报出了衰减的大小，预报曲线和观测符合程度相当高。

2012-11-7
====
### 实现Putty自动登录SSH

网上给出的教程大多数都不能用，原因是PuttyGen和OpenSSH使用的Key Pair格式不匹配。经过漫长的google，终于找到一个解决方案如下。本方法参考[这里](http://www.walkernews.net/2009/03/22/how-to-fix-server-refused-our-key-error-that-caused-by-putty-generated-rsa-public-key/)。

主要步骤是：

1. 用puttygen生成private/public key pair
2. 在VIM中修改public key：
	* 首先删除前两行和最后一行
	* 然后用Ctrl-j将中间的几行合并为单行，并删除中间的空格
	* 在行首添加"ssh-rsa "，行尾添加"== username@domain"
	* 保存
3. 将public key上传至SSH Server，添加至~/.ssh/authorized_keys。
4. 在Putty中进行设置，在“SSH-Auth”添加Private Key文件，后缀名是".ppk"。
5. 用Pageant添加key，这样就不用输入sshkey的password。

### 在github.com建立了自己的blog

就是目前这个空间咯。

使用的是[Octopress Blog](http://octopress.org)，一个基于[Yekyll](http://jekyllrb.com/)的Blog框架。

### 书稿修订
将YY的新能源书稿进行了修订，增加了发展历史介绍，以及引用文献。