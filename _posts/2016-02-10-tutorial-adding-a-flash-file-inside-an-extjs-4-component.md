---
layout: post
title: 'Tutorial: Adding a Flash file inside an ExtJS 4 Component'
published: true
author: Loiane
comments: true
date: 2012-07-23 01:07:32
tags:
    - ExtJS 4
    - Flash
categories:
    - ext-js-4
permalink: >
    /2012/07/tutorial-adding-a-flash-file-inside-an-extjs-4-component
image:
    feature: extjs4-flash-video-loiane-01.png
---
Today we are going to learn how to add a flash file (_**.swf**_) inside an ExtJS 4 Component.

First, let&#8217;s check out what we are going to implement:


  


What we are going to need for this example:

  * ExtJS 4 SDK
  * A flash file (I downloaded one from here: )
  * SWFObject library:  (first one from the list)

The first thing to do is to create the project structure. Unzip the **_SWFObject_** lib inside the project and also add the flash file you want to display inside the project directory as well (mine is called airballoon). I also unziped the ExtJS 4 SDK inside the directory and created a file named _**index.html**_. When everything is ready, this is how it is going to look like:


  


Finally, inside the _**index.html**_ file we are going to add the following content:

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  

	  



  

  

	  

	  
Ext.onReady(function(){
		  
var win = Ext.widget(&#8216;window&#8217;, {
		      
title: "Flash animation inside an ExtJS 4 Component!",
		      
layout: &#8216;fit&#8217;,
		      
width: 700,
		      
height: 500,
		      
resizable: true,
		      
items: {
		          
xtype: &#8216;flash&#8217;,
		          
url: &#8216;airballoon/AIRBALLOON.swf&#8217;
		      
}
		  
});
		  
win.show();
	  
});
	  

  

  
[/code]

The flash animation will be displayed inside an ExtJS 4 Window.The class _**Ext.flash.Component**_ (_xtype_: ‘_flash_’) does all the &#8216;magic&#8217; we need, and in the config _**url**_ you just need to add the full path to the flash file(**.swf**).

_**Demo**_: http://loiane.com.br/extjs/extjs4-flash-video/

**_Complete source code_**: https://github.com/loiane/extjs4-flash-video

Happy Coding! ![icon smile Tutorial: Usando Arquivos Flash com ExtJS 4][1]

 [1]: http://www.loiane.com/wp-includes/images/smilies/icon_smile.gif "Tutorial: Usando Arquivos Flash com ExtJS 4"