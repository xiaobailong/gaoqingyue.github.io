---
layout: post
title: "scala json数据处理 编程模块"
date: 2016-09-22
excerpt: ""
tags: [scala,json]
comments: true
---

1. **添加依赖：**
	
	<pre><code> "com.gilt" %% "jerkson" % "0.6.8",
	"com.fasterxml.jackson.module" %% "jackson-module-scala" % "2.6.1”,</code></pre>

2. **引入依赖：**

	import com.codahale.jerkson.Json._

3. **json数据解析（String）：**

	<pre><code>case class History(
	  csSum: Long,
	  rsSum: Map[String,Long]
	)
	parse\[History](dataString)</code></pre>

4. **class数据转成json数据：**

	<pre><code> val data: History= History(1,Map("1",1))
	val json = generate(data)</code></pre>