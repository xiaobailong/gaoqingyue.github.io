---
layout: post
title: "Presto CLI 使用方法"
date: 2016-09-21
excerpt: ""
tags: [Presto CLI]
comments: true
---

以0.152版本为例:

1. **下载presto-cli-0.152-executable.jar**

	<pre><code>wget https://repo1.maven.org/maven2/com/facebook/presto/presto-cli/0.152/presto-cli-0.152-executable.jar</pre></code>

2. **重命名presto-cli-0.152-executable.jar为presto.jar**

	<pre><code>mv presto-cli-0.152-executable.jar presto.jar</pre></code>

3. **给presto.jar添加可执行权限**

	<pre><code>chmod +x presto.jar </pre></code>

4. **可以直接执行presto.jar并附加参数来使用了**

	<pre><code>./presto --server localhost:8082 --catalog hive --schema default
	./presto.jar --server localhost:8082 --catalog localfile --schema logs --debug</pre></code>
