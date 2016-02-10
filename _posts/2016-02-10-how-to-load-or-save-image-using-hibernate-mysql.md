---
layout: post
title: 'How to Load or Save Image using Hibernate - MySQL'
published: true
author: Loiane
comments: true
date: 2011-10-18 09:10:42
tags:
    - annotations
    - Hibernate
    - hibernate 3
    - Hibernate 3.5
    - hibernate annotations
    - Load Image Hibernate
    - Save Image Hibernate
categories:
    - hibernate
permalink: /2011/10/how-to-load-or-save-image-using-hibernate-mysql
image:
    feature: hibernate-loiane.png
---
This tutorial will walk you throughout how to save and load an image from database (MySQL) using Hibernate.

### Requirements

For this sampel project, we are going to use:

  * Eclipse IDE (you can use your favorite IDE);
  * MySQL (you can use any other database, make sure to change the column type if required);
  * Hibernate jars and dependencies (you can download the sample project with all required jars);
  * JUnit &#8211; for testing (jar also included in the sample project).

### PrintScreen

When we finish implementing this sample projeto, it should look like this:

[][1]

### Database Model

[][2]

Before we get started with the sample projet, we have to run this sql script into MySQL:

[code lang=&#8221;sql&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
DROP SCHEMA IF EXISTS \`blog\` ;
  
CREATE SCHEMA IF NOT EXISTS \`blog\` DEFAULT CHARACTER SET latin1 COLLATE latin1\_swedish\_ci ;
  
USE \`blog\` ;

&#8212; &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;
  
&#8212; Table \`blog\`.\`BOOK\`
  
&#8212; &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;
  
DROP TABLE IF EXISTS \`blog\`.\`BOOK\` ;

CREATE TABLE IF NOT EXISTS \`blog\`.\`BOOK\` (
    
\`BOOK\_ID\` INT NOT NULL AUTO\_INCREMENT ,
    
\`BOOK_NAME\` VARCHAR(45) NOT NULL ,
    
\`BOOK_IMAGE\` MEDIUMBLOB NOT NULL ,
    
PRIMARY KEY (\`BOOK_ID\`) )
  
ENGINE = InnoDB;
  
[/code]

This script will create a table _**BOOK**_, which we are going to use in this tutorial.

### Book POJO

We are going to use a simple _**POJO**_ in this project. A _Book_ has an _ID_, a _name_ and an _image_, which is represented by an array of _bytes_.

As we are going to persist an image into the database, we have to use the **_BLOB_** type. MySQLhas some variations of _BLOBs_, you can check the difference between them here. In this example, we are going to use the **_Medium Blob_**, which can store _L + 3 bytes, where L < 2^24_.

Make sure you do not forget to add the _column definition_ on the _Column_ annotation.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;22,23&#8243;]
  
package com.loiane.model;

import javax.persistence.Column;
  
import javax.persistence.Entity;
  
import javax.persistence.GeneratedValue;
  
import javax.persistence.Id;
  
import javax.persistence.Lob;
  
import javax.persistence.Table;

@Entity
  
@Table(name="BOOK")
  
public class Book {

@Id
	  
@GeneratedValue
	  
@Column(name="BOOK_ID")
	  
private long id;

@Column(name="BOOK_NAME", nullable=false)
	  
private String name;

@Lob
	  
@Column(name="BOOK_IMAGE", nullable=false, columnDefinition="mediumblob")
	  
private byte[] image;

public long getId() {
		  
return id;
	  
}

public void setId(long id) {
		  
this.id = id;
	  
}

public String getName() {
		  
return name;
	  
}

public void setName(String name) {
		  
this.name = name;
	  
}

public byte[] getImage() {
		  
return image;
	  
}

public void setImage(byte[] image) {
		  
this.image = image;
	  
}
  
}
  
[/code]

### Hibernate Config

This configuration file contains the required info used to connect to the database.

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

  

      

          
com.mysql.jdbc.Driver
          
jdbc:mysql://localhost/blog
          
root
          
root
          
org.hibernate.dialect.MySQLDialect
    	  
1
    	  
true
      

  

  
[/code]

### Hibernate Util

The _HibernateUtil_ class helps in creating the _SessionFactory_ from the Hibernate configuration file.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.hibernate;

import org.hibernate.SessionFactory;
  
import org.hibernate.cfg.AnnotationConfiguration;

import com.loiane.model.Book;

public class HibernateUtil {

private static final SessionFactory sessionFactory;

static {
		  
try {
			  
sessionFactory = new AnnotationConfiguration()
								  
.configure()
								  
.addPackage("com.loiane.model") //the fully qualified package name
								  
.addAnnotatedClass(Book.class)
								  
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

### DAO

In this class, we created two methods: one to save a _Book_ instance into the database and another one to load a _Book_ instance from the database.
  
[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import org.hibernate.HibernateException;
  
import org.hibernate.Session;
  
import org.hibernate.Transaction;

import com.loiane.hibernate.HibernateUtil;
  
import com.loiane.model.Book;

public class BookDAOImpl {

/**
	   
* Inserts a row in the BOOK table.
	   
* Do not need to pass the id, it will be generated.
	   
* @param book
	   
* @return an instance of the object Book
	   
*/
	  
public Book saveBook(Book book)
	  
{
		  
Session session = HibernateUtil.getSessionFactory().openSession();
		  
Transaction transaction = null;
		  
try {
			  
transaction = session.beginTransaction();
			  
session.save(book);
			  
transaction.commit();
		  
} catch (HibernateException e) {
			  
transaction.rollback();
			  
e.printStackTrace();
		  
} finally {
			  
session.close();
		  
}
		  
return book;
	  
}

/**
	   
* Delete a book from database
	   
* @param bookId id of the book to be retrieved
	   
*/
	  
public Book getBook(Long bookId)
	  
{
		  
Session session = HibernateUtil.getSessionFactory().openSession();
		  
try {
			  
Book book = (Book) session.get(Book.class, bookId);
			  
return book;
		  
} catch (HibernateException e) {
			  
e.printStackTrace();
		  
} finally {
			  
session.close();
		  
}
		  
return null;
	  
}
  
}
  
[/code]

### Test

To test it, first we need to create a _Book_ instance and set an image to the image attribute. To do so, we need to load an image from the hard drive, and we are going to use the one located in the images folder. Then we can call the DAO class and save into the database.

Then we can try to load the image. Just to make sure it is the same image we loaded, we are going to save it in the hard drive.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.test;

import static org.junit.Assert.assertNotNull;

import java.io.File;
  
import java.io.FileInputStream;
  
import java.io.FileOutputStream;

import org.junit.AfterClass;
  
import org.junit.BeforeClass;
  
import org.junit.Test;

import com.loiane.dao.BookDAOImpl;
  
import com.loiane.model.Book;

public class TestBookDAO {

private static BookDAOImpl bookDAO;

@BeforeClass
	  
public static void runBeforeClass() {
		  
bookDAO = new BookDAOImpl();
	  
}

@AfterClass
	  
public static void runAfterClass() {
		  
bookDAO = null;
	  
}

/**
	   
* Test method for {@link com.loiane.dao.BookDAOImpl#saveBook()}.
	   
*/
	  
@Test
	  
public void testSaveBook() {

//File file = new File("images\\extjsfirstlook.jpg"); //windows
		  
File file = new File("images/extjsfirstlook.jpg");
          
byte[] bFile = new byte[(int) file.length()];

try {
	          
FileInputStream fileInputStream = new FileInputStream(file);
	          
fileInputStream.read(bFile);
	          
fileInputStream.close();
          
} catch (Exception e) {
	          
e.printStackTrace();
          
}

Book book = new Book();
          
book.setName("Ext JS 4 First Look");
          
book.setImage(bFile);

bookDAO.saveBook(book);

assertNotNull(book.getId());
	  
}

/**
	   
* Test method for {@link com.loiane.dao.BookDAOImpl#getBook()}.
	   
*/
	  
@Test
	  
public void testGetBook() {

Book book = bookDAO.getBook((long) 1);

assertNotNull(book);

try{
        	  
//FileOutputStream fos = new FileOutputStream("images\\output.jpg"); //windows
        	  
FileOutputStream fos = new FileOutputStream("images/output.jpg");
              
fos.write(book.getImage());
              
fos.close();
          
}catch(Exception e){
        	  
e.printStackTrace();
          
}
	  
}
  
}
  
[/code]

To verify if it was really saved, let&#8217;s check the table _Book_:

[][3]

and if we right click&#8230;

[][4]

and choose to see the image we just saved, we will see it:


  


### Source Code Download

You can download the complete source code (or _fork/clone_ the project &#8211; _git_) from:

_**Github**_: https://github.com/loiane/hibernate-image-example

_**BitBucket**_: https://bitbucket.org/loiane/hibernate-image-example/downloads

Happy Coding!

 [1]: http://loianegroner.com/wp-content/uploads/2011/10/hibernate-image-loiane-01.png
 [2]: http://loianegroner.com/wp-content/uploads/2011/10/hibernate-image-loiane-02.png
 [3]: http://loianegroner.com/wp-content/uploads/2011/10/hibernate-image-loiane-03.png
 [4]: http://loianegroner.com/wp-content/uploads/2011/10/hibernate-image-loiane-04.png