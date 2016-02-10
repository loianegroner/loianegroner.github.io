---
layout: post
title: >
    How to Serialize Java.util.Date with Jackson JSON Processor
    / Spring 3.0
published: true
author: Loiane
comments: true
date: 2010-09-17 08:09:48
tags:
    - Jackson
    - java.util.Date
    - JSON
    - Spring
    - Spring 3
    - Spring MVC
categories:
    - json
permalink: >
    /2010/09/how-to-serialize-java-util-date-with-jackson-json-processor-spring-3-0
---
Quick tip: how to serialize a java.util.Date object with Jackson JSON Processor &#8211; Sprinv MVC 3.


  


### Scenario:

I have the following Java Bean:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

import java.util.Date;

import org.codehaus.jackson.annotate.JsonAutoDetect;

@JsonAutoDetect
  
@Entity
  
public class Company {

private int id;
	  
private double price;
	  
private String company;
	  
private Date date;
	  
private String size;
	  
private byte visible;
  
}
  
[/code]

And I have the following method in my controller:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public @ResponseBody Map view() throws Exception
  
[/code]

Which returns a list of Company.

The _**@ResponseBody**_ annotation instructs Spring MVC to serialize the hashmap to the client. Spring MVC automatically serializes to JSON because the client accepts that content type:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
{"total":27,"data":[{"price":71.72,"company":"3m Co","visible":1,"id":1,"size":"large","date":1188615600000},{"price":29.01,"company":"Aloca
  
Inc","visible":0,"id":2,"size":"medium","date":1185937200000},{"price":83.81,"company":"Altria Group
  
Inc","visible":0,"id":3,"size":"large","date":1186110000000},{"price":52.55,"company":"American Express Company","visible":1,"id":4,"size":"extra
  
large","date":1199412000000},{"price":64.13,"company":"American International Group
  
Inc.","visible":1,"id":5,"size":"small","date":1204599600000},{"price":31.61,"company":"AT&T Inc.","visible":0,"id":6,"size":"extra
  
large","date":1201831200000},{"price":75.43,"company":"Boeing Co.","visible":1,"id":7,"size":"large","date":1199152800000},{"price":67.27,"company":"Caterpillar
  
Inc.","visible":1,"id":8,"size":"medium","date":1196647200000},{"price":49.37,"company":"Citigroup,
  
Inc.","visible":1,"id":9,"size":"large","date":1195869600000},{"price":40.48,"company":"E.I. du Pont de Nemours and Company","visible":0,"id":10,"size":"extra
  
large","date":1178679600000}],"success":true}
  
[/code]

Take a look how the date attibute is being serialized: _**&#8220;date&#8221;:1188615600000**._ Jackson uses default strategy to determine date formatting in serialization.

One of the annotations Jackson has is **_@JsonSerialize_**. You basically use this annotation for configuring serialization aspects. In my case, I decorated by model objects date getter method with this annotation:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
@JsonSerialize(using=JsonDateSerializer.class)
  
public Date getDate() {
	  
return date;
  
}
  
[/code]

**_JsonDateSerializer_** is a class I wrote that extends **_JsonSerializer_** that will handle the serialization for this field:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.util;

import java.io.IOException;
  
import java.text.SimpleDateFormat;
  
import java.util.Date;

import org.codehaus.jackson.JsonGenerator;
  
import org.codehaus.jackson.JsonProcessingException;
  
import org.codehaus.jackson.map.JsonSerializer;
  
import org.codehaus.jackson.map.SerializerProvider;
  
import org.springframework.stereotype.Component;

/**
   
* Used to serialize Java.util.Date, which is not a common JSON
   
* type, so we have to create a custom serialize method;.
   
*
   
* @author Loiane Groner
   
* http://loianegroner.com (English)
   
* http://loiane.com (Portuguese)
   
*/
  
@Component
  
public class JsonDateSerializer extends JsonSerializer{

private static final SimpleDateFormat dateFormat = new SimpleDateFormat("MM-dd-yyyy");

@Override
	  
public void serialize(Date date, JsonGenerator gen, SerializerProvider provider)
			  
throws IOException, JsonProcessingException {

String formattedDate = dateFormat.format(date);

gen.writeString(formattedDate);
	  
}

}
  
[/code]

And this is how my object is being serialized now:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
{"total":27,"data":[{"price":71.72,"company":"3m Co","visible":1,"id":1,"size":"large","date":"09-01-2007"},{"price":29.01,"company":"Aloca
  
Inc","visible":0,"id":2,"size":"medium","date":"08-01-2007"},{"price":83.81,"company":"Altria Group
  
Inc","visible":0,"id":3,"size":"large","date":"08-03-2007"},{"price":52.55,"company":"American Express Company","visible":1,"id":4,"size":"extra
  
large","date":"01-04-2008"},{"price":64.13,"company":"American International Group
  
Inc.","visible":1,"id":5,"size":"small","date":"03-04-2008"},{"price":31.61,"company":"AT&T Inc.","visible":0,"id":6,"size":"extra
  
large","date":"02-01-2008"},{"price":75.43,"company":"Boeing Co.","visible":1,"id":7,"size":"large","date":"01-01-2008"},{"price":67.27,"company":"Caterpillar
  
Inc.","visible":1,"id":8,"size":"medium","date":"12-03-2007"},{"price":49.37,"company":"Citigroup,
  
Inc.","visible":1,"id":9,"size":"large","date":"11-24-2007"},{"price":40.48,"company":"E.I. du Pont de Nemours and Company","visible":0,"id":10,"size":"extra
  
large","date":"05-09-2007"}],"success":true}
  
[/code]

Happy Coding! 