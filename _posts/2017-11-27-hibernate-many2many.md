---
layout: post
title: ʹ��Hibernate������Զ�ʵ���ϵʱ���ֵ�����
---

����������������������a,b���м��a_b,�м���¼a ��������b����������a_b�������a��b��������Լ����cascade��������ɾ��b��ļ�¼���м��ļ�¼��ȻҲɾ������������Ȼ��a��ļ�¼Ҳɾ����

ԭ��ע�����õ����⣬����Ϊ������������cascadeԼ���������Ҹ�����ʵ�嶼ȥ��Լ�������£��Ϳ�����
ԭ����

@ManyToMany(cascade=CascadeType.ALL,fetch = FetchType.EAGER)
@JoinTable(name = "api_role", joinColumns = @JoinColumn(name = "role_id"), inverseJoinColumns = @JoinColumn(name = "api_id"))
	
�޸ĺ�
@ManyToMany
@JoinTable(name = "api_role", joinColumns = @JoinColumn(name = "role_id"), inverseJoinColumns = @JoinColumn(name = "api_id"))
