---
layout: post
title: MySql安装配置问题
---

先说它的配置文件.ini,如果直接修改不行,是权限问题.那里是系统文件,
也不能新建,可以从其他地方弄好再复制过来.

还有一个是执行安装命令的时候,需要到bin目录下安装,否则后面启动会有问题.
如果已经安装了就
 先卸载 
 mysqld -remove    
 到了指定目录后
 cd 目录 
 再安装
 mysqld -install  
 再启动 net start mysql