---
layout: post
title: Spring MVC and AJAX with JSON
published: true
author: Loiane
comments: true
date: 2010-02-15 09:02:23
tags:
    - JSON
    - Json-lib
    - json-lib-ext-spring
    - Spring
    - Spring MVC
categories:
    - json
    - spring
permalink: /2010/02/spring-mvc-and-ajax-with-json
---

  This tutorial will walk through how to configure Spring MVC to return a JSON object to client browser.



  One of the main decisions to be taken while developing AJAX applications is the format of messages passed by the server to the client browser. There are many options to choose from including plain text, XML, CSV etc. One of the more popular choices today is the JavaScript Object Notation (JSON). JSON provides a nice name-value pair data format that is easy to generate and parse.



  How to do it?



  You can use json-lib-ext-spring. There are other libs, this is the one I found. If you know or use another one, please leave a comment with the library name. 



  Do not forget to download Json-lib and its dependencies.



  Now you have to configure your XML files:



  Create a views.xml file under WEB-INF folder and paste the following code into it:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  





  
[/code]


  Add this config to you spring configuration file:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  

		  
/WEB-INF/views.xml
	  

	  

		  
1
	  

  

  
[/code]


  Make sure to set the order if you are using any other view resolvers.



  Now you just have to use &#8220;jsonView&#8221; as the viewname and the model will be converted to JSONbefore being sent back to the client:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
return new ModelAndView("jsonView", modelMap);
  
[/code]


  Here is an example:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
	  
public ModelAndView getColumnsJson(HttpServletRequest request,
			  
HttpServletResponse response) throws Exception {

Map modelMap = new HashMap(2);
		  
modelMap.put("rows", service.generateColumns());
		  
return new ModelAndView("jsonView", modelMap);

}
  
[/code]


  Happy coding!
