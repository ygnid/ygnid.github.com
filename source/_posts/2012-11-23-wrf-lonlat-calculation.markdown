---
layout: post
title: "WRF lon/lat calculation in NCL"
date: 2012-11-28 17:00
comments: true
categories: NCL, WRF
---

前几天发现NCL的wrf\_user\_ll\_to\_ij()和wrf\_user\_ij\_to\_ll()两个函数算出的结果与直接获取wrfout文件XLAT、XLONG数据不一致。用wrf\_user\_ll\_to\_ij()得到i、j，比XLAT/XLONG对应的i、j index要大1。例如，用(i=0, j=0)位置的XLAT和XLONG带入wrf\_user\_ll\_to\_ij()，得到的结果是(1,1)。因此，为了不产生错位，在调用这两个函数时，需要加上相应的偏离值(即offset)，代码如下：

	loc = wrf_user_ll_to_ij(a, lon, lat, True) - 1
	ll  = wrf_user_ij_to_ll(a, i+1, j+1, True)