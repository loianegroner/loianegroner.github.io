---
layout: post
title: >
    Running a Web Application on Jetty Webserver from Eclipse
    IDE
published: true
author: Loiane
comments: true
date: 2011-07-29 07:07:21
tags:
    - Eclipse
    - Jetty
categories:
    - jetty
permalink: >
    /2011/07/running-a-web-application-on-jetty-webserver-from-eclipse-ide
image:
    feature: Jetty_logo_loiane.jpg
---

  This tutorial will walk you through how to run a web application on Jetty webserver directly from Eclipse.





  Requirements:



  
    Install Jetty webserver
  
  
    Install plugin Run-Jetty-Run on Eclipse
  



  As I mentioned on the tutorials listed above, I am trying to use Jetty instead of Tomcat. In this post, I will show how it is easy to deploy a web applicationon Jetty directly from Eclipse.



  Steps:



  1 – Create a Dynamic Web Project (you can right click on the Project Explorer view or go to File -> New…):



  



  2 – Give a name to the project and click on Next:



  



  3 – Click on Next:



  



  4 – Check the option Generate web.xml… and click on Finish:



  



  5 – This is how the project will look like:



  



  6 – Select the project and click on Run Button -> Run Configurations…



  



  7 – Double click on Jetty Webapp. It will create a jetty project configuration under the Jetty Webapp with the project name. Click on Run:



  



  8 – Wait for the server to start:



  



  9 – Test your application!



  



  I also recorded a video with the steps above:






  See? It is very simple!



  The sample project source code is also available on my github: https://github.com/loiane/test-jetty



  Happy Coding! 
