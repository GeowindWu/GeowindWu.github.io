---
layout: post
title:Tomcat配置SSL--生成服务端客户端安全证书
---


在Tomcat 6中配置SSL双向认证是相当容易的，本文将介绍如何使用JDK的keytool来为Tomcat配置双向SSL认证。并实现批量生成证书 系统需求：JDK 5.0
Tomcat 6.0.16
启动命令行：
###第一步：为服务器生成证书
使用keytool为Tomcat生成证书，假定目标机器的域名是localhost或者“192.168.1.1”，keystore文件存放在“D:/downloads/tomcat.keystore”，口令为“logiscn”，使用如下命令生成：
 

    keytool -genkey -v -alias tomcat -keyalg RSA -keystore D:/downloads/tomcat.keystore -dname "CN=192.168.1.1,OU=logiscn,O=logis,L=beijing,ST=beijing,C=CN" -validity 3650 -storepass logiscn -keypass logiscn 

 
如果Tomcat所在服务器的域名不是“localhost”，应改为对应的域名，如[url]www.baidu.com[/url] 或者IP地址，否则浏览器会弹出警告窗口，提示用户证书与所在域不匹配。 
###第二步：为客户端生成证书
下一步是为浏览器生成证书，以便让服务器来验证它。假设文件存放在D:/downloads/p12/tianli.p12，为了能将证书顺利导入至IE和Firefox，证书格式应该是PKCS12，因此，使用如下命令生成：

    keytool -genkey -v -alias tianli -keyalg RSA -storetype PKCS12 -keystore D:/downloads/p12/tianli.p12 -dname "CN=tianli,OU=logiscn,O=logis,L=beijing,ST=beijing,C=CN" -validity 3650 -storepass tianli -keypass tianli" 

-validity为有效期限，目前的设置为10年，keypass用于在导入浏览器时使用的密码，如果密码不正确，则不能正确导入到浏览器。
对应的证书库存放在“D:/downloads/p12/tianli.p12”，客户端的CN可以是任意值。
###第三步：让服务器信任客户端证书

由于是双向SSL认证，服务器必须要信任客户端证书，因此，必须把客户端证书添加为服务器的信任认证。由于不能直接将PKCS12格式的证书库导入，我们必须先把客户端证书导出为一个单独的CER文件，使用如下命令：

    keytool -export -alias tianli -keystore  D:/downloads/p12/tianli.p12 -storetype PKCS12 -storepass tianli -rfc -file D:/downloads/cert/tianli.cer

 

通过以上命令，客户端证书就被我们导出到“D:/downloads/cert/tianli.cer r”文件了。下一步，是将该文件导入到服务器的证书库，添加为一个信任证书：
 

    keytool -import -alias tianli -v -file D:/downloads/cert/tianli.cer -keystore D:/downloads/tomcat.keystore -storepass logiscn <myint.inf

由于在导入的过程中需要输入Y或者n在此处直接使用一个文件myint.inf代替输入，myint.inf是一个文本文件，里面的内容只有y和一个回车 
通过list命令查看服务器的证书库，我们可以看到两个输入，一个是服务器证书，一个是受信任的客户端证书： 
###第四步：配置Tomcat服务器

打开Tomcat根目录下的/conf/server.xml，找到如下配置段，修改如下：
打开注释

    <Connector port="443" protocol="HTTP/1.1" SSLEnabled="true"
        maxThreads="150" scheme="https" secure="true"
        clientAuth="true" sslProtocol="TLS"
        keystoreFile=" D:/downloads/tomcat.keystore " keystorePass="logiscn "
        truststoreFile=" D:/downloads/tomcat.keystore " truststorePass="logiscn "
    />

其中，clientAuth指定是否需要验证客户端证书，如果该设置为“false”，则为单向SSL验证，SSL配置可到此结束。如果clientAuth设置为“true”，表示强制双向SSL验证，必须验证客户端证书。如果clientAuth设置为“want”，则表示可以验证客户端证书，但如果客户端没有有效证书，也不强制验证。
###第五步：导入客户端证书
如果设置了clientAuth="true"，则需要强制验证客户端证书。双击“D:/downloads/p12/tianli.p12”即可将证书导入至IE：导入证书后，即可启动Tomcat，用IE进行访问。输入[url]https://IPAdress/[/url]      ，https协议默认的访问端口为443。以上所写大都为借鉴网上的资料。
为了实现每人发放一个证书，如果重复以上的操作也可以达到目的，考虑到需要进行大量的测试，并且在不同的机器上部署，就想到使用程序自动生成命令的方法。
生成命令的程序是使用java 写的，生成命令需要预先设置如下的几项：

 1. Basedir生成的命令文件的位置，生成的命令运行后生成cer和p12格式的文件，为了区分存放，需要建立两个文件夹，因此需要与一个基本目
 2. 生成的keyStore文件需要一个密码，为了安全起见，不同的域名的keyStore需要不同的密码。
 3. 域地址，如果域地址不正确，则会在整数上发出警告。因此对于不同的域，地址是不同的。


完成以上的三个设置之后就可以生成命令了。生成的文件包括3个，全部存放在Basedir下。
1．  Myint.inf文件，仅仅用于输入内容很简单包括y 和一个回车
2．  Conf的文件，里面包括了生成的配置文件片段和一段简单的使用说明，内容如下

    <Connector port="443" protocol="HTTP/1.1" SSLEnabled="true"
           maxThreads="150" scheme="https" secure="true"
           clientAuth="true" sslProtocol="TLS"
           keystoreFile="D:/downloads/tomcat.keystore"
            keystorePass="logiscn"
           truststoreFile="D:/downloads/tomcat.keystore"
            truststorePass="logiscn"/>

使用的时候直接复制到相应的server.xml中
3．  可执行的命令文件command.bat，执行上述命令之前，需要建立两个文件夹，以便于把生成的文件存放到合适的位置，部分代码如下

    mkdir cert
    mkdir p12
    keytool -genkey -v -alias tomcat -keyalg RSA -keystore D:/downloads/tomcat.keystore -dname "CN=localhost,OU=logiscn,O=logis,L=beijing,ST=beijing,C=CN" -validity 3650 -storepass logiscn -keypass logiscn

  rem 为 tianli 生成证书
 rem 第二步：为客户端生成证书
 

    keytool -genkey -v -alias tianli -keyalg RSA -storetype PKCS12 -keystore D:/downloads/p12/tianli.p12 -dname "CN=tianli,OU=logiscn,O=logis,L=beijing,ST=beijing,C=CN" -validity 3650 -storepass tianli -keypass tianli"

 rem 第三步：让服务器信任客户端证书

     keytool -export -alias tianli -keystore  D:/downloads/p12/tianli.p12 -storetype PKCS12 -storepass tianli -rfc -file D:/downloads/cert/tianli.cer"
     keytool -import -alias tianli -v -file D:/downloads/cert/tianli.cer -keystore D:/downloads/tomcat.keystore -storepass logiscn <myint.inf

Java程序的实现见附件，这样双击执行程序就可以批量生成证书。相当方便。