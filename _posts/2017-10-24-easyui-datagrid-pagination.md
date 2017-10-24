---
layout: post
title: 关于EeasyUI datagtid 分页实现
---

一些基本的知识参考官网          http://www.jeasyui.com/documentation/index.php#


这里我强调二点

第一点
datagrid会向后台传递 


rows（每一页展示多少条数据），

page（第几页）


这两个数据，而在文档中没有指出来 ，这也是我今天弄了很久的原因（看别人写的代码发现的这一点）


第二点
后台传json数据时也要按照datagrid的数据格式，另外分页有两个数据，文档也没有指出来

 total键 存放总记录数
rows键 存放每页记录


有了这两点，分页就比较容易实现 了

一般按其默认的方法就行

