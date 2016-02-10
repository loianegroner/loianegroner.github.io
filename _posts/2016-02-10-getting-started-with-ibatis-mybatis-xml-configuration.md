---
layout: post
title: 'Getting Started with iBatis (MyBatis): XML Configuration'
published: true
author: Loiane
comments: true
date: 2011-02-14 08:02:31
tags:
    - delete
    - iBatis
    - insert
    - Java
    - MyBatis
    - mySQL
    - select
    - update
    - xml configuration
categories:
    - ibatis-mybatis
permalink: >
    /2011/02/getting-started-with-ibatis-mybatis-xml-configuration
image:
    feature: ibatis_mybatis_loiane.png
---

  This tutorial will walk you through how to setup iBatis (MyBatis) in a simple Java project and will present examples using simple insert, update, select and delete statements.



  



  Pre-Requisites



  For this tutorial I am using:



  IDE: Eclipse (you can use your favorite one)  DataBase: MySQL  Libs/jars: Mybatis, MySQL conector and JUnit (for testing)



  This is how your project should look like:



  



  Sample Database



  Please run this script into your database before getting started with the project implementation:


[code lang=&#8221;sql&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
DROP TABLE IF EXISTS \`blog\`.\`contact\`;
  
CREATE TABLE \`blog\`.\`contact\` (
    
\`CONTACT\_ID\` int(11) NOT NULL AUTO\_INCREMENT,
    
\`CONTACT_EMAIL\` varchar(255) NOT NULL,
    
\`CONTACT_NAME\` varchar(255) NOT NULL,
    
\`CONTACT_PHONE\` varchar(255) NOT NULL,
    
PRIMARY KEY (\`CONTACT_ID\`)
  
)
  
ENGINE=InnoDB;

insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact0&#8242;,'(000) 000-0000&#8217;, &#8216;contact0@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact1&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact1@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact2&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact2@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact3&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact3@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact4&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact4@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact5&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact5@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact6&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact6@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact7&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact7@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact8&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact8@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact9&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact9@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact10&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact10@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact11&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact11@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact12&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact12@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact13&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact13@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact14&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact14@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact15&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact15@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact16&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact16@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact17&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact17@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact18&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact18@loianetest.com&#8217;);
  
insert into CONTACT (CONTACT\_NAME, CONTACT\_PHONE, CONTACT_EMAIL) values (&#8216;Contact19&#8217;, &#8216;(000) 000-0000&#8217;, &#8216;contact19@loianetest.com&#8217;);

[/code]


  1 &#8211; Contact POJO



  We will create a POJO class first to respresent a contact with id, name, phone number and email address:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

public class Contact {

private int id;
	  
private String name;
	  
private String phone;
	  
private String email;

public Contact(int id, String name, String phone, String email) {
		  
super();
		  
this.id = id;
		  
this.name = name;
		  
this.phone = phone;
		  
this.email = email;
	  
}

public Contact() {}

//getters and setters
  
}
  
[/code]


  2 &#8211; Contact.xml



  This is the iBatis/myBatis SQL map configuration file for Contact class. We are going to write all the SQL queries, map a query to object in this file &#8211; here is where all the magic happens!


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





	      

	      

	      

	      

      



    	  
SELECT * FROM CONTACT
      



    	  
SELECT * FROM CONTACT WHERE CONTACT_ID = #{id}
  	  



    	  
DELETE from CONTACT WHERE CONTACT_ID = #{id};
      



      
INSERT INTO CONTACT (CONTACT\_EMAIL, CONTACT\_NAME, CONTACT_PHONE)
		  
VALUES (#{name}, #{phone}, #{email});
        

		  
select last\_insert\_id() as id
	    

      



	  	  
UPDATE CONTACT
		  
SET
			  
CONTACT_EMAIL = #{email},
			  
CONTACT_NAME = #{name},
			  
CONTACT_PHONE = #{phone}
		  
WHERE CONTACT_ID = #{id};
    



  
[/code]

What this file contains:

  * _resultMap_ – The most complicated and powerful element that describes how to load your objects from the database result sets.
  * _insert_ – A mapped INSERT statement.
  * _update_ – A mapped UPDATE statement.
  * _delete_ – A mapped DELEETE statement.
  * _select_ – A mapped SELECT statement.


  Result Map


> 
>   The resultMap element is the most important and powerful element in MyBatis. It’s what allows you to do away with 90% of the code that JDBC requires to retrieve data from ResultSets, and in some cases allows you to do things that JDBC does not even support. In fact, to write the equivalent code for something like a join mapping for a complex statement could probably span thousands of lines of code. The design of the ResultMaps is such that simple statements don’t require explicit result mappings at all, and more complex statements require no more than is absolutely necessary to describe the relationships.
> 


  In this example, the name of the table column is different from the Contact class. That is why we have to map the column with the class property. If the column name is the same as the property, you do not need to use the column=&#8221;&#8221; option in the result map.



  And remember that TypeAliases are your friend. Use them so that you don’t have to keep typing the fully qualified path of your class out. &#8211; we are going to set it in the myBatis main configuration file.



  Select statment



  The first select statment in this example is called &#8220;getAll&#8220;, and it means we are going to use this id to call the statment in DAO class. The other option we set is the resultMap, which we mapped to contact class, and it means the statment is going to return a list of contacts (List).



  The second select statment in this example is called &#8220;getById&#8220;. We set a option called parameter of type int (or Integer) and it returns a object of type Contact. Notice the parameter notation #{id}. This tells MyBatis to create a PreparedStatement parameter. With JDBC, such a parameter would be identified by a “?” in SQL passed to a new PreparedStatement.


### Delete Statment


  The delete statment is also very simple. We set a parameter type called id (same thing as getById statment) so we can filter what it is going to be deleted.


### Update Statment


  In the update statement we ser a parameter of type Contact, which means we are going to pass a contact object as parameter to the update method in DAO class. Note the parameter notation #{name}, #{phone}, #{email} and #{id}. All the parameters must have the same name as contact properties, otherwise myBatis will not be able to map the object-parameters.


### Insert Statment

> 
>   Insert is a little bit more rich in that it has a few extra attributes and sub-elements that allow it to deal with key generation in a number of ways. First, if your database supports auto-generated key fields (e.g. MySQL and SQL Server), then you can simply set useGeneratedKeys=”true” and set the keyProperty to the target property and you’re done.
> 
> 
> 
>   MyBatis has another way to deal with key generation for databases that don’t support auto-generated column types, or perhaps don’t yet support the JDBC driver support for auto-generated keys.
> 


  In this example, we are going to set manually the generated id in the object with the selectKey option. In this example, the selectKey would be run after the insert statment and with the last_insert_id() function we will get the last generated key (of type int) and set it to the id property.


## 3 &#8211; Mapper Configuration File

The MyBatis XML configuration file contains settings and properties that have a dramatic effect on how MyBatis behaves. The high level structure of the document is as follows:

  * Configuration 
      * properties
      * settings
      * typeAliases
      * typeHandlers
      * objectFactory
      * plugins
      * environments 
          * environment 
              * transactionManager
              * dataSource
  * mappers


  Hint: you have to follow the order above, otherwise you will get an exception.



  The SqlMapConfig.xml from our project:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





		  

	  



		  

		    

			  

				  

				  

				  

				  

			  

	     

	  



  	     

      



  
[/code]


  Let&#8217;s take a look at the configuration properties we are using.


### Type Aliases

> 
>   A type alias is simply a shorter name for a Java type. It&#8217;s only relevant to the XML configuration and simply exists to reduce redundant typing of fully qualified classnames.
> 


  Remember we used Contact as type in the resultMap property in Contact.xml ()? This is a great help!


### Environments

> 
>   MyBatis can be configured with multiple environments. This helps you to apply your SQL Maps to multiple databases for any number of reasons. For example, you might have a different configuration for your Development, Test and Production environments. Or, you may have multiple production databases that share the same schema, and you’d like to use the same SQL maps for both. There are many use cases.
> 
> 
> 
>   One important thing to remember though: While you can configure multiple environments, you can only choose ONE per SqlSessionFactory instance.
> 
> 
> 
>   The default environment and the environment IDs are self explanatory. Name them whatever you like, just make sure the default matches one of them.
> 

### Transaction Manager

There are two TransactionManager types (i.e. type=”[JDBC|MANAGED]”) that are included with MyBatis:

>   * JDBC – This configuration simply makes use of the JDBC commit and rollback facilities directly. It relies on the connection retrieved from the dataSource to manage the scope of the transaction.
>   * MANAGED – This configuration simply does almost nothing. It never commits, or rolls back a connection. Instead, it lets the container manage the full lifecycle of the transaction (e.g. Spring or a JEE Application Server context). By default it does close the connection. However, some containers don’t expect this, and thus if you need to stop it from closing the connection, set the closeConnection property to false.


  In this example we are going to use JDBC.


### Data Source

The dataSource element configures the source of JDBC Connection objects using the standard JDBC DataSource interface.

  * driver – This is the fully qualified Java class of the JDBC driver (NOT of the DataSource class if your driver includes one).
  * url – This is the JDBC URL for your database instance.
  * username – The database username to log in with.
  * password &#8211; The database password to log in with.

## 4 &#8211; MyBatisConnectionFactory

> 
>   Every MyBatis application centers around an instance of SqlSessionFactory. A SqlSessionFactory instance can be acquired by using the SqlSessionFactoryBuilder. SqlSessionFactoryBuilder can build a SqlSessionFactory instance from an XML configuration file, of from a custom prepared instance of the Configuration class.
> 

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import java.io.FileNotFoundException;
  
import java.io.IOException;
  
import java.io.Reader;

import org.apache.ibatis.io.Resources;
  
import org.apache.ibatis.session.SqlSessionFactory;
  
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class MyBatisConnectionFactory {

private static SqlSessionFactory sqlSessionFactory;

static {
		  
try {

String resource = "SqlMapConfig.xml";
			  
Reader reader = Resources.getResourceAsReader(resource);

if (sqlSessionFactory == null) {
				  
sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
			  
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

## 5 &#8211; ContactDAO


  Now that we set up everything needed, let&#8217;s create our DAO. To call the sql statments, we need to call the namespace and the name of the SQl statment as follows:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
  
import org.apache.ibatis.session.SqlSessionFactory;

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
	  
@SuppressWarnings("unchecked")
	  
public List selectAll(){

SqlSession session = sqlSessionFactory.openSession();

try {
			  
List list = session.selectList("Contact.getAll");
			  
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
			  
Contact contact = (Contact) session.selectOne("Contact.getById",id);
			  
return contact;
		  
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
			  
session.update("Contact.update", contact);
			  
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
			  
session.insert("Contact.insert", contact);
			  
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
			  
session.delete("Contact.deleteById", id);
			  
session.commit();
		  
} finally {
			  
session.close();
		  
}
	  
}
  
}
  
[/code]

## Download


  If you want to learn more about the MyBatis configuration options, please read the User Guide. You will find everything you need there. All the quoted sentences are from the MyBatis 3 User Guid. I also used it as reference to implement this sample project.



  I also created a TestCase class. If you want to download the complete sample project, you can get it from my GitHub account: https://github.com/loiane/ibatis-helloworld



  If you want to download the zip file of the project, just click on download:



  



  Next articles we are going to explore more iBatis/MyBatis options! 



  Happy Coding!
