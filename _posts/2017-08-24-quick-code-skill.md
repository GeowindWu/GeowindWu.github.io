---
layout: post
title: JAVA快速开发必须要熟练掌握的技能
---

最近做的项目，其实内容不多，但是弄了好久。反思一下，其实是因为在一些问题上弄了太久，事实上编码的时间并不长，
处理一些问题话了大量的时候。所以我总结了一下这些问题，这次只要掌握了这些技能，下次开发及一定能快很多。目测
速度提升百分之三百！
必须要熟练解决的问题如下：
* 中文乱码
* json和Object的相互转换
* xml解析和生成
* http请求，接受与转发
* 数据库类型在Hibernate中的映射类型，尤其注意时间类型
* 时间格式的转换，秒，毫秒转日期，或反之。

###中文乱码
	中文乱码终极解决方案在另一篇文章我写了--[Web项目中文乱码终极解决方案](2017-08-15-web-project-luanma.md)
	
###json字符串和bean的相互转换

	请认准**net.sf.json.JSONObject**，其他的类都不好用，用它无敌。
	**记住，JSONObject是json字符串和bean的中转站**
	
	json字符串转JSON对象 
	`public static JSONObejct fromObject(Object object)`
	入参为Obejct，自然包括字符串，出参数为JSONObject
	
	JSONObject转bean
	`public static Object JSONObject toBean(JSONObject jo)`
	入参是JSONObecjt，出参是Object，强转成需要的bean就行
	
	JSONObject转json字符串，直接toString（）就行
	
### xml解析和生成

``
	/**
	 * 将字符串转换为map对象
	 * 
	 * @param xml
	 * @return
	 */
	public static Map<String, String> readStringXmlToMap(String xml) {
		// 输入检查
		if (Utilitys.isBlank(xml))
			return new HashMap<String, String>();

		Map<String, String> map = new HashMap<String, String>();
		Document doc = null;
		try {
			doc = DocumentHelper.parseText(xml); // 将字符串转为XML
			Element rootElt = doc.getRootElement(); // 获取根节点
			// System.out.println("根节点：" + rootElt.getName()); // 拿到根节点的名称

			Iterator iter = rootElt.elementIterator(); // 获取所有的孩子节点
			// 遍历所有的孩子节点
			while (iter.hasNext()) {
				Element child = (Element) iter.next();
				try {
					String name = child.getName().trim();
					String value = child.getText().trim();
					map.put(name, value);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			doc = null;
		}
		return map;
	}
``
	生成的话，自己写个方法拼接吧，不说了
	
###数据库类型在Hibernate中的映射以及实体类的JAVA类型
来一份三者的对应表


Hibernate映射类型 |	java类型		|   标准SQL类型
| -------------   |:-------------  :| -------------:|
integer			|	java.lang.Integer |	integer|
long			|	java.lang.Long	|	bigint|
short			|	java.lang.Short	|	smallint|
float			|	java.lang.Float	|	float|
double			|	java.lang.Float	|	double|
big_decimal		|	java.math.BigDecimal |numeric|
character		|	java.lang.String|	char(1)|
string			|	java.lang.String	|varchar|
byte			|	byte或java.lang.Byte|	tinyint|
boolean			|	boolean或java.lang.Boolean | 	bit|
yes_no    		|	boolean或java.lang.Boolean|	char(1)('Y'/'N')|
true_false		|	boolean或java.lang.Boolean|	char(1)('Y'/'N')|
date			|	java.util.Date或java.sql.Date|	date|
time		|		java.util.Date或java.sql.Time|	time|
timestamp	|java.util.Date或java.sql.timestamp|	timestamp|
calendar	|java.util.Calendar	|timestamp|
calendar_date|	java.util.Calendar|	date|
binary	|byte[]|	varbinary或blob|
text	|java.lang.String|	clob|
serializable	|java.io.Serializable实例|	varbinary或blob|
clob	|java.sql.Clob|	clob|
blob	|java.sql.Blob	|varbinary或blob|
class	|java.lang.Class	|varchar|
locale	|java.util.Locale|	varchar|
timezone	|java.util.TimeZone|	varchar|
currency	|java.util.Currency	|varchar|


### 时间格式的转换，秒，毫秒转日期，或反之
	
	**毫秒转日期** 注意10位数是秒，13位数是毫秒，同意用毫秒，如果是秒，则乘以1000在用
	实力代码
	``
	String createTime = getParams(xmlmap, "CreateTime");
	Long ctlong = Long.parseLong(createTime);
		
	// 秒转成毫秒转日期
	Calendar c = Calendar.getInstance();
	c.setTimeInMillis(ctlong*1000);
	Date date = c.getTime();
	``
	
	``
	    SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String start="2011-09-20 12:30:45";
		long millis = sdf.parse(start).getTime();
	
	``
	
	 Date类型（注意是这个包：java.util.Date）有getTime（）方法能转成毫秒。上面sdf.parse()后得到的就是Date

	另外，最正确的时间戳是这个**"yyyy-MM-dd HH:mm:ss"** 有大小写之分，否则会有精度丢失的问题，按照这个就没错。
