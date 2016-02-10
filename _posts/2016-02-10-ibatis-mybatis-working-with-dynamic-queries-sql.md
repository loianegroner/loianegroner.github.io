---
layout: post
title: 'IBatis (MyBatis): Working with Dynamic Queries (SQL)'
published: true
author: Loiane
comments: true
date: 2011-03-22 08:03:33
tags:
    - choose
    - database
    - dynamic sql
    - forEach
    - iBatis
    - if
    - Java
    - MyBatis
    - mySQL
    - trim
    - where
categories:
    - ibatis-mybatis
permalink: /2011/03/ibatis-mybatis-working-with-dynamic-queries-sql
---

  
    This tutorial will walk you through how to setup iBatis (MyBatis) in a simple Java project and will present how to work with dynamic queries (SQL).
  
  
  
    
  
  
  
    
  
  
  
    Pre-Requisites
  
  
  
    For this tutorial I am using:
  
  
  
    IDE: Eclipse (you can use your favorite one) DataBase: MySQL Libs/jars: Mybatis, MySQL conector and JUnit (for testing)
  
  
  
    This is how your project should look like:
  
  
  
    
  
  
  
    
  
  
  
    Sample Database
  
  
  
    Please run the script into your database before getting started with the project implementation. You will find the script (with dummy data) inside the sql folder.
  
  
  
    
  
  
  
    1 &#8211; Article POJO
  
  
  
    I represented the pojo we are going to use in this tutorial with a UML diagram, but you can download the complete source code in the end of this article.
  
  
  
    
  
  
  
    
  
  
  
    The goal of this tutorial is to demonstrate how to retrieve the article information from database using dynamic sql to filter the data.
  
  
  
    2 &#8211; Article Mapper &#8211; XML
  
  
  
    
      One of the most powerful features of MyBatis has always been its Dynamic SQL capabilities. If you have any experience with JDBC or any similar framework, you understand how painful it is to conditionally concatenate strings of SQL together, making sure not to forget spaces or to omit a comma at the end of a list of columns. Dynamic SQL can be downright painful to deal with.
    
    
    
      While working with Dynamic SQL will never be a party, MyBatis certainly improves the situation with a powerful Dynamic SQL language that can be used within any mapped SQL statement.
    
    
    
      The Dynamic SQL elements should be familiar to anyone who has used JSTL or any similar XML based text processors. In previous versions of MyBatis, there were a lot of elements to know and understand. MyBatis 3 greatly improves upon this, and now there are less than half of those elements to work with. MyBatis employs powerful OGNL based expressions to eliminate most of the other elements.
    
    
    
      
        if
      
      
        choose (when, otherwise)
      
      
        trim (where, set)
      
      
        foreach
      
    
  


Let&#8217;s explain each one with examples.

**1 &#8211; First scenario**: we want to retrieve all the articles from database with an optional filter: title. In other words, if user specify an article title, we are going to retrieve the articles that match with the title, otherwise we are going to retrieve all the articles from database. So we are going to implement a **condition (if)**:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
SELECT id, title, author
	  
FROM article
	  
WHERE id_status = 1
	  

		  
AND title LIKE #{title}
	  

  

  
[/code]

**2 &#8211; Second scenario**: Now we have two optional filters: article title and author. The user can specify both, none or only one filter. So we are going to implement two conditions:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
SELECT id, title, author
	  
FROM article
	  
WHERE id_status = 1
	  

		  
AND title LIKE #{title}
	  

	  

		  
AND author LIKE #{author}
	  

  

  
[/code]

**3 &#8211; Third scenario**: Now we want to give the user only one option: the user will have to specify only one of the following filters: title, author or retrieve all the articles from iBatis category. So we are going to use a **Choose** element:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
SELECT id, title, author
	  
FROM article
	  
WHERE id_status = 1
	  

		  

			  
AND title LIKE #{title}
		  

		  

			  
AND author LIKE #{author}
		  

		  

			  
AND id_category = 3
		  

	  

  

  
[/code]

**4 &#8211; Fourth scenario**: take a look at all three statements above. They all have a condition in common: WHERE id_status = 1. It means we are already filtering the active articles Let&#8217;s remove this condition to make it more interesting.

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
SELECT id, title, author
	  
FROM article
	  
WHERE
	  

		  
title LIKE #{title}
	  

	  

		  
AND author LIKE #{author}
	  

  

  
[/code]

What if both title and author are null? We are going to have the following statement:

[code lang=&#8221;sql&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
SELECT id, title, author
  
FROM article
  
WHERE
  
[/code]

And what if only the author is not null? We are going to have the fololwing statement:

[code lang=&#8221;sql&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
SELECT id, title, author
  
FROM article
  
WHERE
  
AND author LIKE #{author}
  
[/code]

And both FAILS!

How to fix it?

**5 &#8211; Fifth scenario**: we want to retrieve all the articles with two optional filters: title and author. To avoid the 4th scenatio, we are going to use a Where element:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
SELECT id, title, author
	  
FROM article
	  

		  

			  
title LIKE #{title}
		  

		  

			  
AND author LIKE #{author}
		  

	  

  

  
[/code]



> 
>   MyBatis has a simple answer that will likely work in 90% of the cases. And in cases where it doesn’t, you can customize it so that it does.
> 
> 
> 
>   The where element knows to only insert “WHERE” if there is any content returned by the containing tags. Furthermore, if that content begins with “AND” or “OR”, it knows to strip it off.
> 
> 
> 
>   If the where element does not behave exactly as you like, you can customize it by defining your own trim element. For example,the trim equivalent to the where element is:
> 

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
SELECT id, title, author
	  
FROM article
	  

		  

			  
title LIKE #{title}
		  

		  

			  
AND author LIKE #{author}
		  

	  

  

  
[/code]



> 
>   The overrides attribute takes a pipe delimited list of text to override, where whitespace is relevant. The result is the removal of anything specified in the overrides attribute, and the insertion of anything in the with attribute.
> 


  You can also use the Trim element with SET.



  6 &#8211; Sixth scenario: the user will choose all the categories an article can belong to. So in this case, we have a list (a collection), and we have to interate this collection and we are going to use a foreach element:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  
SELECT id, title, author
	  
FROM article
	  
WHERE id_category IN
	  

		  
#{category}
	  

  

  
[/code]

> 
>   The foreach element is very powerful, and allows you to specify a collection, declare item and index variables that can be used inside the body of the element. It also allows you to specify opening and closing strings, and add a separator to place in between iterations. The element is smart in that it won’t accidentally append extra separators.
> 
> 
> 
>   Note: You can pass a List instance or an Array to MyBatis as a parameter object. When you do, MyBatis will automatically wrap it in a Map, and key it by name. List instances will be keyed to the name “list” and array instances will be keyed to the name “array”.
> 


  The complete Article.xml file looks like this:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





    	  
SELECT id, title, author
		  
FROM article
		  
WHERE id_status = 1
		  

			  
AND title LIKE #{title}
		  

      



    	  
SELECT id, title, author
		  
FROM article
		  
WHERE id_status = 1
		  

			  
AND title LIKE #{title}
		  

		  

			  
AND author LIKE #{author}
		  

      



    	  
SELECT id, title, author
		  
FROM article
		  
WHERE id_status = 1
		  

			  

				  
AND title LIKE #{title}
			  

			  

				  
AND author LIKE #{author}
			  

			  

				  
AND id_category = 3
			  

		  

      



    	  
SELECT id, title, author
		  
FROM article
		  

			  

				  
title LIKE #{title}
			  

			  

				  
AND author LIKE #{author}
			  

		  

      



    	  
SELECT id, title, author
		  
FROM article
		  

			  

				  
title LIKE #{title}
			  

			  

				  
AND author LIKE #{author}
			  

		  

      



    	  
SELECT id, title, author
		  
FROM article
		  
WHERE id_category IN
		  

			  
#{category}
		  

      

  

  
[/code]


  
    Download
  
  
  
    If you want to download the complete sample project, you can get it from my GitHub account: https://github.com/loiane/ibatis-dynamic-sql
  
  
  
    If you want to download the zip file of the project, just click on download:
  
  
  
    
  
  
  
    
  
  
  
    There are more articles about iBatis to come. Stay tooned!
  
  
  
    Happy Coding! 
  
