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

2012-11-12 晴
===
上周五一到公司，就发现笔记本电脑坏了，开机就报错“FAN ERROR”，风扇硬件故障。这导致我前几天都无法更新这个Blog，看来这是Octopress blog系统的一个问题，即无法简单的在其他电脑上更新内容。

上周完成了基于NCL的CSV文件读取以及WRF预报结果按预报时段分类的脚本。其主要思想是依次读取每天的预报文件，找出对应预报时段的数据，再存放至统一的变量中。其中主要的技术点有如下几个：

1. **NCL读取ASCII文件**：可用readAsciiTable()加上str_get_field()完成。用前者可将整个文件存入一个string数组内，再用后者将想要的column提取出来。例如：

		lines = readAsciiTable(csvFile,1,"string",1)
		delim = ","
		column_1 = strip_quotes ( ndtooned (str_get_field(lines, 1, delim)))

2. **查找数组成员的序号**：可用ind_nearest_coord()。如果想要数组的一个连续片段，可以用coordinate subscripting的方式。

3. **NCL格式化输出TXT文本**：以前用的是老土的循环生成各行string的方式，这次发现一个简单方法，即将*格式string*和*内容string*（通常是将数组用sprintf()或tostring()转化而来的string数组）直接用“+”连接起来。例如：

		printArray = ( string1 + ", " + string2 \
		                            + ", "+ sprintf("%6.2f", float_array1) + ", "+ sprinti("%3i", int_array1) \
		                            + ", "+ sprintf("%6.2f", float_array2) + ", "+ sprinti("%3i", int_array2))


2012-11-13 晴
===
今天找了TM帮忙处理GIS文件提取边界的问题。

### 改造wrf\_user\_getvar()

对wrf\_user\_getvar()进行了改造，以支持读取特定区域和高度范围的WRF结果，其优点是对大文件的读取速度有显著地提升。

2012-11-14 晴
===

### 升级NCL版本到V6.1.0

### 完成了对WRF站点数据的快速读取
使用昨天编写的改进版wrf\_user\_getvar()，实现了对WRF站点数据（主要是风速）的快速读取。


2012-11-15 晴
===

### 用NCL实现了判断一个点是否在多边形内的算法

参考了某个著名的算法，将其在NCL语言中实现。

2012-11-28 晴
==
时隔几个月之后，今天又一次骑车上班了，感觉体力明显比不上以前，看来要坚持锻炼啊。

前几天发现NCL的wrf\_user\_ll\_to\_ij()和wrf\_user\_ij\_to\_ll()两个函数算出的结果与直接获取wrfout文件XLAT、XLONG数据不一致。用wrf\_user\_ll\_to\_ij()得到i、j，比XLAT/XLONG对应的i、j index要大1。例如，用(i=0, j=0)位置的XLAT和XLONG带入wrf\_user\_ll\_to\_ij()，得到的结果是(1,1)。因此，为了不产生错位，在调用这两个函数时，需要加上相应的偏离值(即offset)，代码如下：

	loc = wrf_user_ll_to_ij(a, lon, lat, True) - 1
	ll  = wrf_user_ij_to_ll(a, i+1, j+1, True)