---
layout: post
title: How to use ExtJS 4 + JQuery together
published: true
author: Loiane
comments: true
date: 2011-08-09 07:08:30
tags:
    - Ext JS 4
    - ExtJS
    - ExtJS 4
    - jQuery
categories:
    - ext-js-4
permalink: /2011/08/how-to-use-extjs-4-jquery-together
---
This is an example of how to use Ext JS 4 and JQuery in an application together.

To use Ext JS 4 with any JS frameowork is very simple: you need to import the js framework file (in this case JQuery ) and import Ext JS. ANd you are ready to develop with both frameworks!

It is very basic and simple, but I get some emails asking about it once in a while, so I thought it would be nice to share, in case you did not use both in a project together before.

Following is how the sample project structure looks like:

[][1]

And this is the HTML page:
  
[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  
Ext JS 4 + JQuery




      

  

  

	  

	  
Ext.onReady(function() {
		  
Ext.Msg.alert(&#8216;Alert&#8217;,$(&#8216;#divId&#8217;).text());

});
	  

	  
Using Ext JS 4 + JQuery
  

  

  
[/code]

And the output will be:

[][2]

### Download the source code:

**Github**: https://github.com/loiane/extjs4-jquery

**Google Code (zip file):** https://code.google.com/p/extjs4-jquery/downloads/list

Happy Coding! 

 [1]: http://loianegroner.com/wp-content/uploads/2011/08/extjs4_jquery_loiane.png
 [2]: http://loianegroner.com/wp-content/uploads/2011/08/extjs4_jquery_loiane_01.png