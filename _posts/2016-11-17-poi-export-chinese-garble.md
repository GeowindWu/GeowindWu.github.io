---
layout: post
title: 解决POI导出Excel文件无法显示中文的问题
description: 以中文名命名,不会显示,只显示非中文字符
keyword: POI,Excel,乱码,不显示
---

### 就两招

*对需要输出的中文名以gbk解码,以ISO重新编码
>name=new String(name.getBytes("gbk"),"ISO-8859-1");

*设置response的编码格式
response.setContentType("application/x-msdownload;charset=GBK");

###完美解决