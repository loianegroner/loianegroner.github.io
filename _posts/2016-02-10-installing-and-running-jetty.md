---
layout: post
title: Installing and Running Jetty
published: true
author: Loiane
comments: true
date: 2011-07-06 07:07:20
tags:
    - Jetty
categories:
    - jetty
permalink: /2011/07/installing-and-running-jetty
image:
    feature: Jetty_loiane_02.png
---
This tutorial will walk you through how to download, install and run Jetty &#8211; 100 % Java HTTP and Servlet Container.

If you do not know Jetty, Following is what Wikipedia says about it:

**Jetty** is a pure [Java][1]-based [HTTP server][2] and [servlet][3] container ([Application server][4]) developed as a [free][5] and [open source][6] project as part of the [Eclipse Foundation][7]. It is currently used in products such as [ActiveMQ][8],[1] [Alfresco][9], [2] [Apache Geronimo][10],[3] [Apache Maven][11], [Google App Engine][12],[4] [Eclipse][13],[5] [FUSE][14],[6] [HP OpenView][15], [JBoss][16],[7] [Liferay][17],[8] [Ubuntu][18], [Twitter&#8217;s Streaming API][19][9] and [Zimbra][20].[10] Jetty is also used as a standard Java application server by many open source projects such as [Eucalyptus][21] and [Hadoop][22].

I am doing some experiments, and I decided to use Jetty instead of TomCat.

So let&#8217;s get this server up and running!

## 1 &#8211; Downloading

You can download Jetty from two sources: Eclipse or Codehaus.

  1. http://jetty.codehaus.org/jetty/
  2. http://www.eclipse.org/jetty/downloads.php

The current stable version is 7, so I downloaded this one from Eclipse page.

The compressed file is platform independent. So if you use Mac, Linux or Windows, that is ok, it will run on any OS.

## 2 &#8211; Installing

Simply uncompress the file to a directory. You should have something like this:


  



  Installation is complete! Let&#8217;s get it running.



  3 &#8211; Running Jetty


  1. Open a terminal.
  2. Go to the Jetty installation directory.
  3. Enter the following command:

[code lang=&#8221;bash&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
java -jar start.jar
  
[/code]


  



  Now open a browser and go to localhost to check if Jetty was installed sucessfully:


[code lang=&#8221;bash&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
http://localhost:8080/
  
[/code]

You should get a page like this:


  


And it is done!

Happy coding!

 [1]: http://en.wikipedia.org/wiki/Java_(programming_language) "Java (programming language)"
 [2]: http://en.wikipedia.org/wiki/HTTP_server "HTTP server"
 [3]: http://en.wikipedia.org/wiki/Servlet "Servlet"
 [4]: http://en.wikipedia.org/wiki/Application_server "Application server"
 [5]: http://en.wikipedia.org/wiki/Free_software "Free software"
 [6]: http://en.wikipedia.org/wiki/Open_source "Open source"
 [7]: http://en.wikipedia.org/wiki/Eclipse_Foundation "Eclipse Foundation"
 [8]: http://en.wikipedia.org/wiki/ActiveMQ "ActiveMQ"
 [9]: http://en.wikipedia.org/wiki/Alfresco_(software) "Alfresco (software)"
 [10]: http://en.wikipedia.org/wiki/Apache_Geronimo "Apache Geronimo"
 [11]: http://en.wikipedia.org/wiki/Apache_Maven "Apache Maven"
 [12]: http://en.wikipedia.org/wiki/Google_App_Engine "Google App Engine"
 [13]: http://en.wikipedia.org/wiki/Eclipse "Eclipse"
 [14]: http://en.wikipedia.org/wiki/Fuse_Services_Framework "Fuse Services Framework"
 [15]: http://en.wikipedia.org/wiki/HP_OpenView "HP OpenView"
 [16]: http://en.wikipedia.org/wiki/JBoss "JBoss"
 [17]: http://en.wikipedia.org/wiki/Liferay "Liferay"
 [18]: http://en.wikipedia.org/wiki/Ubuntu_(operating_system) "Ubuntu (operating system)"
 [19]: http://en.wikipedia.org/wiki/HOSEbird "HOSEbird"
 [20]: http://en.wikipedia.org/wiki/Zimbra "Zimbra"
 [21]: http://en.wikipedia.org/wiki/Eucalyptus_(computing) "Eucalyptus (computing)"
 [22]: http://en.wikipedia.org/wiki/Hadoop "Hadoop"