---
layout:post
title: 记OpenVPN For Android 
description: 公司为了我一个任务,在一个已经集成了OpenVPN的项目上加个功能,于是给了我代码,今天终于搞定了,讲道理,看代码的时间花了很长,现在终于算是基本懂了openVPN for android 的代码,但openVPN的相关知识还不太懂,有时间补一下
keyword:vpn,openVPN,Android
---
## 先讲讲openVPN源码的几个比较重要的类,也是我之前看代码时候做的笔记

	*OpenVPNService*   打开VPN的服务类
	> 此类继承于系统api: VpnService
	> 打开VPN的代码会new 一个OpenVPNManagerMentThread,启动VPN
	> 此服务是真正的VPN服务,可有配置文件看出,根据官方文档,intent-filter必须
	> 声明为android.net.VpnService,也必须要有
		android:permission="android.permission.BIND_VPN_SERVICE"
	> 权限,写在<sercive>标签内
	
	![VpnService声明](/images/2016-11/10-01.png)


	*OpenVpnManagementThread*  是VPN的直接管理类 ,openVPN的关键知识都在这个类里面
	> 这是线程,启动线程,其中的run方法就是开启VPN
	> 关闭线程就是关闭VPN
	> 这线程扫了一下,是socket,估计是TCP协议,长连接,是属于服务器端

	*ProfileManager* 配置文件管理
	> 载入VPN列表 loadVPNList
	> 保存在profiles中
	> 通过get(String key)方法获取相应的VpnProfile
	> addProfile(),saveProfile()方法会把经解析的配置文件存进去

	------------------------------------------------------------


	
	*无论是连接VPN还是重新连接VPN,都会用一个Intent 目标class是LaunchVPN
	而LaunchVPN是如何启动的呢,是调用系统类VpnService,调用方法prepare()返回一个Intent
	startActivityForResult(intent)就行
	(官方文档指定要用这个方法,怀疑是服务和能不能用startActivity的歇歇了,权威说明就这样用)
	启动activity后,在回调方法中,成功会执行     VPNLaunchHelper.startOpenVpn();*
	
	--------------------------------------------------------------
	
	*VPNLaunchHelper*
	> 此类会的startOpenVPN()会获取一个服务,然后启动服务,服务来自传入的VpnProfile
	> 的prepareStartService()方法
	> prepareStartService()方法返回的OpenVPNService服务的Intent对象

	###看到这里,可能会疑惑了,OpenVPNservice继承自VpnSercice,前面用了VpnService的prepare()然后startActivityForResult,
	###到了这又startService?
	###原因在官方文档可以找到,prepare()的目的是获取授权,在回调方法onActivityResult可知道是否授权成功,
	###成功的话在执行真正的VPN服务就是继承自VpnService的OpenVPNService
	![VPNService步骤](/images/2016-11/10-02.png)
	###刚刚写了那么多只是完成了第一步,然后执行第二步
	
	*注:如果还需要其他用户信息,如用户名和密码,可以在第一步和第二步直接获取,就是授权成功在向用户索要*
	
	*ConfigParser 解析配置文件用的,从文件流中读取配置文件的内容*
	> 经此类解析,生成VPNProfile对象,然后用ProfileManager存起来

	
	###一个android的打开文件的方法,顺便记下来
	![openFile](/images/2016-11/10-03.png)
