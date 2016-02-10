---
layout: post
title: 'IBatis (MyBatis): Handling Constructors'
published: true
author: Loiane
comments: true
date: 2011-03-10 08:03:59
tags:
    - annotations
    - constructor
    - database
    - iBatis
    - ibatis annotation
    - Java
    - MyBatis
    - MyBatis annotation
    - mySQL
categories:
    - ibatis-mybatis
permalink: /2011/03/ibatis-mybatis-handling-constructors
image:
    feature: ibatis_mybatis_loiane.png
---

  This tutorial will walk you through how to setup iBatis (MyBatis) in a simple Java project and will present an example using a class constructor with arguments.



  



  



  Pre-Requisites



  For this tutorial I am using:



  IDE: Eclipse (you can use your favorite one) DataBase: MySQL Libs/jars: Mybatis, MySQL conector and JUnit (for testing)



  This is how your project should look like:



  



  



  Sample Database



  Please run the script into your database before getting started with the project implementation. You will find the script (with dummy data) inside the sql folder.



  



  1 &#8211; POJO



  I represented the beans here with a UML model, but you can download the complete source code in the end of this article.



  



  



  As you can see, we do not have a default construtor in this class, only a constructor with some arguments. Some persistence frameworks requires a default constructor, but you have to be careful with your business logic.



  Let&#8217;s say there are a couple of mandatory atributes in your class. Nothing is going to stop you if you do not set these atributes and try to insert, update on the database. And you problably will get an exception for that.



  To prevent this kind of situation, you can create a constructor with the mandatory arguments. And ibatis/mybatis can handle this situation for you, because you do not need to create a default constructor.



  2 &#8211; Blog Mapper &#8211; XML



  
    While properties will work for most Data Transfer Object (DTO) type classes, and likely most of yourdomain model, there may be some cases where you want to use immutable classes. Often tables thatcontain reference or lookup data that rarely or never changes is suited to immutable classes.Constructor injection allows you to set values on a class upon instantiation, without exposing publicmethods. MyBatis also supports private properties and private JavaBeans properties to achieve this, butsome people prefer Constructor injection. The constructor element enables this.
  
  
  
    
      In order to inject the results into the constructor, MyBatis needs to identify the constructor by the type of its parameters. Java has no way to introspect (or reflect) on parameter names. So when creating a constructor element, ensure that the arguments are in order, and that the data types are specified.
    
  



  If you need, you can still map another attributes, such as association, collection in your resultMap.



  Following is the Blog.xml file:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





		  

			  

			  

		  

	      

      



    	  
SELECT idBlog as id, name, url FROM BLOG
      



  
[/code]


  3 &#8211; Blog Mapper &#8211; Annotations



  We did the configuration in XML, now let&#8217;s try to use annotations to do the same thing we did using XML.



  This is the code for BlogMapper.java:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.data;

import java.util.List;

import org.apache.ibatis.annotations.Arg;
  
import org.apache.ibatis.annotations.ConstructorArgs;
  
import org.apache.ibatis.annotations.Select;

import com.loiane.model.Blog;

public interface BlogMapper {

/**
	   
* Returns the list of all Blog instances from the database.
	   
* @return the list of all Blog instances from the database.
	   
*/
	  
@Select("SELECT idBlog as id, name, url FROM BLOG ")
	  
@ConstructorArgs(value = {
          
@Arg(column="id",javaType=Integer.class),
          
@Arg(column="url",javaType=String.class)
      
})
	  
List selectAllBlogs();
  
}
  
[/code]


  Let&#8217;s take a look at the @ConstructorArgs and @Arg annotations:



  @ConstructorArgs



  Collects a group of results to be passed to a result object constructor.



  Attributes: value, which is an array of Args.



  Xml equivalent: constructor



  @Arg



  A single constructor argument that is part of a ConstructorArgs collection.



  Attributes: id, column, javaType, jdbcType, typeHandler.



  The id attribute is a boolean value that identifies the property to be used for comparisons, similar to the  XML element.



  Xml equivalent: idArg, arg



  We do not need to use the @Results annotation because the other attributes has the same name, so Mybatis knows how to map them.



  Download



  If you want to download the complete sample project, you can get it from my GitHub account: https://github.com/loiane/ibatis-constructor



  If you want to download the zip file of the project, just click on download:



  



  



  There are more articles about iBatis to come. Stay tooned!



  Happy Coding! 



  Once again though, some may find this external definition of maps somewhat tedious. Thereforethere’s an alternative syntax for those that prefer a more concise mapping style. For example:
