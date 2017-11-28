---
layout: post
title: 使用Hibernate建立多对多实体关系时出现的问题
---

问题描述：我现在两个表a,b，中间表a_b,中间表记录a 的主键和b的主键，表a_b的外键是a，b的主键，约束的cascade，我现在删除b表的记录，中间表的记录自然也删除，但是它居然连a表的记录也删除了

原因，注解配置的问题，是因为都加上了联级cascade约束，后来我给两个实体都去掉约束，如下，就可以了
原来：

@ManyToMany(cascade=CascadeType.ALL,fetch = FetchType.EAGER)
@JoinTable(name = "api_role", joinColumns = @JoinColumn(name = "role_id"), inverseJoinColumns = @JoinColumn(name = "api_id"))
	
修改后
@ManyToMany
@JoinTable(name = "api_role", joinColumns = @JoinColumn(name = "role_id"), inverseJoinColumns = @JoinColumn(name = "api_id"))
