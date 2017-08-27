---
layout: post
title: 数据库保存emoji表情解决方案
---

    java.sql.SQLException: Incorrect string value: '\xF0\x9F\x92\x94' for colum n 'name' at row 1 
    at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:1073) 
    at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3593) 
    at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3525) 
    at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:1986) 
    at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2140) 
    at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2620) 
    at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1662) 
    at com.mysql.jdbc.StatementImpl.executeUpdate(StatementImpl.java:1581)


使用mysql数据库的时候，如果字符集是UTF-8并且在java服务器上，当存储emoji表情的时候，会抛出以上异常（比如微信开发获取用户昵称，有的用户的昵称用的是emoji的图像）

## 解决方案

### 一.从数据库层面进行解决

#### 1.修改database,table,column字符集

    ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
    ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE table_name CHANGE column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

#### 2.修改mysql配置文件my.cnf（window为my.ini）

    [client]
    default-character-set = utf8mb4
    [mysql]
    default-character-set = utf8mb4
    [mysqld]
    character-set-client-handshake = FALSE
    character-set-server = utf8mb4
    collation-server = utf8mb4_unicode_ci
    init_connect='SET NAMES utf8mb4'

#### 3.用的是java服务器，升级或者确保mysql connection版本高于5.1.13否则仍然不能试用utf8mb4

#### 4.服务器端的db配置文件

    jdbc.driverClassName=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/database?useUnicode=true&characterEncoding=utf8&autoReconnect=true&rewriteBatchedStatements=TRUE
    jdbc.username=root
    jdbc.password=password

如果升级了mysql-connector，其中的characterEncoding=utf8可以自动被识别为utf8mb4（兼容原来的utf8），而
autoReconnection（当数据库连接异常中断时，是否自动重新连接？默认为false）强烈建议配上，忽略这个属性，可能导致缓存缘故 ，
没有读取到DB最新的配置，导致一直无法试用utf8mb4字符集；	
详细可见 ：[mysql/Java服务端对emoji的支持][1]


### 二.从应用层的方面进行解决

在获得数据之后往数据库存之前先进行编码: 

    URLEncoder.encode(nickName, "utf-8");

当从数据库中取出准备显示的时候进行解码， 

    URLDecoder.decode(nickname, "utf-8");
     　
从应用层进行解决的时候建议不要在对象getter，setter方法中直接编码，因为放入对象的时候setter方法将nickname进行编码，当插入数据库的时候相当于从对象中调用getter方法将你参考取出这就将之前setter编码过的nickname又重新解码了，等于未对Nickname进行任何操作。依然会出现以上问题。






  [1]: http://segmentfault.com/a/1190000000616820
  