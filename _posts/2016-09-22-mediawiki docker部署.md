---
layout: post
title: "mediawiki docker部署"
date: 2016-09-22
excerpt: ""
tags: [docker,mediawiki]
comments: true
---

1. **拉取mysql镜像**

	<pre><code>sudo docker pull mysql</code></pre>

2. **启动MySQL数据库镜像**

	<pre><code>sudo docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql</code></pre>

3. **拉取mediawiki镜像**

	<pre><code>sudo docker pull synctree/mediawiki</code></pre>

4. **启动mediawiki镜像，并按照提示进行设置**

	<pre><code>sudo docker run --name mediawiki --link mysql:mysql -p 81:80 -d synctree/mediawiki</code></pre>

5. **关掉mediawiki**

	<pre><code>sudo docker stop mediawiki && sudo docker rm mediawiki</code></pre>

6. **将第五步配置好并下载的配置文件放到/docker/mediawiki/data/LocalSettings.php，重新启动mediawiki**

	<pre><code>docker run --name mediawiki --link mysql:mysql -p 81:80 -v /docker/mediawiki/data/LocalSettings.php:/var/www/html/LocalSettings.php -d synctree/mediawiki</code></pre>