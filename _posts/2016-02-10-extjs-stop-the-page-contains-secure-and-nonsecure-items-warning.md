---
layout: post
title: 'ExtJS: Stop the "page contains secure and nonsecure items" Warning'
published: true
author: Loiane
comments: true
date: 2010-05-24 08:05:14
tags:
    - ExtJS
    - https
    - nonsecure items
    - security violation pop-up
categories:
    - extjs
permalink: >
    /2010/05/extjs-stop-the-page-contains-secure-and-nonsecure-items-warning
---

  Are your SSL web pages plagued by the browser warning &#8220;This page contains both secure and nonsecure items. Do you want to display the nonsecure items?&#8220;


[][1]


  This is a common error that occurs when some element on a secure web page (one that is loaded with https:// in the address bar) is not being loaded from a secure source. This usually occurs with images, frames, iframes, Flash, and JavaScripts.


### How to solve this &#8220;_issue_&#8221; if the system was developed with ExtJS?

There is a property in Ext class named [BLANK\_IMAGE\_URL][2]:

> 
>   &#8220;URL to a 1&#215;1 transparent gif image used by Ext to create inline icons with CSS background images. (Defaults to &#8220;http://extjs.com/s.gif&#8221; and you should change this to a URL on your server).&#8221;
> 

Following are given a screenshot from Firebug when I loaded an application which uses ExtJS:


  



  The solution is very simple. You need to point BLANK_IMAGE_URL to &#8220;s.gif&#8221; image that is in your application&#8217;s directory.



  For example:  if your application&#8217;s name is &#8220;extjs-crud-grid&#8220;, you should point it to:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.onReady(function(){
       
Ext.BLANK\_IMAGE\_URL = &#8216;/extjs-crud-grid/ext-3.1.1/resources/images/default/s.gif&#8217;;
  
)};
  
[/code]

_extjs-crud-grid_ application&#8217;s directory:

[][3]

Hope it helped!

Happy coding! 

 [1]: http://loianegroner.com/wp-content/uploads/2010/05/ie7-security-warning.gif
 [2]: http://www.extjs.com/deploy/ext/docs/output/Ext.html#BLANK_IMAGE_URL
 [3]: http://loianegroner.com/wp-content/uploads/2010/05/extjs_nonsecure_warning_02.gif