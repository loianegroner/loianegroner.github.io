---
layout: post
title: 'Tutorial: Getting Started with Spring Security'
published: true
author: Loiane
comments: true
date: 2010-01-18 08:01:23
tags:
    - Acegi
    - Spring
    - Spring Security
categories:
    - spring
    - spring-security
permalink: /2010/01/tutorial-getting-started-with-spring-security
---

  



  This tutorial will cover a basic scenario where it  integrates Spring Security, using database-backed authentication, into an existing Spring web application.


> 
>   Spring Security is a security framework that provides declarative security for your Spring-based applications. Spring Security provides a comprehensive security solution, handling authentication and authorization, at both the web request level and at the method invocation level. Based on the Spring Framework, Spring Security takes full advantage of dependency injection (DI) and aspect oriented techniques.
> 

> 
>   Spring Security is also known as Acegi Security (or simply Acegi).
> 


  As with anything else related to spring the learning curve on spring-security is just as steep. But once you get the hang of it, it’s easy peasy and you can use the same configuration over and over again in your web apps.



  When I started to study Spring Security, I found these suggested steps at Spring Security page.



  If you want to configure Spring Security in your web application, follow the steps:



  First thing you need to do is to add the jar files in the application classpath. Download Spring Security, and from inside dist folder, copy the following jar files and paste them into your web application lib folder:


  * spring-security-core-2.0.4.jar
  * spring-security-core-tiger-2.0.4.jar
  * spring-security-acl-2.0.4.jar
  * spring-security-taglibs-2.0.4.jar

Also, you need to download Apache Commons Codec: _commons-codec-1.3.jar_

Now, let’s start with XML configuration.

### **Web.xml**

Insert the following block of code. It should be inserted right after the/context-param end-tag.

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
contextConfigLocation
	  

             
/WEB-INF/security-applicationContext.xml
	  

  



	  
springSecurityFilterChain
	  
org.springframework.web.filter.DelegatingFilterProxy
  



	  
springSecurityFilterChain
	  
/*
  


[/code]

### applicationContext-security.xml


  Let’s create the applicationContext-security.xml.



  I would suggest getting started with the applicationContext-security.xml that is found in the tutorial sample, and trimming it down a bit. Here’s what I got when I trimmed it down:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  







	  



          

      



      

          

          

              

              

              

              

	      

	  

  


[/code]

Now, you can try to execute the web application.

If you try to execute the app and get this **exception**:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
SEVERE: Context initialization failed
  
org.springframework.beans.factory.BeanDefinitionStoreException: Unexpected exception parsing XML document from ServletContext resource [/WEB-INF/applicationContext-security.xml]; nested exception is java.lang.NoClassDefFoundError: org/aspectj/lang/Signature
	  
at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.doLoadBeanDefinitions(XmlBeanDefinitionReader.java:420)
	  
at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:342)
	  
at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:310)
	  
at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:143)
	  
at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:178)
	  
at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:149)
	  
at org.springframework.web.context.support.XmlWebApplicationContext.loadBeanDefinitions(XmlWebApplicationContext.java:124)
	  
at org.springframework.web.context.support.XmlWebApplicationContext.loadBeanDefinitions(XmlWebApplicationContext.java:92)
	  
at org.springframework.context.support.AbstractRefreshableApplicationContext.refreshBeanFactory(AbstractRefreshableApplicationContext.java:123)
	  
at org.springframework.context.support.AbstractApplicationContext.obtainFreshBeanFactory(AbstractApplicationContext.java:423)
	  
at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:353)
	  
at org.springframework.web.context.ContextLoader.createWebApplicationContext(ContextLoader.java:255)
	  
at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:199)
	  
at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:45)
	  
at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:3764)
	  
at org.apache.catalina.core.StandardContext.start(StandardContext.java:4216)
	  
at org.apache.catalina.core.ContainerBase.start(ContainerBase.java:1014)
	  
at org.apache.catalina.core.StandardHost.start(StandardHost.java:736)
	  
at org.apache.catalina.core.ContainerBase.start(ContainerBase.java:1014)
	  
at org.apache.catalina.core.StandardEngine.start(StandardEngine.java:443)
	  
at org.apache.catalina.core.StandardService.start(StandardService.java:448)
	  
at org.apache.catalina.core.StandardServer.start(StandardServer.java:700)
	  
at org.apache.catalina.startup.Catalina.start(Catalina.java:552)
	  
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	  
at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
	  
at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
	  
at java.lang.reflect.Method.invoke(Unknown Source)
	  
at org.apache.catalina.startup.Bootstrap.start(Bootstrap.java:295)
	  
at org.apache.catalina.startup.Bootstrap.main(Bootstrap.java:433)
  
[/code]

Download **_aspectjrt-1.5.4.jar_** and add it to your application classpath. It will work.

**Let’s make some changes in applicationContext-security.xml.**

First change: replace the following code

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

          

  

  
[/code]

for

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  



    	  



          



          



  
[/code]


  The auto-config attribute basically tells spring-security to configure default settings for itself.



  The login.jsp is allowed to be access from ANY role.



  Rrestricting access to it would mean that no one would be able to reach even the login page.



  Note how we are using a jsp instead of a spring managed controller. The login page does not need to be a spring managed controller at all.



  We also tell spring-security to restrict access to ALL url’s to only those users who have the role ROLE_USER.



  Let’s say you have more than one user role. Mapping URLs to roles is really easy. In your http element, simply put successive elements like this:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

  

  
[/code]


  Of course you do not want to put all the usernames and passwords and theirs roles in the applicationContext-security.xml. But how do we tell spring-security to get all user authentication details from the database?



  Put the following code in applicationContext-security.xml (replace usernames and passwords block of code)


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  

  


[/code]


  This requires that a dataSource be created first.



  Now you ask: what does the convention over configuration assume my database tables look like?



  You should be aware that the default authentication provider requires the database structure to be in a certain way:


[][1]

[code lang=&#8221;sql&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
CREATE TABLE users
  
(
    
username character varying(50) NOT NULL,
    
"password" character varying(50) NOT NULL,
    
enabled boolean NOT NULL,
    
CONSTRAINT users_pkey PRIMARY KEY (username)
  
);

CREATE TABLE authorities
  
(
    
username character varying(50) NOT NULL,
    
authority character varying(50) NOT NULL,
    
CONSTRAINT fk\_authorities\_users FOREIGN KEY (username)
        
REFERENCES users (username) MATCH SIMPLE
        
ON UPDATE NO ACTION ON DELETE NO ACTION
  
);

CREATE UNIQUE INDEX ix\_auth\_username
    
ON authorities
    
USING btree
    
(username, authority);

[/code]


  If you want to configure the queries that are used, simply match the available attributes on the jdbc-user-service element to the SQL queries in the Java class referenced above.



  For example: You want to simplify tha database schema by adding the user’s role directly to the user table. Let’s modify the xml configuration:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  
[/code]


  There are a couple other pages that maybe you want to configure.



  Access Denied: This is the page the user will see if they are denied access to the site due to lack of authorization (i.e. tried to hit a page that they didn’t have access to hit, even though they were authenticated properly). This is configured as follows:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

       
&#8230;
  

  
[/code]


  Default Target URL: This is where the user will be redirected upon successful login. This can (and probably should) be a page located under Spring control. Configured as follows:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

      
&#8230;
          

      
&#8230;
  

  
[/code]


  Logout URL: The page where the user is redirected upon a successful logout. This can be a page located under Spring control too (provided that it allows anonymous access):


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

      
&#8230;
    	  

      
&#8230;
  

  
[/code]


  Login Failure URL: Where the user will be sent if there was an authentication failure. Typically this is back to the login form, with a URL parameter, such as:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

      
&#8230;
          

      
&#8230;
  

  
[/code]

This is what you need to get started with Spring Security.

Next week, I am going to post how Spring Security **login.jsp** looks like.

Happy coding!

 [1]: http://loianegroner.com/wp-content/uploads/2010/01/spring-security-tutorial-database.png