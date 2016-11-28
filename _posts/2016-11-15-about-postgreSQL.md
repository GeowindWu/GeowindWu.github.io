---
layout: post
title: SQL语句
description: 公司项目用了postpreSQL数据库管理系统
---

## 做一个任务时候的一点心得

任务是根据记录的插入时间查询出来,有当天的,本周的,本月的,所有的,需要一次性查出,开始用的是Union ALL ,后来被告知效率不高,用case When好,的确用case when 写比较好,sql语句也比较好看,收货就是学习了这两个语句 union 和 case when

当时用的代码
	SELECT 
		TMP.ieventtype  "eventType" ,
		TMP.streventname "streventname",
		TMP.day  "thatDayNotResume",
		TMP.week "thatWeekNotResume",
		TMP.month "thatMouthNotResume",
		TMP.day+TMP.week+TMP.month "allNotResume"	
	FROM (
		select ieventtype,streventname,
		sum(case when (to_timestamp(CURRENT_DATE || ' 00:00:00','yyyy-mm-dd hh24:mi:ss') <= dteventocutime AND dteventocutime <= to_timestamp(CURRENT_DATE || ' 23:59:59','yyyy-mm-dd hh24:mi:ss')) then 1 else 0 end) as "day" ,
		sum(case when (EXTRACT (week FROM CURRENT_DATE) = EXTRACT (week FROM dteventocutime))AND (EXTRACT (YEAR FROM CURRENT_DATE) = EXTRACT (YEAR FROM dteventocutime)) then 1 else 0 end) as "week",
																																																												sum(case when (EXTRACT (MONTH FROM CURRENT_DATE) = EXTRACT (MONTH FROM dteventocutime))AND (EXTRACT (YEAR FROM CURRENT_DATE) = EXTRACT (YEAR FROM dteventocutime)) then 1 else 0 end) as "month"  
		from tbl_mobilealarmeventinfo
																																																												GROUP BY 	ieventtype,streventname	
																																																												)TMP
						
																																																										
## 写一些网上看到的MySQL的提高性能的方法

嗨呀,忘了,微博上找不到那个连接了....
以后吧,以后有相关知识再补上来
