---
layout: post
title: 部署maven项目到tomcat
---

1、环境如下

　　eclipse、apache-maven-3.0.5、apache-tomcat-7.0.39

 

2、配置如下

　　apache-tomcat-7.0.39配置C:\Program Files\apache-tomcat-7.0.39\conf\tomcat-users.xml，因为tomcat7默认情况下没有配置manager访问权限，所以这里需要在tomcat-users.xml加入用户以及权限

复制代码
<tomcat-users>


    <role rolename="admin-gui"/>
    <role rolename="admin-script"/>
    <role rolename="manager-gui"/>
    <role rolename="manager-script"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-script,admin-gui"/>

</tomcat-users>
复制代码
　　apache-maven-3.0.5配置C:\Program Files\apache-maven-3.0.5\conf\settings.xml，为了让maven可以访问tomcat的权限，所以需要把如上创建的用户添加到settings.xml中，如下

复制代码
<servers>

    <!-- 配置tomcat-/manager/text 访问权限 -->
    <server>
      <id>tomcat</id>
      <username>admin</username>
      <password>admin</password>
    </server>

  </servers>
复制代码
　　工程目录下的pom.xml文件，加入build，并配置tomcat7的maven插件，如下配置

复制代码
<build>
        <finalName>myApp</finalName>
        <!-- directory缺省情况下指向target --> 
        <!--<directory>${basedir}/target</directory>-->
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <url>http://localhost:8080/manager/text</url>
                    <!-- server、username、password对应maven的setting下的配置 -->
                    <server>tomcat</server>
                    <username>admin</username>
                    <password>admin</password>
                    <path>/${project.build.finalName}</path>
                    <!-- war文件路径缺省情况下指向target -->
                    <!--<warFile>${basedir}/target/${project.build.finalName}.war</warFile>-->
                </configuration>
            </plugin>
        </plugins>
    </build>
复制代码
　　${project.build.finalName}这个是根据xml的路径来标记的

 

3、命令部署

　　在部署之前，必须先启动tomcat7服务，C:\Program Files\apache-tomcat-7.0.39\bin\startup.bat

　　找到要部署的工程文件根目录下，执行如下maven命令

　　> mvn clean:install           　　//clean是清理输出文件，install编译打包，在每次打包之前必须执行clean，才能保证发布为最新文件

　　> mvn tomcat7:redeploy  　　//第一次发布 tomcat7:deploy，再次发布 tomcat7:redeploy

---- 注意，如果是在myeclipse的里面部署到tomcat，就执行mvn tomcat7:redeploy.（可以认为是开发模式，因为需要在myeclipse里面debug）
	如果是采用把项目打成war包，放到tomcat的webapps目录下的方式，就执行 mvn tomcat7:run-war 就行了。
