---
layout: post
title: >
    Importing an Excel Spreadsheet into an ExtJS DataGrid using
    DataDrop Grid Plugin
published: true
author: Loiane
comments: true
date: 2010-03-08 08:03:35
tags:
    - Excel
    - ExtJS
    - ExtJS Grid
    - ExtJS Plugin
    - import excel to grid
categories:
    - extjs
permalink: >
    /2010/03/importing-an-excel-spreadsheet-into-an-extjs-datagrid-using-datadrop-grid-plugin
---

  



  This tutorial will walk through how to import an Excel Spreadsheet into a DataGrid using DataDrop Plugin (by Shea Freaderick).



  Last week I was looking for a plugin that would allow me to import data from an excel file.



  I did not find any plugin on ExtJS forum that works like export excel from grid; I mean, a plugin I do not need any server side code – I could use file upload plugin and do the whole logic thing on server side, but that is not what I was looking for. However, I did find an interesting plugin. Actuality, this plugin I found is awesome! The coolest ExtJS grid plugin I have ever seen! It is really amazing!



  So, what is this plugin I am talking a lot about it?



  It is called DataDrop developed by Shea Frederick (a.k.a VinylFox) and you can drag data into an ExtJS Datagrid from a spreadsheet.



  It is very helpful if you need to import some data from an Excel file. In my point of view, I think it is not good plugin if you need to import large amount of data, because it is not very practical to select a large amount of rows and columns from a spreadsheet. It works very well with any file size, though.



  What do you need to use this plugin?



  1 &#8211; Download plugin from author’s repository (Google code)



  2 &#8211; Add javascript import to your html page (along with your other ExtJS imports)


[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
	  

	  

  
[/code]


  3 &#8211; Add “Ext.ux.grid.DataDrop” to your datagrid plugins declaration


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;11&#8243;]
      
// create grid
      
var grid = new Ext.grid.GridPanel({
          
store: store,
          
columns: [
              
{header: "NAME", width: 170, sortable: true, dataIndex: &#8216;name&#8217;},
              
{header: "PHONE #", width: 150, sortable: true, dataIndex: &#8216;phone&#8217;},
              
{header: "EMAIL", width: 150, sortable: true, dataIndex: &#8217;email&#8217;},
              
{header: "BIRTHDAY", width: 100, sortable: true, dataIndex: &#8216;birthday&#8217;,
            	  
renderer: Ext.util.Format.dateRenderer(&#8216;m/d/Y&#8217;)}
          
],
          
plugins: [Ext.ux.grid.DataDrop],
          
title: &#8216;My Contacts&#8217;,
          
autoHeight:true,
          
width:590,
		  
renderTo: document.body,
		  
frame:true
      
});
  
[/code]


  You can download a working example from my Github repository: http://github.com/loiane/extjs-grid-dragdrop-excel



  Other observations:



  On VinylFox website, there is a video that demonstrates how to use the plugin with Open Office.



  In my first attempt to run my example, I tried to select data from Microsoft Excel and drag and drop into datagrid, but I could not find how to do it (yes, I know, I’m totally M$ Excel newbie – just know how to do simple math – shame on me) – it is so easy to drag data from Open Office and Lotus Symphony – you can click on any place!



  If you are newbie just like me, I recorded a video demonstrating how to do it:





  Happy coding!
