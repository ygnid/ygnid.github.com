---
layout: post
title: "实现Putty自动登录SSH"
date: 2012-11-07 17:28
comments: true
categories: putty, SSH
---

网上给出的教程大多数都不能用，原因是PuttyGen和OpenSSH使用的Key Pair格式不匹配。经过漫长的google，终于找到一个解决方案如下。本方法参考[这里](http://www.walkernews.net/2009/03/22/how-to-fix-server-refused-our-key-error-that-caused-by-putty-generated-rsa-public-key/)

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