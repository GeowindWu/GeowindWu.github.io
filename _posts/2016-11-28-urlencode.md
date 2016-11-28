---
layout: post
title: 上传数据之URLEncode
description: 为了避免中文乱码,需要对数据进行编码
keyword: urlencode
---

## 对应JAVA的类,方法
=================
java.net.URLEncoder

URLEncoder.encode(String s, String charsetName)



## 编码的代码
==================

一次编码
```java
/**
	 * 处理参数
	 * 
	 * @param param
	 * @return
	 * @throws UnsupportedEncodingException
	 */
	private static String encodeUrlParams(Map<String, String> param)
			throws UnsupportedEncodingException {
		StringBuilder bulider = new StringBuilder();
		if (param != null) {
			Set<String> keys = param.keySet();
			for (String key : keys) {
				if (StringUtils.isBlank(param.get(key))) {
					bulider.append(URLEncoder.encode(key, "UTF-8")).append("=").append("").append("&");
				} else {
					bulider.append(URLEncoder.encode(key, "UTF-8")).append("=")
							.append(URLEncoder.encode(param.get(key), "UTF-8"))
							.append("&");
				}
			}
		}
		if (bulider.length() > 0) {
			return bulider.substring(0, bulider.length() - 1);
		}
		return bulider.toString();
	}
```

二次编码
```java
/**
	 * 二次编码
	 * 
	 * @param params
	 * @return
	 * @throws UnsupportedEncodingException
	 */
	private static String doubleEncodeUrlParams(Map<String, String> params)
			throws UnsupportedEncodingException {
		Set<Entry<String, String>> entrySet = params.entrySet();
		int count = 0;
		StringBuilder sb = new StringBuilder();
		for(Entry<String,String> entry : entrySet){
			if(count > 0){
				sb.append("&");
			}
			String param ;
			// value 二次编码,
			if(StringUtils.isBlank(entry.getValue())){
				param = URLEncoder.encode( URLEncoder.encode("","UTF-8"),"UTF-8");
			}else{
				param = URLEncoder.encode(URLEncoder.encode(entry.getValue(),"UTF-8"),"UTF-8");
			}
			count ++;
			
			sb.append(entry.getKey()).append("=").append(param);
		}
		return sb.toString();
	}
```

## HTTP 工具类的请求方法
http
=====
```java
public static InputStream postHttpRequestReturnStream(String url,
			Map<String, String> params) throws Exception {
		// 处理参数 二次编码
		String param = doubleEncodeUrlParams(params);
		URL console = new URL(url);
		HttpURLConnection conn = (HttpURLConnection) console.openConnection();

		int iconnectimeout = StringUtils.isNotBlank(params
				.get("iconnectimeout")) ? Integer.parseInt(params
				.get("iconnectimeout")) : 30000;// 默认30秒
		conn.setConnectTimeout(iconnectimeout);// 追加一个超时设置：30秒。设置连接主机超时（单位：毫秒）
		int ireadtimeout = StringUtils.isNotBlank(params.get("ireadtimeout")) ? Integer
				.parseInt(params.get("ireadtimeout")) : 1000 * 60 * 1;// 默认60秒
		conn.setReadTimeout(ireadtimeout);// 默认设置数据读取时间：60秒。设置从主机读取数据超时（单位：毫秒）

		if (params != null
				&& StringUtils.isNotBlank(params.get(REQUEST_TIMEOUT))) {
			try {
				conn.setReadTimeout(1000 * Integer.parseInt(params
						.get(REQUEST_TIMEOUT)));
			} catch (Exception e) {
				// log.error("set rqeust timeout error:"+ e.getMessage());
				e.printStackTrace();
			}
		}
		conn.setRequestProperty("Accept-Charset", "utf-8");
		conn.setRequestProperty("contentType", "utf-8");
		conn.setUseCaches(false);
		conn.setRequestMethod("POST");// POST请求
		if (param != null) {
			conn.setDoOutput(true);// 是否输入参数
			conn.getOutputStream().write(param.toString().getBytes());// 输入参数
		}
		// 开始连接
		InputStream is = conn.getInputStream();
		return is;
	}

```

https
=====
```java
public static InputStream postHttpsUrlReturnStream(String url,
			Map<String, String> params) throws Exception {
		// 对空URL不处理
		if (url == null || url.length() == 0)
			return null;

		// 处理参数
		String param = encodeUrlParams(params);
		SSLContext sc = SSLContext.getInstance("SSL");
		// 指定信任https
		sc.init(null, new TrustManager[] { new TrustAnyTrustManager() },
				new java.security.SecureRandom());
		URL console = new URL(url);
		HttpsURLConnection conn = (HttpsURLConnection) console.openConnection();

		int iconnectimeout = StringUtils.isNotBlank(params
				.get("iconnectimeout")) ? Integer.parseInt(params
				.get("iconnectimeout")) : 30000;// 默认30秒
		conn.setConnectTimeout(iconnectimeout);// 追加一个超时设置：30秒。设置连接主机超时（单位：毫秒）
		int ireadtimeout = StringUtils.isNotBlank(params.get("ireadtimeout")) ? Integer
				.parseInt(params.get("ireadtimeout")) : 1000 * 60 * 1;// 默认60秒
		conn.setReadTimeout(ireadtimeout);// 默认设置数据读取时间：60秒。设置从主机读取数据超时（单位：毫秒）

		if (params != null
				&& StringUtils.isNotBlank(params.get(REQUEST_TIMEOUT))) {
			try {
				conn.setReadTimeout(1000 * Integer.parseInt(params
						.get(REQUEST_TIMEOUT)));
			} catch (Exception e) {
				// log.error("set rqeust timeout error:"+ e.getMessage());
				e.printStackTrace();
			}
		}
		conn.setSSLSocketFactory(sc.getSocketFactory());
		conn.setHostnameVerifier(new TrustAnyHostnameVerifier());
		conn.setRequestProperty("Accept-Charset", "utf-8");
		conn.setRequestProperty("contentType", "utf-8");
		// 设置POST请求
		conn.setRequestMethod("POST");
		// 不需要缓存
		conn.setUseCaches(false);
		if (param != null) {
			conn.setDoOutput(true);// 是否输入参数
			conn.getOutputStream().write(param.toString().getBytes("UTF-8"));// 输入参数
		}
		// 开始连接
		conn.connect();
		InputStream is = conn.getInputStream();
		// conn.disconnect();
		return is;
	}
```	
	
主要就是这些比较重要的代码,记录下来,以后可以复用