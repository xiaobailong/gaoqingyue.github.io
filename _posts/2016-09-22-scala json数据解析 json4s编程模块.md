---
layout: post
title: "scala json数据解析 json4s编程模块"
date: 2016-09-22
excerpt: ""
tags: [scala,json]
comments: true
---

1. **添加依赖：**

	"org.json4s" %% "json4s-native" % "3.3.0",
	"org.json4s" %% "json4s-ext" % "3.3.0",

2. **引入依赖：**

	<pre><code> import org.json4s.native.JsonMethods._ 	    //json to object
	import org.json4s.native.Serialization.write //object to json
	import org.json4s.DefaultFormats</code></pre>

3. **json数据解析（String）：**

	<pre><code>implicit val formats = DefaultFormats
	case class History(
	  csSum: Long,
	  rsSum: Map[String,Long]
	)
	parse(str).extract[History]</code></pre>

4. **class数据转成json数据：**

	<pre><code> implicit val formats = DefaultFormats
	val data: History=Histroy(1,Map("1",1))
	val json = write(data)</code></pre>