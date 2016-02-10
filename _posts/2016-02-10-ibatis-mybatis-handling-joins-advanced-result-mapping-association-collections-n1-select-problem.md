---
layout: post
title: 'IBatis (MyBatis): Handling Joins: Advanced Result Mapping, Association, Collections, N+1 Select Problem'
published: true
author: Loiane
comments: true
date: 2011-03-01 08:03:22
tags:
    - annotations
    - database
    - delete
    - iBatis
    - ibatis annotation
    - insert
    - Java
    - many-to-many
    - MyBatis
    - MyBatis annotation
    - mySQL
    - one-to-many
    - One-to-one
    - select
    - update
categories:
    - ibatis-mybatis
permalink: >
    /2011/03/ibatis-mybatis-handling-joins-advanced-result-mapping-association-collections-n1-select-problem
---

  This tutorial will walk you through how to setup iBatis (MyBatis) in a simple Java project and will present examples using advanced result mapings, how to hadle mappings with association, collections, n+1 problem and will show how to configure these relationships using XML configuration and annotations.



  



  Pre-Requisites



  For this tutorial I am using:



  IDE: Eclipse (you can use your favorite one) DataBase: MySQL Libs/jars: Mybatis, MySQL conector and JUnit (for testing)



  This is how your project should look like:



  



  Sample Database



  Please run the script into your database before getting started with the project implementation. You will find the script (with dummy data) inside the sql folder.



  


## 1 &#8211; POJOs &#8211; Beans


  I represented the beans here with a UML model, but you can download the complete source code in the end of this article.



  



  The goal of this post is to demonstrate how to retrieve all the blog information from the database, but as you can see, the class Blog contains an association (Author) and a collection of Posts (and it contains a collection of Tags). And we are going to try to retrieve all this information at once.



  So we are going to demonstrate One-to-one, one-to-many and many-to-many relationships using iBatis/Mybatis.


## 2 &#8211; Advanced Result Mapping


  The result map on the code above is an advanced result mapping. As I already mentioned on previous posts, the resultMap element is the most important and powerful element in MyBatis.




> 
>   MyBatis was created with one idea in mind: Databases aren’t always what you want or need them to be. While we’d love every database to be perfect 3rd normal form or BCNF, they aren’t. And it would be great if it was possible to have a single database map perfectly to all of the applications that use it, it’s not. Result Maps are the answer that MyBatis provides to this problem.
> 
> 
> 
>   The resultMap element has a number of sub-elements and a structure worthy of some discussion. The following is a conceptual view of the resultMap element.
> 
> 
>   * constructor – used for injecting results into the constructor of a class upon instantiation
>   *  id – an ID result; flagging results as ID will help improve overall performance
>   * result – a normal result injected into a field or JavaBean property
>   * association – a complex type association; many results will roll up into this type
>   * collection – a collection of complex types
>   * discriminator – uses a result value to determine which resultMap to use.
> 
> 
>   Best Practice: Always build ResultMaps incrementally. Unit tests really help out here. If you try to build a gigantic resultMap like the one above all at once, it’s likely you’ll get it wrong and it will be hard to work with. Start simple, and evolve it a step at a time. And unit test! The downside to using frameworks is that they are sometimes a bit of a black box (open source or not). Your best bet to ensure that you’re achieving the behaviour that you intend, is to write unit tests. It also helps to have them when submitting bugs.
> 






  Our goal is to write the following result map:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  

	  

	  

	  

	  

  

  
[/code]


  But let&#8217;s take a step at the time. We are going to start retrieving only the Blog data, so our initial result map and query is going to look like this:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  

	  

	  

  



	  
SELECT idBlog, name as blogname, url as blogurl FROM BLOG
  

  
[/code]


  So far, so good. Let&#8217;s take another step.


### Association


  Now let&#8217;s also try to retrieve the Author data.


> 
>   The association element deals with a “has-one” type relationship. For example, in our example, a Blog has one Author. An association mapping works mostly like any other result. You specify the target property, the column to retrieve the value from, the javaType of the property (which MyBatis can figure out most of the time), the jdbcType if necessary and a typeHandler if you want to override the retrieval of the result values.
> 
> 
> 
>   Where the association differs is that you need to tell MyBatis how to load the association. MyBatis can do so in two different ways:
> 
> 
>   * Nested Select: By executing another mapped SQL statement that returns the complex type desired.
>   * Nested Results: By using nested result mappings to deal with repeating subsets of joined results.

We are going to take a look at the Nested Select first.

Here is our resultMap with Author association.

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  

	  

	  

	  

  



	  
SELECT idBlog, name as blogname, url as blogurl FROM BLOG
  



	  
SELECT idAuthor as id, name, email FROM AUTHOR WHERE idBlog = #{idBlog}
  

  
[/code]

Take a look at the _select=&#8221;selectAuthor&#8221;_ atribute. This means MyBatis is going to execute the author select statment to retrieve all the authors that belong to the blog. To make the relationship between blog and author we specify the _column=&#8221;idBlog&#8221;_, so we can filter the authors list.

Note that we set the javaType=&#8221;Author&#8221;. We are using an Alias (remember?). This is because the columns we are retrieving from database match with Author atributes, so we do not need to specify a resultMap for author.

> That’s it. We have two select statements: one to load the Blog, the other to load the Author, and the Blog’s resultMap describes that the “selectAuthor” statement should be used to load its author property.
> 
> All other properties will be loaded automatically assuming their column and property names match.

> While this approach is simple, it will not perform well for large data sets or lists. This problem is knownas the “**N+1 Selects Problem**”. In a nutshell, the N+1 selects problem is caused like this:
> 
>   * You execute a single SQL statement to retrieve a list of records (the “+1”).
>   * For each record returned, you execute a select statement to load details for each (the “N”).
> 
> This problem could result in hundreds or thousands of SQL statements to be executed. This is notalways desirable.The upside is that MyBatis can lazy load such queries, thus you might be spared the cost of thesestatements all at once. However, if you load such a list and then immediately iterate through it toaccess the nested data, you will invoke all of the lazy loads, and thus performance could be very bad.

We are going to show how to avoid the N+1 Select Problem later.

### Collection

We are retrieving Blog and Author information from database. So we have to retrieve the Post information now. And a Blog contains a list of Posts, and a Post contains a list of Tags. We are dealing with two relationships here: first one is a **one-to-many** (Blog-Post) and the second one is a **many-to-many** (Post-Tag). We are going to show you how to do it.

We are also going to use a Nested Select to retrieve Posts.

Let&#8217;s take a look at the resultMap with Post collection:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  

	  

	  

	  

	  

  

  
[/code]

> The collection element works almost identically to the association. In fact, it’s so similar, to document the similarities would be redundant. So let’s focus on the differences.To continue with our example above, a Blog only had one Author. But a Blog has many Posts.
> 
> To map a set of nested results to a List like this, we use the collection element. Just like the association element, we can use a nested select, or nested results from a join.
> 
> There are a number things you’ll notice immediately, but for the most part it looks very similar to the association element we learned about above. First, you’ll notice that we’re using the collection element. Then you’ll notice that there’s a new “ofType” attribute. This attribute is necessary to distinguish between the JavaBean (or field) property type and the type that the collection contains.

To handle the Many-to-Many relationship between Post and Tag, we are also going to use a collection element, but we don&#8217;t need to use nested results for it:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  

	  

	  

  



	  

	  

  



	  
SELECT
	  
P.idPost as idPost, P.title as title,
	  
T.idTag as idTag, T.value as value
	  
FROM Post P
	  
left outer join Post_Tag PT on P.idPost = PT.idPost
	  
left outer join Tag T on PT.idTag = T.idTag
	  
WHERE P.idBlog = #{idBlog}
  

  
[/code]

And we are done! Let&#8217;s see how the _**Blog.xml**_ file looks like:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





	      

	      

	      

	      

	      

      



	      

	      

	      

      



    	  

    	  

      



    	  
SELECT idBlog, name as blogname, url as blogurl FROM BLOG
      



   		  
SELECT idAuthor as id, name, email FROM AUTHOR WHERE idBlog = #{idBlog}
     



   		  
SELECT
			  
P.idPost as idPost, P.title as title,
		      
T.idTag as idTag, T.value as value
		  
FROM Post P
      		  
left outer join Post_Tag PT on P.idPost = PT.idPost
		  	  
left outer join Tag T on PT.idTag = T.idTag
		  
WHERE P.idBlog = #{idBlog}
     



  
[/code]

### Solution to N+1 Selects Problem

As you could read above, the **N+1 Selects Problem** can happen while you are retrieving data.

How to solve it?

Using **Nested Results**: By using nested result mappings to deal with repeating subsets of joined results.

What we have to do is to write a single query to retrieve all the data (Blog + Author + Posts + Tags), and hadle the mapping in a single ResultMapping.

> 
>   Very Important: id elements play a very important role in Nested Result mapping. You should alwaysspecify one or more properties that can be used to uniquely identify the results. The truth is that MyBatis will still work if you leave it out, but at a severe performance cost. Choose as few properties as possible that can uniquely identify the result. The primary key is an obvious choice (even if composite).
> 


  This is how the Blog.xml will look like is we use NestedResults:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





	      

	      

	      

	      

	    	  

			  

			  

	      

	      

	    	  

	    	  

	    	  

	    		  

    			  

	    	  

	      

      



    	  
SELECT
		      
B.idBlog as idBlog, B.name as blogName, B.url as url,
		      
A.idAuthor as idAuthor, A.name as authorName, A.email as email ,
        	  
P.idPost as idPost, P.title as title,
		      
T.idTag as idTag, T.value as value
		  
FROM BLOG as B
      		  
left outer join Author A on B.idBlog = A.idBlog
      		  
left outer join Post P on P.idBlog = B.idBlog
      		  
left outer join Post_Tag PT on P.idPost = PT.idPost
		  	  
left outer join Tag T on PT.idTag = T.idTag
      



  
[/code]


  Notice that this is a best practice. You should try to avoid the N+1 Selects problem.


## 3 &#8211; BlogDAO


  Now that we have all the configuration we need, let&#8217;s write our DAO:



  There are 2 methods: the first one will retrieve the blog data using the first approach: Nested Select and the second method will use the second approach: Nested Results.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
  
import org.apache.ibatis.session.SqlSessionFactory;

import com.loiane.model.Blog;

public class BlogDAO {

/**
	   
* Returns the list of all Contact instances from the database.
	   
* @return the list of all Contact instances from the database.
	   
*/
	  
@SuppressWarnings("unchecked")
	  
public List select(){

SqlSessionFactory sqlSessionFactory = MyBatisConnectionFactory.getSqlSessionFactory();
		  
SqlSession session = sqlSessionFactory.openSession();

try {
			  
List list = session.selectList("Blog.selectBlog");
			  
return list;
		  
} finally {
			  
session.close();
		  
}
	  
}

/**
	   
* Returns the list of all Contact instances from the database avoiding the N + 1
	   
* problem
	   
* @return the list of all Contact instances from the database.
	   
*/
	  
@SuppressWarnings("unchecked")
	  
public List selectN1ProblemSolution(){

SqlSessionFactory sqlSessionFactory = MyBatisConnectionFactory.getSqlSessionFactory();
		  
SqlSession session = sqlSessionFactory.openSession();

try {
			  
List list = session.selectList("BlogBestPractice.selectBlogBestPractice");
			  
return list;
		  
} finally {
			  
session.close();
		  
}
	  
}
  
}
  
[/code]

## 4 &#8211; Annotations


  As there is an article explaining iBatis/MyBatis annotations already, I am going to list the differents annotations, ok?



  We are going to write 3 selects (one for Blog, another one for Author and another one for Posts and Tags). It is the same thing we did using XML:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.data;

import java.util.List;

import org.apache.ibatis.annotations.Many;
  
import org.apache.ibatis.annotations.One;
  
import org.apache.ibatis.annotations.Result;
  
import org.apache.ibatis.annotations.Results;
  
import org.apache.ibatis.annotations.Select;

import com.loiane.model.Author;
  
import com.loiane.model.Blog;
  
import com.loiane.model.Post;

public interface BlogMapper {

final String SELECT_POSTS = "SELECT P.idPost as idPost, P.title as title, T.idTag as idTag, T.value as value " +
			  
"FROM Post P left outer join Post_Tag PT on P.idPost = PT.idPost " +
			  
"left outer join Tag T on PT.idTag = T.idTag WHERE P.idBlog = #{idBlog}";

/**
	   
* Returns the list of all Blog instances from the database.
	   
* @return the list of all Blog instances from the database.
	   
*/
	  
@Select("SELECT idBlog, name as blogname, url as blogurl FROM BLOG")
	  
@Results(value = {
		  
@Result(property="id", column="idBlog"),
		  
@Result(property="name", column="blogname"),
		  
@Result(property="url", column="blogurl"),
		  
@Result(property="author", column="idBlog", javaType=Author.class, one=@One(select="selectAuthor")),
		  
@Result(property="posts", column="idBlog", javaType=List.class, many=@Many(select="selectBlogPosts"))
	  
})
	  
List selectAllBlogs();

/**
	   
* Returns the list of all Author instances from the database of a Blog
	   
* @param idBlog
	   
* @return the list of all Author instances from the database of a Blog
	   
*/
	  
@Select("SELECT idAuthor as id, name, email FROM AUTHOR WHERE idBlog = #{idBlog}")
	  
Author selectAuthor(String idBlog);

/**
	   
* Returns the list of all Post instances from the database of a Blog
	   
* @param idBlog
	   
* @return the list of all Post instances from the database of a Blog
	   
*/
	  
@Select(SELECT_POSTS)
	  
@Results(value = {
		  
@Result(property="id", column="idPost"),
		  
@Result(property="title", column="title"),
		  
@Result(property="tags", column="idPost", javaType=List.class, many=@Many)
	  
})
	  
List selectBlogPosts(String idBlog);

}
  
[/code]


  We are going to set the has-one or has-many relationships using @One or @Many annotations.


### **@**Result

> 
>   A single result mapping between a column and a property or field.
> 
> 
> 
>   Attributes: id, column, property, javaType, jdbcType, typeHandler, one, many.
> 
> 
> 
>   The id attribute is a boolean value that indicates that the property should be used for comparisons (similar to  in the XML mappings).
> 
> 
> 
>   The one attribute is for single associations, similar to , and the many attribute is for collections, similar to . They are named as they are to avoid class naming conflicts.
> 

### @One

> 
>   A mapping to a single property value of a complex type.
> 
> 
> 
>   Attributes: select, which is the fully qualified name of a mapped statement (i.e. mapper method) that can load an instance of the appropriate type.
> 
> 
> 
>   Note: You will notice that join mapping is not supported via the Annotations API. This is due to the limitation in Java Annotations that does not allow for circular references.
> 

### @Many

> 
>   A mapping to a collection property of a complex types.
> 
> 
> 
>   Attributes: select, which is the fully qualified name of a mapped statement (i.e. mapper method) that can load a collection of instances of the appropriate types.
> 
> 
> 
>   Note: You will notice that join mapping is not supported via the Annotations API. This is due to the limitation in Java Annotations that does not allow for circular references.
> 


  5 &#8211; SqlMapConfig.xml


This is how our SqlMapConfig.xml looks like:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





		  

		  

		  

		  

	  



		  

		    

			  

				  

				  

				  

				  

			  

	     

	  



  	     

  	     

      



  
[/code]

## 6 &#8211; MyBatisConnectionFactory


  As you can see, we set alias and 2 mappers on the SqlMapConfig.xml. But we also have a annotation mapper in this project.



  We have to set it on the MyBatisConnectionFactory. This is how you can use both: XML and annotations, though I thing it is best if you use only one.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;27&#8243;]
  
package com.loiane.dao;

import java.io.FileNotFoundException;
  
import java.io.IOException;
  
import java.io.Reader;

import org.apache.ibatis.io.Resources;
  
import org.apache.ibatis.session.SqlSessionFactory;
  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.loiane.data.BlogMapper;

public class MyBatisConnectionFactory {

private static SqlSessionFactory sqlSessionFactory;

static {

try {

String resource = "SqlMapConfig.xml";
			  
Reader reader = Resources.getResourceAsReader(resource);

if (sqlSessionFactory == null) {
				  
sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);

sqlSessionFactory.getConfiguration().addMapper(BlogMapper.class);
			  
}
		  
}

catch (FileNotFoundException fileNotFoundException) {
			  
fileNotFoundException.printStackTrace();
		  
}
		  
catch (IOException iOException) {
			  
iOException.printStackTrace();
		  
}
	  
}

public static SqlSessionFactory getSqlSessionFactory() {

return sqlSessionFactory;
	  
}
  
}
  
[/code]


  Download



  If you want to download the complete sample project, you can get it from my GitHub account: https://github.com/loiane/ibatis-handling-joins



  If you want to download the zip file of the project, just click on download:



  



  There are more articles about iBatis to come. Stay tooned!



  Happy Coding! 
