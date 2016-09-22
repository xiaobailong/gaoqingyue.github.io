---
layout: post
title: "scala mail发送 编程模块"
date: 2016-09-22
excerpt: ""
tags: [scala,mail]
comments: true
---

1. **添加依赖：**

	"org.apache.commons" % "commons-email" % “1.4"

2. **引入依赖：**
	
	<pre><code> import org.apache.commons.mail.HtmlEmail
	import org.apache.commons.mail.DefaultAuthenticator
	import javax.activation.{ MailcapCommandMap, CommandMap 
	import org.apache.commons.mail.EmailException</code></pre>

3. **使用样例：**

	<pre><code>
	import scala.concurrent.duration.FiniteDuration
	/**
	 * The email message sent to Actors in charge of delivering email
	 *
	 * @param subject the email subject
	 * @param recipient the recipient
	 * @param from the sender
	 * @param text alternative simple text
	 * @param html html body
	 */
	case class EmailMessage(subject: String,
	                        recipient: String,
	                        ccToEmail: Seq[String],
	                        from: String,
	                        text: Option[String] = None,
	                        html: Option[String] = None,
	                        smtpConfig: SmtpConfig,
	                        retryOn: FiniteDuration,
	                        var deliveryAttempts: Int = 0)
	---
	/**
	 * Smtp config
	 * @param tls if tls should be used with the smtp connections
	 * @param ssl if ssl should be used with the smtp connections
	 * @param port the smtp port
	 * @param host the smtp host name
	 * @param user the smtp user
	 * @param password the smtp password
	 */
	case class SmtpConfig(tls: Boolean = false,
	                      ssl: Boolean = false,
	                      port: Int = 25,
	                      host: String,
	                      user: String,
	                      password: String)
	---
	import org.apache.commons.mail.HtmlEmail
	import org.apache.commons.mail.DefaultAuthenticator
	import scala.concurrent.duration.FiniteDuration
	import com.typesafe.config.ConfigFactory
	import javax.activation.{ MailcapCommandMap, CommandMap }
	import org.apache.commons.mail.EmailException
	object EmailSender {
	  val config = ConfigFactory.load()
	  def sendEmail(emailMessage: EmailMessage) = {
	    val email = new HtmlEmail()
	    email.setTLS(emailMessage.smtpConfig.tls)
	    email.setSSL(emailMessage.smtpConfig.ssl)
	    email.setSmtpPort(emailMessage.smtpConfig.port)
	    email.setHostName(emailMessage.smtpConfig.host)
	    email.setAuthenticator(new DefaultAuthenticator(
	      emailMessage.smtpConfig.user,
	      emailMessage.smtpConfig.password))
	    emailMessage.text match {
	      case Some(text) => email.setTextMsg(text)
	      case None       =>
	    }
	    emailMessage.html match {
	      case Some(html) => email.setHtmlMsg(html)
	      case None       =>
	    }
	    emailMessage.ccToEmail.foreach { x => email.addCc(x) }
	    email.addTo(emailMessage.recipient)
	      .setFrom(emailMessage.from)
	      .setSubject(emailMessage.subject)
	      .send()
	  }
	  def sendLogByEmail(title: String, content: String) = {
	    val host = config.getString("email.serverHost")
	    val port = config.getInt("email.serverPort")
	    val userName = config.getString("email.userName")
	    val password = config.getString("email.password")
	    val smtpConfig = SmtpConfig(false, true, port, host, userName, password)
	    val ccEmailSeq = config.getStringList("email.ccEamilList").toArray().map { x => x.toString() }
	    val mainReciverEmail = config.getString("email.mainReciverEmail")
	    val fromEmail = config.getString("email.fromEmail")
	    val emailMessage = EmailMessage(title, mainReciverEmail, ccEmailSeq, fromEmail, Some(content), None, smtpConfig, 		FiniteDuration(100, "millis"), 0)
	    val mc = CommandMap.getDefaultCommandMap.asInstanceOf[MailcapCommandMap]
	    mc.addMailcap("text/html;; x-java-content-handler=com.sun.mail.handlers.text_html")
	    mc.addMailcap("text/xml;; x-java-content-handler=com.sun.mail.handlers.text_xml")
	    mc.addMailcap("text/plain;; x-java-content-handler=com.sun.mail.handlers.text_plain")
	    mc.addMailcap("multipart/*;; x-java-content-handler=com.sun.mail.handlers.multipart_mixed")
	    mc.addMailcap("message/rfc822;; x-java-content-handler=com.sun.mail.handlers.message_rfc822")
	    CommandMap.setDefaultCommandMap(mc)
	    try {
	      sendEmail(emailMessage)
	    } catch {
	      case eme: EmailException => {
	         }
	      case _: Throwable => {
	              }
	    }
	  }
	  def main(args: Array[String]): Unit = {
	    sendLogByEmail("prod-alert email test", "prod-alert email test")
	  }
	}</code></pre>