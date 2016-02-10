---
layout: post
title: 'JAXB Custom Binding - Java.util.Date / Spring 3 Serialization'
published: true
author: Loiane
comments: true
date: 2011-06-07 07:06:45
tags:
    - @XmlJavaTypeAdapter
    - java.util.Date
    - JAXB
    - JAXB Custom Binding
    - Spring 3
    - XmlAdapter
categories:
    - spring
permalink: >
    /2011/06/jaxb-custom-binding-java-util-date-spring-3-serialization
image:
    feature: 111-jaxb.gif
---

  JaxB can handle Java.util.Date serialization, but it expects the following format: &#8220;yyyy-MM-ddTHH:mm:ss&#8220;. What if you need to format the date object in another format?



  I had the same issue when I was working with Spring MVc 3 and Jackson JSON Processor, and recently, I faced the same issue working with Spring MVC 3 and JAXB for XML serialization.



  Let&#8217;s digg into the issue:



  Problem:



  I have the following Java Beans which I want to serialize in XML using Spring MVC 3:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

import java.util.Date;

public class Company {

private int id;

private String company;

private double price;

private double change;

private double pctChange;

private Date lastChange;

//getters and setters
  
[/code]


  And I have another object which is going to wrap the POJO above:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

import java.util.List;

import javax.xml.bind.annotation.XmlElement;
  
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name="companies")
  
public class Companies {

@XmlElement(required = true)
	  
private List list;

public void setList(List list) {
		  
this.list = list;
	  
}
  
}
  
[/code]


  In my Spring controller, I&#8217;m going to return a List of Company through the the @ResponseBody annotation &#8211; which is going to serialize the object automatically with JaxB:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
@RequestMapping(value="/company/view.action")
  
public @ResponseBody Companies view() throws Exception {}
  
[/code]


  When I call the controller method, this is what it returns to the view:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;6,14&#8243;]
  

	  

		  
0.02
		  
3m Co
		  
1
		  
2011-09-01T00:00:00-03:00
		  
0.03
		  
71.72
	  

	  

		  
0.42
		  
Alcoa Inc
		  
2
		  
2011-09-01T00:00:00-03:00
		  
1.47
		  
29.01
	  

  

  
[/code]


  Note the date format. It is not the format I expect it to return. I need to serialize the date in the following format: &#8220;MM-dd-yyyy&#8220;



  Solution:



  I need to create a class extending the XmlAdapter and override the marshal and unmarshal methods and in these methods I am going to format the date as I need to:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.util;

import java.text.SimpleDateFormat;
  
import java.util.Date;

import javax.xml.bind.annotation.adapters.XmlAdapter;

public class JaxbDateSerializer extends XmlAdapter{

private SimpleDateFormat dateFormat = new SimpleDateFormat("MM-dd-yyyy");

@Override
      
public String marshal(Date date) throws Exception {
          
return dateFormat.format(date);
      
}

@Override
      
public Date unmarshal(String date) throws Exception {
          
return dateFormat.parse(date);
      
}
  
}
  
[/code]


  And in my Java Bean class, I simply need to add the @XmlJavaTypeAdapter annotation in the get method of the date property.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;23&#8243;]
  
package com.loiane.model;

import java.util.Date;

import javax.xml.bind.annotation.adapters.XmlJavaTypeAdapter;

import com.loiane.util.JaxbDateSerializer;

public class Company {

private int id;

private String company;

private double price;

private double change;

private double pctChange;

private Date lastChange;

@XmlJavaTypeAdapter(JaxbDateSerializer.class)
	  
public Date getLastChange() {
		  
return lastChange;
	  
}
	  
//getters and setters
  
}
  
[/code]


  If we try to call the controller method again, it is going to return the following XML:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;6,14&#8243;]
  

	  

		  
0.02
		  
3m Co
		  
1
		  
09-01-2011
		  
0.03
		  
71.72
	  

	  

		  
0.42
		  
Alcoa Inc
		  
2
		  
09-01-2011
		  
1.47
		  
29.01
	  

  

  
[/code]


  Problem solved!



  Happy Coding! 
