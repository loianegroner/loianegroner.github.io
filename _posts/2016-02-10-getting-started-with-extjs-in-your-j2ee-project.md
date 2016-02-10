---
layout: post
title: Getting started with ExtJS in your J2EE project
published: true
author: Loiane
comments: true
date: 2009-12-01 08:12:33
tags:
    - ExtJS
    - ExtJS + J2EE
    - Hello World ExtJS
    - J2EE
categories:
    - extjs
permalink: /2009/12/getting-started-with-extjs-in-your-j2ee-project
---
This tutorial will walk through how to get an Ext JS installation up and running quickly in your java (J2EE) project.

Level: **Basic**

This is the screenshot of this tutorial:



First, if you haven&#8217;t downloaded ExtJS already, do it: http://extjs.com/products/extjs/download.php (most current release).

PS.: I&#8217;m using Eclipse IDE.

**1 &#8211;** After download the ExtJS library, let&#8217;s create a Dynamic Web Project.

**2** &#8211; Under WebContent folder, create a folder where all ExtJS files will be located (I named it ext-3.0.3). Paste adapter and resources folders under your extjs folder. Also, paste this file:  _ext-all.js_ under _ext-3.0.3_:



**3 &#8211;** Let&#8217;s create a html page. You can use this page as a template and adjust it to your needs:

[code lang=&#8221;html&#8221; toolbar=&#8221;true&#8221;]
  

  

  



  	  

	  





	  



	  






Getting Started with Extjs


  	  

  	  




// Path to the blank image must point to a valid location on your server
	  
Ext.BLANK\_IMAGE\_URL = &#8216;/extjs-helloworld/ext-3.0.3/resources/images/default/s.gif&#8217;;




  



  

  
[/code]

**4 &#8211;** Now, let&#8217;s put a Hello World code to make sure everything is ok:

[code lang=&#8221;js&#8221; toolbar=&#8221;true&#8221;]
	  
Ext.onReady(function(){
		  
Ext.Msg.alert(&#8216;Test Extjs&#8217;, &#8216;Hello World&#8217;);
	  
});
  
[/code]

You can download the source code in my Github:  http://github.com/loiane/extjs-helloworld

Reference: Ext Js Community

Happy Coding!