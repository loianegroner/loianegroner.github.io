---
layout: post
title: 'Ext.Window Panel: Show or Hide?'
published: true
author: Loiane
comments: true
date: 2010-01-04 08:01:16
tags:
    - Ext.Window
    - ExtJS
categories:
    - extjs
permalink: /2010/01/ext-window-panel-show-or-hide
---

  From this short tutorial you will learn how to control the Ext.Window panel. It explains how to hide or show it on button click.



  



  Problem: you created a Ext.Window, clicked on a button or link (or something else) and a window showed up. You clicked on close button (up right corner) and it closed. You tried again, but the window did not show from second time (and on) &#8211; maybe you got an error message in Firebug console.



  Solution: the default behaviour of this componet is to close. But close means destroy the component, so the second time you tried to open it, it did not work. The solution is to hide the window, so you can reuse it when you need it again.



  Let&#8217;s see some code (Reference: Hello Word Window)



  HTML:


[code lang=&#8221;html&#8221; ffirstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

  
Ext.Window: close or hide




	  





  

	  




Hello Dialog
      

          

          

              
Ext.Window Panel: Close or Hide?
          

      

  



  

  
[/code]

**JS**:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.onReady(function(){

Ext.BLANK\_IMAGE\_URL = &#8216;/ext-window/ext-3.0.3/resources/images/default/s.gif&#8217;;

var win;
      
var button = Ext.get(&#8216;show-btn&#8217;);

var tab = new Ext.TabPanel({
          
applyTo: &#8216;hello-tabs&#8217;,
          
autoTabs:true,
          
activeTab:0,
          
deferredRender:false,
          
border:false
      
});

button.on(&#8216;click&#8217;, function(){

// create the window on the first click and reuse on subsequent clicks
    	  
//cria a janela no primeiro clique e a reusa nos próximos cliques
          
if(!win){
              
win = new Ext.Window({
                  
applyTo:&#8217;hello-win&#8217;,
                  
layout:&#8217;fit&#8217;,
                  
width:500,
                  
height:300,
                  
closeAction:&#8217;hide&#8217;, //&#8217;close&#8217; &#8211; destroy the component
                  
plain: true,

items: tab,

buttons: [{
                      
text: &#8216;Close&#8217;,
                      
handler: function(){
                          
win.hide();
                      
}
                  
}]
              
});
          
}
          
win.show(this);
      
});
  
});
  
[/code]

Happy Coding!

Download (J2EE project):  http://github.com/loiane/ext-window

Portuguese: ExtJS: Ext.Window: hide ou close?