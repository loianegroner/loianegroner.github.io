---
layout: post
title: >
    Introduction to iBatis (MyBatis), An alternative to
    Hibernate and JDBC
published: true
author: Loiane
comments: true
date: 2011-02-07 08:02:59
tags:
    - database
    - iBatis
    - Java
    - MyBatis
    - persistence framework
categories:
    - ibatis-mybatis
permalink: >
    /2011/02/introduction-to-ibatis-mybatis-an-alternative-to-hibernate-and-jdbc
---

  I started to write a new article series about iBatis/MyBatis. This is the first article and it will walk you through what is iBatis/MyBatis and why you should use it.



  



  For those who does not know iBatis/MyBatis yet, it is a persistence framework – an alternative to JDBC and Hibernate, available for Java and .NET platforms. I&#8217;ve been working with it for almost two year, and I am enjoying it!



  The fisrt thin you may notice in this and following articles about iBatis/MyBatis is that I am using iBatis and Mybatis terms. Why? Until June 2010, iBatis was under Apache license and since then, the framework founders decided to move it to Google Code and they renamed it to MyBatis. The framework is still the same though, only has a different name now.



  I gathered some resources, so I am just going to quote them:



  What is MyBatis/iBatis?



  
    The MyBatis data mapper framework makes it easier to use a relational database with object-oriented applications. MyBatis couples objects with stored procedures or SQL statements using a XML descriptor. Simplicity is the biggest advantage of the MyBatis data mapper over object relational mapping tools. To use the MyBatis data mapper, you rely on your own objects, XML, and SQL. There is little to learn that you don&#8217;t already know. With the MyBatis Data Mapper, you have the full power of both SQL and stored procedures at your fingertips. (www.mybatis.org)
  



  



  
    iBATIS is based on the idea that there is value in relational databases and SQL, and that it is a good idea to embrace the industrywide investment in SQL. We have experiences whereby the database and even the SQL itself have outlived the application source code, and even multiple versions of the source code. In some cases we have seen that an application was rewritten in a different language, but the SQL and database remained largely unchanged.
  
  
  
    It is for such reasons that iBATIS does not attempt to hide SQL or avoid SQL. It is a persistence layer framework that instead embraces SQL by making it easier to work with and easier to integrate into modern object-oriented software. These days, there are rumors that databases and SQL threaten our object models, but that does not have to be the case. iBATIS can help to ensure that it is not.
  
  
  
    (iBatis in Action book)
  



  So&#8230;



  
     What is iBatis ?
  
  
  
    
      A JDBC Framework
    
    
      Developers write SQL, iBATIS executes it using JDBC.
    
    
      No more try/catch/finally/try/catch.
    
    
      An SQL Mapper
    
    
      Automatically maps object properties to prepared statement parameters.
    
    
      Automatically maps result sets to objects.
    
    
      Support for getting rid of N+1 queries.
    
    
      A Transaction Manager
    
    
      iBATIS will provide transaction management for database operations if no other transaction manager is available.
    
    
      iBATIS will use external transaction management (Spring, EJB CMT, etc.) if available.
    
    
      Great integration with Spring, but can also be used without Spring (the Spring folks were early supporters of iBATIS).
    
  
  
  
    What isn&#8217;t iBATIS ?
  
  
  
    
      An ORM
    
    
      Does not generate SQL
    
    
      Does not have a proprietary query language
    
    
      Does not know about object identity
    
    
      Does not transparently persist objects
    
    
      Does not build an object cache
    
  
  
  
    Essentially, iBatis is a very lightweight persistence solution that gives you most of the semantics of an O/R Mapping toolkit, without all the drama. In other words ,iBATIS strives to ease the development of data-driven applications by abstracting the low-level details involved in database communication (loading a database driver, obtaining and managing connections, managing transaction semantics, etc.), as well as providing higher-level ORM capabilities (automated and configurable mapping of objects to SQL calls, data type conversion management, support for static queries as well as dynamic queries based upon an object&#8217;s state, mapping of complex joins to complex object graphs, etc.). iBATIS simply maps JavaBeans to SQL statements using a very simple XML descriptor. Simplicity is the key advantage of iBATIS over other frameworks and object relational mapping tools.(http://www.developersbook.com )
  



  Who is using iBatis/MyBatis?



  See the list in this link: http://www.apachebookstore.com/confluence/oss/pages/viewpage.action?pageId=25



  I think the biggest case is MySpace, with millions of users. Very nice!



  This was just an introduction, so in next articles I will show how to create an application using iBatis/Mybatis – step-by-step.



  Enjoy! 
