---
layout: post
title: SQL反模式--包闭表
date: 2017-4-28
categories: 数据库
---

### 评论系统的数据库设计

最好的方法才是采用包闭表的设计模式

评论的表结构如下

CREATE TABLE Comments (
	comment_id SERIAL PRIMARY KEY,
	bug_id BIGINT UNSIGNED NOT NULL,
	author BIGINT UNSIGNED NOT NULL,
	comment_date DATETIME NOT NULL,
	comment TEXT NOT NULL,
	FOREIGN KEY (bug_id) REFERENCES Bugs(bug_id),
	FOREIGN KEY (author) REFERENCES Accounts(account_id)
);

引入包闭表
CREATE TABLE TreePaths (
	ancestor BIGINT UNSIGNED NOT NULL,
	descendant BIGINT UNSIGNED NOT NULL,
	PRIMARY KEY(ancestor, descendant),
	FOREIGN KEY (ancestor) REFERENCES Comments(comment_id),
	FOREIGN KEY (descendant) REFERENCES Comments(comment_id)
);

数据库E-R图
![closure table illustration](/images/170428.png)


新增评论的SQL语句

	假设新增评论id: 2,跟随的评论id是1,那么就是
insert into TreePaths(ancestor,descendant)
	select t.ancestor,2
	from TreePaths as t
	where t.descendant = 1
	union 
	select 2,2;
	
	
删除一个叶子节点(最后的评论)
	假设id=3;
delete from TreePaths where descendant = 3

删除一个子树
	
	delete from TreePaths 
	where descendant in 
	(select descendant from TreePaths where ancestor = id)

移动一个子树
  假设移动的是E-R图中的#6评论的子树到#3评论
	
	-先删除与子树的非直系关系(路径)
	
	取祖先为6的节点的后代,取后代为6的节点的祖先,再剔除(6,6)本身,进行乘积运算,得到的行就是要删掉的
	
	delete from TreePaths 
	where ancestor in (select ancestor		
						from TreePaths
						where descendant = 6)
		   And
		   descendant in (select descendant 
						   from TreePaths			
						   where ancestor = 6);
		   
	经过这个删除,子树就孤立出来了,但还是在表中,只是他们的数学逻辑关系没了,就是path,路径没有了
	
	孤立出来后,既然没有数学关系,那么给它加入关系,连接到评论#3去
	
	使用笛卡尔积运算,选出合适的关系,插入
	
	insert into TreePaths (ancestor,descendant)
		select t.ancestor, subT.descendant
		from TreePaths as t
		cross join TreePaths subT
		where t.descendant = 3
				And
			   subT.ancestor = 6;
			   
			   
			   
### 不同数据库语法会不一样,以上SQL语句不能完美移植,这里只讲例子,把原理讲了,实际使用做相应修改即可,也算是做个笔记
### 这里讲的也不够清楚,只大概讲了实现方式,详细看<<SQL反模式>>这本书
### 这个模式不仅仅用在评论系统上,在员工组织架构,产品分类等上很有用处
