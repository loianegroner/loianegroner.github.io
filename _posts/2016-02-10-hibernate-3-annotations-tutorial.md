---
layout: post
title: Hibernate 3 Annotations Tutorial
published: true
author: Loiane
comments: true
date: 2010-06-14 09:06:43
tags:
    - Hibernate
    - hibernate 3
    - hibernate annotations
    - mySQL
    - tutorial
categories:
    - hibernate
permalink: /2010/06/hibernate-3-annotations-tutorial
---
This tutorial will walk through how to implement a hello world project using Hibernate Annotations and MySQL database.

### Requirements


  Download and unzip Hibernate Core distribution from the Hibernate website. Hibernate 3.5 and onward contains Hibernate Annotations.


  
    Starting with version 3.5 (currently trunk), Annotations and EntityManager have been merged back into the Hibernate Core codebase as invidual modules.  We will also begin bundling Envers at the same time.  This will significantly help simplify compatibility guidelines.
  



  Download SLF4 lib as well.


  Download the database connector. In this example, I&#8217;m using MySQL database.


### Configuration

First, set up your classpath:

  * Copy **hibernate3.jar** and the required 3rd party libraries available in **lib/required**.
  * Copy **lib/jpa/hibernate-jpa-2.0-api-1.0.0.Final.jar** to your classpath as well.
  * Also copy **slf4j-simple-1.5.8.jar** from SLF4 (I&#8217;ve got an exception without this lbi in my classpath)

The picture will all required libraries is given bellow:

[][1]

Let&#8217;s start by creating our POJO (Plain Old Java Object) class that represents the table in the database.

This is the City table:

[][2]And here is the **City** class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.com.model;

import javax.persistence.Column;
  
import javax.persistence.Entity;
  
import javax.persistence.GeneratedValue;
  
import javax.persistence.Id;
  
import javax.persistence.Table;

@Entity
  
@Table(name="CITY")
  
public class City {

private long id;

private String name;

public City(String name) {
		  
super();
		  
this.name = name;
	  
}

public City() {}

@Id
	  
@GeneratedValue
	  
@Column(name="CITY_ID")
	  
public long getId() {
		  
return id;
	  
}

public void setId(long id) {
		  
this.id = id;
	  
}

@Column(name="CITY_NAME", nullable=false)
	  
public String getName() {
		  
return name;
	  
}

public void setName(String name) {
		  
this.name = name;
	  
}
  
}
  
[/code]

Here is a list of all annotations used and its meaning:

  * **@Entity** declares the class as an entity (i.e. a persistent POJO class)
  * **@Table** is set at the class level; it allows you to define the table, catalog, and schema names for your entity mapping. If no @Table is defined the default values are used: the unqualified class name of the entity.
  * **@Id** declares the identifier property of this entity.
  * **@GeneratedValue** annotation is used to specify the primary key generation strategy to use. If the strategy is not specified by default AUTO will be used.
  * **@Column** annotation is used to specify the details of the column to which a field or property will be mapped. If the @Column annotation is not specified by default the property name will be used as the column name.

You also need to create the **_HibernateUtil_** class. The **_HibernateUtil_** class helps in creating the _SessionFactory_ from the Hibernate configuration file. Its implementation is shown bellow:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.util;

import org.hibernate.SessionFactory;
  
import org.hibernate.cfg.AnnotationConfiguration;

import com.loiane.com.model.City;

public class HibernateUtil {

private static final SessionFactory sessionFactory;

static {
		  
try {
			  
sessionFactory = new AnnotationConfiguration()
								  
.configure()
								  
.addPackage("com.loiane.model") //the fully qualified package name
								  
.addAnnotatedClass(City.class)
								  
.buildSessionFactory();

} catch (Throwable ex) {
			  
System.err.println("Initial SessionFactory creation failed." + ex);
			  
throw new ExceptionInInitializerError(ex);
		  
}
	  
}

public static SessionFactory getSessionFactory() {
		  
return sessionFactory;
	  
}
  
}
  
[/code]

You need to create a _**hibernate.cfg.xml**_ __as well, with all the information needed to connect to the database, such as database url, user, password:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

  

      

          
com.mysql.jdbc.Driver
          
root
          
jdbc:mysql://localhost/blog
          
root
          
org.hibernate.dialect.MySQLDialect
    	  
1
    	  
true
          
create
      

  

  
[/code]

Here is an example of how to select, update, insert and delete a City:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import java.util.List;

import org.hibernate.HibernateException;
  
import org.hibernate.Session;
  
import org.hibernate.Transaction;

import com.loiane.com.model.City;
  
import com.loiane.util.HibernateUtil;

public class CityDAO {

public Long saveCity(String cityName)
	  
{
		  
Session session = HibernateUtil.getSessionFactory().openSession();
		  
Transaction transaction = null;
		  
Long cityId = null;
		  
try {
			  
transaction = session.beginTransaction();
			  
City city = new City();
			  
city.setName(cityName);
			  
cityId = (Long) session.save(city);
			  
transaction.commit();
		  
} catch (HibernateException e) {
			  
transaction.rollback();
			  
e.printStackTrace();
		  
} finally {
			  
session.close();
		  
}
		  
return cityId;
	  
}

@SuppressWarnings("unchecked")
	  
public void listCities()
	  
{
		  
Session session = HibernateUtil.getSessionFactory().openSession();
		  
Transaction transaction = null;
		  
try {
			  
transaction = session.beginTransaction();
			  
List cities = session.createQuery("from City").list();

for (City city : cities){
				  
System.out.println(city.getName());
			  
}

transaction.commit();
		  
} catch (HibernateException e) {
			  
transaction.rollback();
			  
e.printStackTrace();
		  
} finally {
			  
session.close();
		  
}
	  
}

public void updateCity(Long cityId, String cityName)
	  
{
		  
Session session = HibernateUtil.getSessionFactory().openSession();
		  
Transaction transaction = null;
		  
try {
			  
transaction = session.beginTransaction();
			  
City city = (City) session.get(City.class, cityId);
			  
city.setName(cityName);
			  
transaction.commit();
		  
} catch (HibernateException e) {
			  
transaction.rollback();
			  
e.printStackTrace();
		  
} finally {
			  
session.close();
		  
}
	  
}

public void deleteCity(Long cityId)
	  
{
		  
Session session = HibernateUtil.getSessionFactory().openSession();
		  
Transaction transaction = null;
		  
try {
			  
transaction = session.beginTransaction();
			  
City city = (City) session.get(City.class, cityId);
			  
session.delete(city);
			  
transaction.commit();
		  
} catch (HibernateException e) {
			  
transaction.rollback();
			  
e.printStackTrace();
		  
} finally {
			  
session.close();
		  
}
	  
}
  
}
  
[/code]

and the Main class (you can try to run this class):

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.main;

import com.loiane.dao.CityDAO;

public class Main {

public static void main(String[] args) {

CityDAO cityDAO = new CityDAO();

long cityId1 = cityDAO.saveCity("New York");
		  
long cityId2 = cityDAO.saveCity("Rio de Janeiro");
		  
long cityId3 = cityDAO.saveCity("Tokyo");
		  
long cityId4 = cityDAO.saveCity("London");

cityDAO.listCities();

cityDAO.updateCity(cityId4, "Paris");

cityDAO.deleteCity(cityId3);

cityDAO.listCities();
	  
}
  
}
  
[/code]

If your setup is OK, this is the output:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Hibernate: insert into CITY (CITY_NAME) values (?)
  
Hibernate: insert into CITY (CITY_NAME) values (?)
  
Hibernate: insert into CITY (CITY_NAME) values (?)
  
Hibernate: insert into CITY (CITY_NAME) values (?)
  
Hibernate: select city0\_.CITY\_ID as CITY1\_0\_, city0\_.CITY\_NAME as CITY2\_0\_ from CITY city0_
  
New York
  
Rio de Janeiro
  
Tokyo
  
London
  
Hibernate: select city0\_.CITY\_ID as CITY1\_0\_0\_, city0\_.CITY\_NAME as CITY2\_0\_0\_ from CITY city0_ where city0\_.CITY\_ID=?
  
Hibernate: update CITY set CITY\_NAME=? where CITY\_ID=?
  
Hibernate: select city0\_.CITY\_ID as CITY1\_0\_0\_, city0\_.CITY\_NAME as CITY2\_0\_0\_ from CITY city0_ where city0\_.CITY\_ID=?
  
Hibernate: delete from CITY where CITY_ID=?
  
Hibernate: select city0\_.CITY\_ID as CITY1\_0\_, city0\_.CITY\_NAME as CITY2\_0\_ from CITY city0_
  
New York
  
Rio de Janeiro
  
Paris
  
[/code]

[][3]

You do not need to worry about creating the table in the database.The bellow configuration does it for you!

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
create
  
[/code]

For more information about Hibernate Annotations, please read the Hibernate Documentation.

You can download the complete source code (including all the libraries) from my GitHub repository: http://github.com/loiane/hibernate-annotations

I developed this sample project using Eclipse IDE.

Happy Coding!

 [1]: http://loianegroner.com/wp-content/uploads/2010/06/hibernate_3_annotations_tutorial_loiane_01.gif
 [2]: http://loianegroner.com/wp-content/uploads/2010/06/hibernate_3_annotations_tutorial_loiane_02.png
 [3]: http://loianegroner.com/wp-content/uploads/2010/06/hibernate_3_annotations_tutorial_loiane_03.gif