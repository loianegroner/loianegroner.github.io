---
layout: post
title: 'Getting Started with iBatis (MyBatis): Annotations'
published: true
author: Loiane
comments: true
date: 2011-02-21 08:02:17
tags:
    - annotations
    - database
    - delete
    - iBatis
    - ibatis annotation
    - insert
    - Java
    - MyBatis
    - MyBatis annotation
    - mySQL
    - select
    - update
categories:
    - ibatis-mybatis
permalink: /2011/02/getting-started-with-ibatis-mybatis-annotations
---

  This tutorial will walk you through how to setup iBatis (MyBatis) in a simple Java project and will present examples using simple insert, update, select and delete statements using annotations.



  



  This is the third tutorial of the iBatis/MyBatis series, you can read the first 2 tutorials on the following links:



  
    Introduction to iBatis (MyBatis), An alternative to Hibernate and JDBC
  
  
    Getting Started with iBatis (MyBatis): XML Configuration
  



  iBatis/ MyBatis 3 offers a new feature: annotations. But it lacks examples and documentation about annotations. I started to explore and read the MyBatis mailing list archive to write this tutorial.



  Another thing you will notice is the limitation related to annotations. I am going to demonstrate some examples that you  can do with annotations in this tutorial. All the power of iBatis is in its XMl configuration.



  So let’s play a little bit with annotations. It is much simpler and you can use it for simple queries in small projects. As I already mentioned, if you want something more complex, you will have to use XML configuration.



  One more thing before we get started: this tutorial is the same as the previous one, but instead of XML, we are going to use annotations.



  Pre-Requisites



  For this tutorial I am using:



  IDE: Eclipse (you can use your favorite one) DataBase: MySQL Libs/jars: Mybatis, MySQL conector and JUnit (for testing)



  This is how your project should look like:



  



  Sample Database



  Please run this script into your database before getting started with the project implementation:



  I will not post the sample database here again. You can get it from the previous post about iBatis or download this sample project. The files are the same.



  1 &#8211; Contact POJO



  We will create a POJO class first to respresent a contact with id, name, phone number and email address &#8211; same as previous post.



  2 &#8211; ContactMapper



  In this file, we are going to set up all the queries using annotations. It is the MyBatis-Interface for the SQLSessionFactory.


A mapper class is simply an interface with method definitions that match up against the SqlSession methods.


  You can create some strings with the SQL code. Remember it has to be the same code as in XML Configuration.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.data;

import java.util.List;

import org.apache.ibatis.annotations.Delete;
  
import org.apache.ibatis.annotations.Insert;
  
import org.apache.ibatis.annotations.Options;
  
import org.apache.ibatis.annotations.Param;
  
import org.apache.ibatis.annotations.Result;
  
import org.apache.ibatis.annotations.Results;
  
import org.apache.ibatis.annotations.Select;
  
import org.apache.ibatis.annotations.Update;

import com.loiane.model.Contact;

public interface ContactMapper {

final String SELECT_ALL = "SELECT * FROM CONTACT";
	  
final String SELECT\_BY\_ID = "SELECT * FROM CONTACT WHERE CONTACT_ID = #{id}";
	  
final String UPDATE = "UPDATE CONTACT SET CONTACT\_EMAIL = #{email}, CONTACT\_NAME = #{name}, CONTACT\_PHONE = #{phone} WHERE CONTACT\_ID = #{id}";
	  
final String UPDATE\_NAME = "UPDATE CONTACT SET CONTACT\_NAME = #{name} WHERE CONTACT_ID = #{id}";
	  
final String DELETE = "DELETE FROM CONTACT WHERE CONTACT_ID = #{id}";
	  
final String INSERT = "INSERT INTO CONTACT (CONTACT\_EMAIL, CONTACT\_NAME, CONTACT_PHONE) VALUES (#{name}, #{phone}, #{email})";

/**
	   
* Returns the list of all Contact instances from the database.
	   
* @return the list of all Contact instances from the database.
	   
*/
	  
@Select(SELECT_ALL)
	  
@Results(value = {
		  
@Result(property="id", column="CONTACT_ID"),
		  
@Result(property="name", column="CONTACT_NAME"),
		  
@Result(property="phone", column="CONTACT_PHONE"),
		  
@Result(property="email", column="CONTACT_EMAIL")
	  
})
	  
List selectAll();

/**
	   
* Returns a Contact instance from the database.
	   
* @param id primary key value used for lookup.
	   
* @return A Contact instance with a primary key value equals to pk. null if there is no matching row.
	   
*/
	  
@Select(SELECT\_BY\_ID)
	  
@Results(value = {
		  
@Result(property="id"),
		  
@Result(property="name", column="CONTACT_NAME"),
		  
@Result(property="phone", column="CONTACT_PHONE"),
		  
@Result(property="email", column="CONTACT_EMAIL")
	  
})
	  
Contact selectById(int id);

/**
	   
* Updates an instance of Contact in the database.
	   
* @param contact the instance to be updated.
	   
*/
	  
@Update(UPDATE)
	  
void update(Contact contact);

/**
	   
* Updates an instance of Contact in the database.
	   
* @param name name value to be updated.
	   
* @param id primary key value used for lookup.
	   
*/
	  
void updateName(@Param("name") String name, @Param("id") int id);

/**
	   
* Delete an instance of Contact from the database.
	   
* @param id primary key value of the instance to be deleted.
	   
*/
	  
@Delete(DELETE)
	  
void delete(int id);

/**
	   
* Insert an instance of Contact into the database.
	   
* @param contact the instance to be persisted.
	   
*/
	  
@Insert(INSERT)
	  
@Options(useGeneratedKeys = true, keyProperty = "id")
	  
void insert(Contact contact);
  
}
  
[/code]


  @Select



  The @Select annotation is very simple. Let&#8217;s take a look at the first select statment of this class: selectAll. Note that you don&#8217;t need to use the mapping if your database table columns mach the name of the class atributes. I used different names to use the @Result annotation. If table columns match with atribute names, you don&#8217;t need to use the @Result annotation.



  Not let&#8217;s take a look on the second method: selectById. Notice that we have a parameter. It is a simple parameter &#8211; easy to use when you have a single parameter.



  @Update



  Let&#8217;s say you want to update all the columns. You can pass the object as parameter and iBatis will do all the magic for you. Remember to mach the parameter name with the atribute name, otherwise iBatis can get confused,



  Now let&#8217;s say you want to use 2 or 3 paramaters, and they don&#8217;t belong to an object. If you take a look at iBatis XML configuration you will see you have to set the option parameterType (remember?) and specify you parameter type in it, in another words, you can use only one parameter. If you are using annotation, you can use more than one parameter using the @Param annotation.



  @Delete



  The @Delete annotation is also very simple. It follows the previous rules related to parameters.



  @Insert



  The @Insert annotation also follows the rules related to parameters. You can use an object or use the @Param annotation to specify more than one parameter.



  What about the generation key? Well, if your database supports auto generation key, you can set it up using annotations with the @Options annotation. You will need to specify the option useGeneratedKeys and keyProperty. If your database does not support auto generation key, sorry, but I still did not figure out how to do it (in last case, use can do it manually and then pass as parameter to your insert query).



  3 &#8211; MyBatisConnectionFactory



  Every MyBatis application centers around an instance of SqlSessionFactory. A SqlSessionFactory instance can be acquired by using the SqlSessionFactoryBuilder. SqlSessionFactoryBuilder can build a SqlSessionFactory instance from an XML configuration file, of from a custom prepared instance of the Configuration class.



  An observation about this file: on the previous example, we set the mappers on the iBatis Configuration XML file. Using annotations, we will set the mapper manually. In a future example, I&#8217;ll show you how to work with XML and Annotations together.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;27&#8243;]
  
package com.loiane.dao;

import java.io.FileNotFoundException;
  
import java.io.IOException;
  
import java.io.Reader;

import org.apache.ibatis.io.Resources;
  
import org.apache.ibatis.session.SqlSessionFactory;
  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.loiane.data.ContactMapper;

public class MyBatisConnectionFactory {

private static SqlSessionFactory sqlSessionFactory;

static {

try {

String resource = "SqlMapConfig.xml";
			  
Reader reader = Resources.getResourceAsReader(resource);

if (sqlSessionFactory == null) {
				  
sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);

sqlSessionFactory.getConfiguration().addMapper(ContactMapper.class);
			  
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


  4 &#8211; ContactDAO



  Now that we set up everything needed, let&#8217;s create our DAO. To call the sql statments, we need to do one more configuration, that is to set and get the mapper from the SqlSessionFactory. Then we just need to call the mapper method. It is a little bit different, but it does the same thing.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;29,49,68,88&#8243;]
  
package com.loiane.dao;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
  
import org.apache.ibatis.session.SqlSessionFactory;

import com.loiane.data.ContactMapper;
  
import com.loiane.model.Contact;

public class ContactDAO {

private SqlSessionFactory sqlSessionFactory;

public ContactDAO(){
		  
sqlSessionFactory = MyBatisConnectionFactory.getSqlSessionFactory();
	  
}

/**
	   
* Returns the list of all Contact instances from the database.
	   
* @return the list of all Contact instances from the database.
	   
*/
	  
public List selectAll(){

SqlSession session = sqlSessionFactory.openSession();

try {

ContactMapper mapper = session.getMapper(ContactMapper.class);
			  
List list = mapper.selectAll();

return list;
		  
} finally {
			  
session.close();
		  
}
	  
}

/**
	   
* Returns a Contact instance from the database.
	   
* @param id primary key value used for lookup.
	   
* @return A Contact instance with a primary key value equals to pk. null if there is no matching row.
	   
*/
	  
public Contact selectById(int id){

SqlSession session = sqlSessionFactory.openSession();

try {

ContactMapper mapper = session.getMapper(ContactMapper.class);
			  
Contact list = mapper.selectById(id);

return list;
		  
} finally {
			  
session.close();
		  
}
	  
}

/**
	   
* Updates an instance of Contact in the database.
	   
* @param contact the instance to be updated.
	   
*/
	  
public void update(Contact contact){

SqlSession session = sqlSessionFactory.openSession();

try {

ContactMapper mapper = session.getMapper(ContactMapper.class);
			  
mapper.update(contact);

session.commit();
		  
} finally {
			  
session.close();
		  
}
	  
}

/**
	   
* Updates an instance of Contact in the database.
	   
* @param name name value to be updated.
	   
* @param id primary key value used for lookup.
	   
*/
	  
public void updateName(String name, int id){

SqlSession session = sqlSessionFactory.openSession();

try {

ContactMapper mapper = session.getMapper(ContactMapper.class);
			  
mapper.updateName(name, id);

session.commit();
		  
} finally {
			  
session.close();
		  
}
	  
}

/**
	   
* Insert an instance of Contact into the database.
	   
* @param contact the instance to be persisted.
	   
*/
	  
public void insert(Contact contact){

SqlSession session = sqlSessionFactory.openSession();

try {

ContactMapper mapper = session.getMapper(ContactMapper.class);
			  
mapper.insert(contact);

session.commit();
		  
} finally {
			  
session.close();
		  
}
	  
}

/**
	   
* Delete an instance of Contact from the database.
	   
* @param id primary key value of the instance to be deleted.
	   
*/
	  
public void delete(int id){

SqlSession session = sqlSessionFactory.openSession();

try {

ContactMapper mapper = session.getMapper(ContactMapper.class);
			  
mapper.delete(id);

session.commit();
		  
} finally {
			  
session.close();
		  
}
	  
}
  
}
  
[/code]


  5 &#8211; Mapper Configuration File



  The MyBatis XML configuration file contains settings and properties that have a dramatic effect on how MyBatis behaves.



  This time, we do not need to configure alias or xml config files. We did all the magic with annotations, so we have a simple config file with only the information about the database we want to connect with.


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  


[/code]


  Download



  I suggest you to take a look at the org.apache.ibatis.annotations package and try to find out what each annotation can do. Unfortunatelly, you won&#8217;t find much documentation or examples on MyBatis website.



  I also created a TestCase class. If you want to download the complete sample project, you can get it from my GitHub account: https://github.com/loiane/ibatis-annotations-helloworld



  If you want to download the zip file of the project, just click on download:



  



  There are more articles about iBatis to come. Stay tooned!



  In next articles, I&#8217;m going to demonstrate how to implement the feature using XML and then Annotations (when it is possible).



  Happy Coding! 
