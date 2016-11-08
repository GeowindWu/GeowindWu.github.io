---
layout: post
title: form表单提交数据有中文乱码解决
description: 前端数据中文乱码问题
keyword: 中文乱码,表单,form,前端
---

	###我的问题是form表单中的method属性写错了,导致form表单没有声明提交方法
	
	
	我是看了一篇文章,才意识到这个问题
	
	[form表单提交中文乱码的详细解析](http://blog.csdn.net/jixinhuluwa/article/details/51496183)
	
	里面提到,前端中文乱码是由于*处理方式没选对引起*,一言惊醒梦中人,我果断意识到我是不是我表单的
	提交方法没写,然后一看,是写错了,才解决了这个问题
	
	感谢!
