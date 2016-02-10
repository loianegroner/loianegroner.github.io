---
layout: post
title: 'ExtJS: How to Export DataGrid to Excel'
published: true
author: Loiane
comments: true
date: 2010-02-08 09:02:36
tags:
    - export grid to Excel
    - ExtJS Grid
categories:
    - extjs
permalink: /2010/02/extjs-how-to-export-datagrid-to-excel
---

  



  This tutorial will walk through how to export data from ExtJS DataGrid directly to Excel.



  Sometimes the user wants to export the data from the datagrid to an excel file. There is an ExtJS third party plugin that does it for you.



  There are some &#8220;issues&#8221; you have to be aware of before you start:



  
    It needs a browser that supports data URLs, such as Firefox, Opera and IE8.
  
  
    I tested it with the following ExtJS versions: 2.2.1, 3.0, 3.0.3 and 3.1, but it only worked with ExtJS 3.0. If anyone is using other ExtJS version and the plugin worked, please, let me know.
  
  
    It only works on the data in the Store &#8211; if you are using server-side paging, then perform this processing on the server. For quick and dirty conversion of a small table to Excel, this might be useful.
  
  
    If the data in the Store is volatile (It gets reloaded or edited), the data URL will have to be recalculated.
  



  Let&#8217;s get started:



  First, create a file in your projet and paste this content in it (I called it exporter.js): http://github.com/loiane/extjs-export-excel/blob/master/WebContent/js/exporter.js (this file has too many lines, that&#8217;s why I&#8217;m not going to past its content here).



  Then, in your datagrid, add this code:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
var linkButton = new Ext.LinkButton({
          
id: &#8216;grid-excel-button&#8217;,
          
text: &#8216;Export to Excel&#8217;
  
});

//create the Grid
  
var grid = new Ext.grid.GridPanel({
      
bbar: new Ext.Toolbar({
          
buttons: [linkButton]
      
})
  
});

linkButton.getEl().child(&#8216;a&#8217;, true).href = &#8216;data:application/vnd.ms-excel;base64,&#8217; +
  
Base64.encode(grid.getExcelXml());
  
[/code]


  And if you try to export the data, it will look like this:



  



  Feel free to change the color, fonts. Take a look in the code and try to understand it to make the changes you want.



  Ed Spencer also developed a similar plugin. The source code is cleaner than this one. The output is the same, though.



  You can download my sample app from my GitHub repository (JEE project): http://github.com/loiane/extjs-export-excel



  Happy coding!
