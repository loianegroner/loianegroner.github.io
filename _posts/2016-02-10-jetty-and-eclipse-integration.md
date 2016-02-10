---
layout: post
title: Jetty and Eclipse Integration
published: true
author: Loiane
comments: true
date: 2011-07-26 10:07:44
tags:
    - Eclipse
    - Jetty
categories:
    - jetty
permalink: /2011/07/jetty-and-eclipse-integration
image:
    feature: jetty-logo-80x22.png
---

  This tutorial will walk you through out how to install Jetty on Eclipse, so you can run web applications on Jetty directly from Eclipse IDE.



  



  As I mentioned on a previous post (Installing and running Jetty), I am trying to use Jetty instead of Tomcat. What I like about Tomcat is that Eclipse has native support to it. You can quickly go to the servers view and add a new Tomcat server instance in less than a minute. But Eclipse does not have native support to Jetty, and I did some research of how to do the same easy way with Jetty as well.



  I found this plugin Run-Jetty-Run hosted on Google Code and it is very simple to use. So in this post I will show how to install it within Eclipse and in the next post I will show how it is easy to use.



  There are two ways of installing a plugin on Eclipse now: using Eclipse Marketplace or the old way which is Install new Software.



  Eclipse Marketplace



  The link of Run-Jetty_run Plugin on Marketplace is: http://marketplace.eclipse.org/content/run-jetty-run



  1 &#8211; Open Eclipse, go to the Help -> Eclipse Marketplace:



  



  2 &#8211; Search for run-jetty-run and click on install:



  



  3 &#8211; Confirm the installation of the plugin:



  



  4 &#8211; Review and accept the licence and click on Finish:



  



  5 &#8211; Wait while the plugin is being installed and then restart Eclipse.



  Old way &#8211; Install new Software



  1 &#8211; Open Eclipse, go to the Help -> Install new Software:



  



  2 &#8211; Click on Add, then fill the form with Name:Jetty (or whatever you like) and Location: http://run-jetty-run.googlecode.com/svn/trunk/updatesite 



  



  3 &#8211; Select the plugin and click on Next:



  



  4 &#8211; Confirm the installation of the plugin:



  



  5 &#8211; Review and accept the licence and click on Finish:



  



  6 &#8211; Wait while the plugin is being installed and then restart Eclipse.



  On next post, we will see how to use it.



  Happy Coding! 
