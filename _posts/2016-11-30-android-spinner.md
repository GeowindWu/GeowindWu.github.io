---
layout: post
title: 彻底解决spinner点击相同item时不触发事件的办法
---

正确的办法就是换一个监听方法,用这个

	// 从这个方法去改,因为每次点击都会触发这个方法,从这里入手,可以每次都执行相应的事件
	spinner.setOnHierarchyChangeListener(this);
	
详细看这个文章 [Android中Spinner控件关于二次点击同一item无响应事件解析及处理方法](http://blog.csdn.net/yiming_8988/article/details/51512153)

很厉害,自己也要学会看源码,从源码去分析