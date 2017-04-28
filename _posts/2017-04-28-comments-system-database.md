---
layout: post
title: SQL��ģʽ--���ձ�
---

### ����ϵͳ�����ݿ����

��õķ������ǲ��ð��ձ�����ģʽ

���۵ı�ṹ����

CREATE TABLE Comments (
	comment_id SERIAL PRIMARY KEY,
	bug_id BIGINT UNSIGNED NOT NULL,
	author BIGINT UNSIGNED NOT NULL,
	comment_date DATETIME NOT NULL,
	comment TEXT NOT NULL,
	FOREIGN KEY (bug_id) REFERENCES Bugs(bug_id),
	FOREIGN KEY (author) REFERENCES Accounts(account_id)
);

������ձ�
CREATE TABLE TreePaths (
	ancestor BIGINT UNSIGNED NOT NULL,
	descendant BIGINT UNSIGNED NOT NULL,
	PRIMARY KEY(ancestor, descendant),
	FOREIGN KEY (ancestor) REFERENCES Comments(comment_id),
	FOREIGN KEY (descendant) REFERENCES Comments(comment_id)
);

���ݿ�E-Rͼ
![closure table illustration](/images/170428.png)


�������۵�SQL���

	������������id: 2,���������id��1,��ô����
insert into TreePaths(ancestor,descendant)
	select t.ancestor,2
	from TreePaths as t
	where t.descendant = 1
	union 
	select 2,2;
	
	
ɾ��һ��Ҷ�ӽڵ�(��������)
	����id=3;
delete from TreePaths where descendant = 3

ɾ��һ������
	
	delete from TreePaths 
	where descendant in 
	(select descendant from TreePaths where ancestor = id)

�ƶ�һ������
  �����ƶ�����E-Rͼ�е�#6���۵�������#3����
	
	-��ɾ���������ķ�ֱϵ��ϵ(·��)
	
	ȡ����Ϊ6�Ľڵ�ĺ��,ȡ���Ϊ6�Ľڵ������,���޳�(6,6)����,���г˻�����,�õ����о���Ҫɾ����
	
	delete from TreePaths 
	where ancestor in (select ancestor		
						from TreePaths
						where descendant = 6)
		   And
		   descendant in (select descendant 
						   from TreePaths			
						   where ancestor = 6);
		   
	�������ɾ��,�����͹���������,�������ڱ���,ֻ�����ǵ���ѧ�߼���ϵû��,����path,·��û����
	
	����������,��Ȼû����ѧ��ϵ,��ô���������ϵ,���ӵ�����#3ȥ
	
	ʹ�õѿ���������,ѡ�����ʵĹ�ϵ,����
	
	insert into TreePaths (ancestor,descendant)
		select t.ancestor, subT.descendant
		from TreePaths as t
		cross join TreePaths subT
		where t.descendant = 3
				And
			   subT.ancestor = 6;
			   
			   
			   
### ��ͬ���ݿ��﷨�᲻һ��,����SQL��䲻��������ֲ,����ֻ������,��ԭ����,ʵ��ʹ������Ӧ�޸ļ���,Ҳ���������ʼ�
### ���ｲ��Ҳ�������,ֻ��Ž���ʵ�ַ�ʽ,��ϸ��<<SQL��ģʽ>>�Ȿ��
### ���ģʽ��������������ϵͳ��,��Ա����֯�ܹ�,��Ʒ������Ϻ����ô�
