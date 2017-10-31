---
layout: post
title: easyUI汉化出现中文乱码
---

原因就是汉化包 easyui-lang-zh_CN.js 的编码方式有问题。

解决办法： 复制js内容到notepad++，以utf-8格式编辑，另存为替换原文件；或者直接用notepad++ 打开js，以utf-8格式编码，然后保存，再放到项目中去。