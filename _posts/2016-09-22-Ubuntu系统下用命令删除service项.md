---
layout: post
title: "Ubuntu系统下用命令删除service项"
date: 2016-09-22
excerpt: ""
tags: [Ubuntu,service]
comments: true
---

1. **从系统服务启动列表删除**

	<pre><code>sudo update-rc.d -f service-name remove</code></pre>

2. **从service列表删除**

	<pre><code>sudo rm -rf /etc/init.d/service-name</code></pre>





	
