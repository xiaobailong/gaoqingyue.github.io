---
layout: post
title: "scala file数据处理 编程模块"
date: 2016-09-22
excerpt: ""
tags: [scala,文件]
comments: true
---

1. **添加依赖：**

	<pre><code>"com.github.scala-incubator.io" %% "scala-io-core" % "0.4.3",
	"com.github.scala-incubator.io" %% "scala-io-file" % “0.4.3"</code></pre>

2. **引入依赖：**

	<pre><code>import scala.io.Source._
	import scalax.io._</code></pre>

3. **使用样例：**

	<pre><code>def writeToFile(content: String, file: String) = {
	  val output: Output = Resource.fromFile(file)
	  output.write(content + "\n")("UTF-8")
	}
	
	def inputFromFile(file: String): Seq[Log] = {
	  val lines = fromFile(file)("UTF-8").getLines()
	  lines.map { case x: String => Factory.getDataFromJson(x) }.toSeq
	}</code></pre>