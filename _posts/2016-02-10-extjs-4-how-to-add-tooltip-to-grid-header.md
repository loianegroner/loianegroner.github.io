---
layout: post
title: 'ExtJS 4: How to add Tooltip to Grid Header'
published: true
author: Loiane
comments: true
date: 2011-10-04 07:10:05
tags:
    - Ext JS 4
    - ExtJS
    - ExtJS 4
    - ExtJS Grid
    - Grid Header Tooltip
categories:
    - ext-js-4
permalink: /2011/10/extjs-4-how-to-add-tooltip-to-grid-header
---

  This tutorial will walk through out how to add a tooltip to a Grid Header. This feature is not natively supported by Ext JS 4 API. Fortunately,  there is a third-party plugin we can use to do it.



  To get started, I created a JavaScript project on Eclipse IDE and it looks like this:



  



  Plugin Code



  The first thing we have to add (after Ext JS 4 SDK) is the plugin. To do so, I created a folder ux (for plugins) and a folder called grid (inside ux) because it is a plugin for Ext 4 grid. Then I created a file name HeaderToolTip.js with the following content:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
/**
   
* @class Ext.ux.grid.HeaderToolTip
   
* @namespace ux.grid
   
*
   
* Text tooltips should be stored in the grid column definition
   
*
   
* Sencha forum url:
   
* http://www.sencha.com/forum/showthread.php?132637-Ext.ux.grid.HeaderToolTip
   
*/
  
Ext.define(&#8216;Ext.ux.grid.HeaderToolTip&#8217;, {
      
alias: &#8216;plugin.headertooltip&#8217;,
      
init : function(grid) {
          
var headerCt = grid.headerCt;
          
grid.headerCt.on("afterrender", function(g) {
              
grid.tip = Ext.create(&#8216;Ext.tip.ToolTip&#8217;, {
                  
target: headerCt.el,
                  
delegate: ".x-column-header",
                  
trackMouse: true,
                  
renderTo: Ext.getBody(),
                  
listeners: {
                      
beforeshow: function(tip) {
                          
var c = headerCt.down(&#8216;gridcolumn[id=&#8217; + tip.triggerElement.id +&#8217;]&#8217;);
                          
if (c && c.tooltip)
                              
tip.update(c.tooltip);
                          
else
                              
return false;
                      
}
                  
}
              
});
          
});
      
}
  
});
  
[/code]


  Grid with Header Tooltip



  Now we need to build the application code. To test, simply create a data grid:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;1,3,4,5,6,7,35,42&#8243;]
  
Ext.Loader.setConfig({enabled: true});

Ext.require([
      
&#8216;Ext.grid.*&#8217;,
      
&#8216;Ext.data.*&#8217;,
      
&#8216;Ext.ux.grid.HeaderToolTip&#8217;
  
]);

Ext.onReady(function() {

var myData = [
          
[&#8216;3m Co&#8217;],
          
[&#8216;Alcoa Inc&#8217;],
          
[&#8216;Altria Group Inc&#8217;],
          
[&#8216;American Express Company&#8217;],
          
[&#8216;American International Group, Inc.&#8217;],
          
[&#8216;AT&T Inc.&#8217;],
          
[&#8216;Boeing Co.&#8217;],
          
[&#8216;Caterpillar Inc.&#8217;],
          
[&#8216;Citigroup, Inc.&#8217;],
          
[&#8216;E.I. du Pont de Nemours and Company&#8217;],
          
[&#8216;Exxon Mobil Corp&#8217;],
          
[&#8216;General Electric Company&#8217;]
      
];

var store = Ext.create(&#8216;Ext.data.ArrayStore&#8217;, {
          
fields: [
             
{name: &#8216;company&#8217;}
          
],
          
data: myData
      
});

Ext.create(&#8216;Ext.grid.Panel&#8217;, {
          
store: store,
          
plugins: [&#8216;headertooltip&#8217;],
          
columns: [
              
{
                  
text : &#8216;Company&#8217;,
                  
flex : 1,
                  
sortable : false,
                  
dataIndex: &#8216;company&#8217;,
                  
tooltip: &#8216;Some tooltip&#8217;
              
}
          
],
          
height: 200,
          
width: 200,
          
title: &#8216;Grid with Header Tooltip&#8217;,
          
renderTo: &#8216;grid-example&#8217;,
          
viewConfig: {
              
stripeRows: true
          
}
      
});
  
});
  
[/code]


  On line 1, we have to enable the Ext.Loader so Ext can dynamic loading the files we need.



  On lines 3-7 we declared the components we need to have loaded before loading our application. Note the Ext.ux.grid.HeaderTooltip.js is included as well. This way, Ext JS knows it has to look for a file called HeaderTooltip.js inside the folder ux/grid.



  Then on line 35 we have to include the HeaderTooltip plugin as a plugin of the grid we want to display a header tooltip.



  And finally, on line 42 we need to declared a column config called tooltip with the header tooltip we want to display.



  HTML Page



  Then we can create an HTML file we can run on a browser:


[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

      
Grid with Header Tooltip


      



  

  

      

  

  

  
[/code]


  And when we execute the application, we will get the following:



  



  And it is done!


> _**Disclaimer**: I am not the author of this HeaderTooltip plugin. So if you get any errors, please contact the author of the plugin on Sencha Forum: http://www.sencha.com/forum/showthread.php?132637-Ext.ux.grid.HeaderToolTip. I simply demonstrated how to use the plugin on this tutorial._


  I&#8217;m using Ext JS 4.0.2a (open source version) on this project.



  Download the Source Code:



  You can download the source code from:



  My github: https://github.com/loiane/extjs4-grid-header-tooltip



  Happy Coding! 
