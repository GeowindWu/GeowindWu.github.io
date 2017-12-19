---
layout: post
title: ����maven��Ŀ��tomcat
---

1����������

����eclipse��apache-maven-3.0.5��apache-tomcat-7.0.39

 

2����������

����apache-tomcat-7.0.39����C:\Program Files\apache-tomcat-7.0.39\conf\tomcat-users.xml����Ϊtomcat7Ĭ�������û������manager����Ȩ�ޣ�����������Ҫ��tomcat-users.xml�����û��Լ�Ȩ��

���ƴ���
<tomcat-users>


    <role rolename="admin-gui"/>
    <role rolename="admin-script"/>
    <role rolename="manager-gui"/>
    <role rolename="manager-script"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-script,admin-gui"/>

</tomcat-users>
���ƴ���
����apache-maven-3.0.5����C:\Program Files\apache-maven-3.0.5\conf\settings.xml��Ϊ����maven���Է���tomcat��Ȩ�ޣ�������Ҫ�����ϴ������û���ӵ�settings.xml�У�����

���ƴ���
<servers>

    <!-- ����tomcat-/manager/text ����Ȩ�� -->
    <server>
      <id>tomcat</id>
      <username>admin</username>
      <password>admin</password>
    </server>

  </servers>
���ƴ���
��������Ŀ¼�µ�pom.xml�ļ�������build��������tomcat7��maven�������������

���ƴ���
<build>
        <finalName>myApp</finalName>
        <!-- directoryȱʡ�����ָ��target --> 
        <!--<directory>${basedir}/target</directory>-->
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <url>http://localhost:8080/manager/text</url>
                    <!-- server��username��password��Ӧmaven��setting�µ����� -->
                    <server>tomcat</server>
                    <username>admin</username>
                    <password>admin</password>
                    <path>/${project.build.finalName}</path>
                    <!-- war�ļ�·��ȱʡ�����ָ��target -->
                    <!--<warFile>${basedir}/target/${project.build.finalName}.war</warFile>-->
                </configuration>
            </plugin>
        </plugins>
    </build>
���ƴ���
����${project.build.finalName}����Ǹ���xml��·������ǵ�

 

3�������

�����ڲ���֮ǰ������������tomcat7����C:\Program Files\apache-tomcat-7.0.39\bin\startup.bat

�����ҵ�Ҫ����Ĺ����ļ���Ŀ¼�£�ִ������maven����

����> mvn clean:install           ����//clean����������ļ���install����������ÿ�δ��֮ǰ����ִ��clean�����ܱ�֤����Ϊ�����ļ�

����> mvn tomcat7:redeploy  ����//��һ�η��� tomcat7:deploy���ٴη��� tomcat7:redeploy

---- ע�⣬�������myeclipse�����沿��tomcat����ִ��mvn tomcat7:redeploy.��������Ϊ�ǿ���ģʽ����Ϊ��Ҫ��myeclipse����debug��
	����ǲ��ð���Ŀ���war�����ŵ�tomcat��webappsĿ¼�µķ�ʽ����ִ�� mvn tomcat7:run-war �����ˡ�
