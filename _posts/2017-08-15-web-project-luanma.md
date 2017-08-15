---
layout: post
title: web项目中文乱码问题终极解决方案
---
**jsp页面**
 一定要带上 <%@ page language="java" contentType="text/html; charset=GBK" pageEncoding="UTF-8"%>  
 
 
 **servlet**
 request response设置编码
 
 **tomcat**
 配置问server.xml 加上URLEncoding="UTF-8"
 
 "针对单个字符如果还乱码"
 可以重新解码再编码  new String(str.getBytes(decode),encode);
 
 **使用的HTTP请求库**
 比例HttpClient，使用HttpPost,在把数据放进entity是，需要设置编码；
 post.setEntity(new UrlEncodedFormEntity(nvps)，encode);
 
 
 **请求头**
 设置Content-Type 时，值都带上编码
 
 Content-Type :  *; charset=utf-8