---
layout: post
title: Jetty and Eclipse Integration in 3 Steps
published: true
author: Loiane
comments: true
date: 2012-06-14 05:06:54
tags:
    - Eclipse
    - Jetty
    - Web Application
categories:
    - jetty
permalink: /2012/06/jetty-and-eclipse-integration-in-3-steps
image:
    feature: Jetty_loiane_10.png
---
This tutorial will walk you through out how to integrate Jetty and Eclipse and how to run a web application on Jetty server inside Eclipse.

## Steps:

  1. Install Jetty Eclipse plugin
  2. Create web application
  3. Run web application

## 1 &#8211; Installing Jetty Eclipse Plugin

  1. When you add a server to the Servers view, you will not see an option for Jetty as you will find for Tomcat, JBoss, Apache, etc.
  2. First you need to install a plugin.
  3. Go to Eclipse -> Install new Software menu.
  4. Click on add and type Jetty for Name and http://run-jetty-run.googlecode.com/svn/trunk/updatesite for Location.
  5. Select the Jetty plugin to install. Click on Next and follow the installation:


  2 &#8211; Creating a Web Application



  When you restart Eclipse, got o Project Explorer view or the New menu and click on New -> Dynamic Web Project:



  



  Configure the Project, create a name for it and click on Next:



  



  Click on Next:



  



  Configure the Web Module:



  



  And the project is create. Create also a index.html file. The project structure should look like this:



  



  3 &#8211; Running the Web Application



  Select the application you want to run on Jetty.



  Click on the Run button -> Run Configurations.



  



  Configure your app on Jetty as shown in the picture bellow and click on Run:



  



  Wait for the server to start. You should get something like the following on your log:



  



  Open a browser and test the application!



  



  Done!



  Happy Coding!
