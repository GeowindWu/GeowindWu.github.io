---
layout: post
title: ʹ��Hibernate������Զ�ʵ���ϵʱ���ֵ�����
---

����������������������a,b���м��a_b,�м���¼a ��������b����������a_b�������a��b��������Լ����cascade��������ɾ��b��ļ�¼���м��ļ�¼��ȻҲɾ������������Ȼ��a��ļ�¼Ҳɾ����

ԭ��ע�����õ����⣬����һ��ʵ�����˼�Լ���������Ҹ�����ʵ�嶼���ϵ�Լ�������£��Ϳ�����
@ManyToMany(cascade=CascadeType.ALL,fetch = FetchType.EAGER)
	@JoinTable(name = "api_role", joinColumns = @JoinColumn(name = "role_id"), inverseJoinColumns = @JoinColumn(name = "api_id"))