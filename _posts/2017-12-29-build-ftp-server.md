---
layout: post
title: �FTP����
---
??һ���ڸ���linux�ķ��а��У�Ĭ�ϴ��е�ftp�����vsftp���Ӹ���linux���а��vsftp���Ͽɿ��Կ�����vsftpӦ����һ����ftp�����

1����鰲װvsftpd���

ʹ����������#

rpm -qa |grep vsftpd
1
??���Լ����Ƿ�װ��vsftpd��������û�а�װ��ʹ��YUM������а�װ

yum install vsftpd -y
1
2����������

ʹ��vsftpd�������Ҫ�������¼������

����ftp����
#service vsftpd start
ֹͣftp����
#service vsftpd stop
����ftp��
#service vsftpd restart
1
2
3
4
5
6
3��vsftpd������

??ftp�������ļ���Ҫ��������λ��/etc/vsftpd/Ŀ¼�£��ֱ��ǣ�

ftpusers ���ļ�����ָ����Щ�û����ܷ���ftp��������������
user_list ���ļ�����ָʾ��Ĭ���˻���Ĭ�������Ҳ�ܷ���ftp��������
vsftpd.conf vsftpd���������ļ�
4���������û���¼

����ȥ�������ļ�vsftpd.conf ��������

anon_upload_enable=YES
anon_mkdir_write_enable=YES
1
2
����ǰ���#�ţ��Ϳ�����������û������ã���ʱ�����û��ȿ��Ե�¼�ϴ��������ļ����ǵ��޸������ļ�����Ҫ��������

5���������˻��Ĵ�����ʹ��

vsftpd������ϵͳ�û����໥�����ģ��������Ǵ���һ����Ϊtestwww

#useradd testwww
#passwd testwww
1
2
6����¼��ʽ����vsftp������

������� �� 
�����������
ftp://vsftp���ڻ���ip/
![0](http://img.blog.csdn.net/20160919114947338)
�ļ��� �� 
�ļ�������
ftp://vsftp���ڻ���ip/ ��
 �Ҽ�����ѡ���¼

![1](http://img.blog.csdn.net/20160919115145245)

cmd �� 
dos������
ftp vsftp���ڻ���ip  
 �����û���������

![2](http://img.blog.csdn.net/20160919115740762)

xftp��¼��



Сϸ��

Ĭ��sftp���Ե�¼������ftp���ܵ�¼����Ҫ�� 
vsftpd.conf����ftp��Ĭ�϶˿�(sftp Ĭ�϶˿�22)��

listen_port=21
![3](http://img.blog.csdn.net/20160919115856329)