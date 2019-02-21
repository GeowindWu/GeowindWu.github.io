---
layout: post
title: 搭建FTP服务
---
??一般在各种linux的发行版中，默认带有的ftp软件是vsftp，从各个linux发行版对vsftp的认可可以看出，vsftp应该是一款不错的ftp软件。

1、检查安装vsftpd软件

使用如下命令#

rpm -qa |grep vsftpd
1
??可以检测出是否安装了vsftpd软件，如果没有安装，使用YUM命令进行安装

yum install vsftpd -y
1
2、启动服务

使用vsftpd软件，主要包括如下几个命令：

启动ftp命令
#service vsftpd start
停止ftp命令
#service vsftpd stop
重启ftp命
#service vsftpd restart
1
2
3
4
5
6
3、vsftpd的配置

??ftp的配置文件主要有三个，位于/etc/vsftpd/目录下，分别是：

ftpusers 该文件用来指定那些用户不能访问ftp服务器。黑名单
user_list 该文件用来指示的默认账户在默认情况下也能访问ftp，白名单
vsftpd.conf vsftpd的主配置文件
4、以匿名用户登录

我们去掉配置文件vsftpd.conf 里面以下

anon_upload_enable=YES
anon_mkdir_write_enable=YES
1
2
两项前面的#号，就可以完成匿名用户的配置，此时匿名用户既可以登录上传、下载文件。记得修改配置文件后需要重启服务。

5、非匿名账户的创建与使用

vsftpd服务与系统用户是相互关联的，例如我们创建一个名为testwww

#useradd testwww
#passwd testwww
1
2
6、登录方式（非vsftp机器）

浏览器打开 ： 
浏览器上输入
ftp://vsftp所在机器ip/
![0](http://img.blog.csdn.net/20160919114947338)
文件打开 ： 
文件夹输入
ftp://vsftp所在机器ip/ ；
 右键可以选择登录

![1](http://img.blog.csdn.net/20160919115145245)

cmd ： 
dos中输入
ftp vsftp所在机器ip  
 输入用户名，密码

![2](http://img.blog.csdn.net/20160919115740762)

xftp登录：



小细节

默认sftp可以登录，但是ftp不能登录；需要在 
vsftpd.conf加入ftp的默认端口(sftp 默认端口22)。

listen_port=21
![3](http://img.blog.csdn.net/20160919115856329)