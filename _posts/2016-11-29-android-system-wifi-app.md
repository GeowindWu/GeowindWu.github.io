---
layout: post
title: Android 原生应用之WIFI
description: 下了原生应用的代码来看,本想找到android连接wifi的系统代码,没找到,不过有新的发现
---

系统连接wifi的那个界面是集成自PreferenceActivity,里面的内容是由组件PreferenceCategory来做的,如要分页,用PreferenceScreen ,详细不说了,百度谷歌这些关键词就知道了 

系统切换连接其他wifi的原理是通过设置优先级,系统会连接优先级高的接入点,通过设置目标接入点的Wificonfig的为最高优先级, 然后断开重连
就会自动去连优先级最高的接入点了