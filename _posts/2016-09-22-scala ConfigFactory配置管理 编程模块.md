---
layout: post
title: "scala ConfigFactory配置管理 编程模块"
date: 2016-09-22
excerpt: ""
tags: [scala,ConfigFactory]
comments: true
---

1. **添加依赖：**

	"com.typesafe" % "config" % “1.3.0"

2. **引入依赖：**

	import com.typesafe.config.ConfigFactory

3. **使用样例：**

	<pre><code>1）在src/main/resources/application.conf中添加如下内容
	
	email {
	  serverHost = "smtp.exmail.qq.com"
	  serverPort = 465
	  userName = "auto@google.com"
	  password = "123"
	  mainReciverEmail = "alert@google.com"
	  ccEamilList = []
	  fromEmail = "auto@google.com”
	}
	
	2）在代码中加载配置项
		
	val config = ConfigFactory.load()
	
	3）在代码中使用配置项：
	
	val host = config.getString("email.serverHost")
	val port = config.getInt("email.serverPort")
	val userName = config.getString("email.userName")
	val password = config.getString("email.password")</code></pre>