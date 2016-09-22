---
layout: post
title: "Scala impala数据库连接，sql执行，c3p0 编程模块"
date: 2016-09-22
excerpt: ""
tags: [scala,impala,sql,c3p0]
comments: true
---

1. **添加依赖：**

	"org.apache.hive" % "hive-jdbc" % "1.2.1",
	"com.mchange" % "c3p0" % “0.9.2.1"

2. **引入依赖：**

	import com.mchange.v2.c3p0.ComboPooledDataSource

3. **使用样例：**

	<pre><code>1）在src/main/resources/application.conf中添加如下内容
	
	impala {
	 connectionUrl= "jdbc:hive2://impala.google.com:21050/test;auth=noSasl"
	 jdbcDriverName= "org.apache.hive.jdbc.HiveDriver"
	  db = "test”
	}
	
	2）数据库连接，sql执行代码
	
	import java.sql.{SQLException, ResultSet}
	import com.mchange.v2.c3p0.ComboPooledDataSource
	import com.typesafe.config.ConfigFactory
	
	object SQLUtil {
	
	  val config = ConfigFactory.load()
	  val connectionUrl = config.getString("impala.connectionUrl")
	  val jdbcDriverName = config.getString("impala.jdbcDriverName")
	  val dataSource = createDataSource()
	  
	  private def createDataSource(): ComboPooledDataSource = {
	   val cpds = new ComboPooledDataSource()
	    cpds.setDriverClass(jdbcDriverName)
	    cpds.setJdbcUrl(connectionUrl)
	    //不要设置过大,以免超过数据库的最大连接数,导致程序异常.
	    cpds.setInitialPoolSize(10)
	    cpds.setMaxPoolSize(100)
	    cpds.setMinPoolSize(10)
	    cpds.setMaxStatements(0)
	    cpds
	  }
	
	  def execSql[T](sqlStatement: String)(function: Option[ResultSet] => T) = {
	    var connection: Option[java.sql.Connection] = None
	    try {
	      connection = Some(this.dataSource.getConnection)
	      val statement = connection.get.createStatement()
	      val rs = Some(statement.executeQuery(sqlStatement))
	      function(rs)
	    } catch {
	      case sqlEp: SQLException =>
	        sqlEp.printStackTrace()
	        function(None)
	      case e: Exception =>
	        e.printStackTrace()
	        function(None)
	    } finally {
	      try {
	        connection match {
	          case Some(con) => con.close()
	          case None =>
	        }
	      } catch {
	        case e: Exception => e.printStackTrace()
	      }
	    }
	  }
	
	  def execUpdateSql(sqlStatement: String) = {
	    var connection: Option[java.sql.Connection] = None
	    try {
	      connection = Some(this.dataSource.getConnection)
	      val statement = connection.get.createStatement()
	      val rs = statement.executeUpdate(sqlStatement)
	    } catch {
	      case sqlEp: SQLException =>
	        sqlEp.printStackTrace()
	      case e: Exception =>
	        e.printStackTrace()
	    } finally {
	      try {
	        connection match {
	          case Some(con) => con.close()
	          case None =>
	        }
	      } catch {
	        case e: Exception => e.printStackTrace()
	      }
	    }
	  }
	}</code></pre>