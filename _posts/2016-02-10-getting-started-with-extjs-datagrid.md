---
layout: post
title: Getting Started with ExtJS DataGrid
published: true
author: Loiane
comments: true
date: 2009-12-22 12:12:55
tags:
    - ExtJS
    - ExtJS + J2EE
    - ExtJS Grid
categories:
    - extjs
permalink: /2009/12/getting-started-with-extjs-datagrid
---
This tutorial will walk through how to implement a simple Ext JS datagrid, using a GridPanel to display structured data in a user-friendly grid.

Screeshot for this tutorial:



The grid is, without doubt, one of the most widely-used components of Ext. We all have data, and this needs to be presented to the end user in an easy-to-understand manner. The spreadsheet (a.k.a.: grid) is the perfect way to do this—the concept has been around for quite a while because it works. Ext takes that concept and makes it flexible and downright amazing!

### Terminology:

Displaying data in a grid requires two Ext components:

  * A store that acts like an in-memory database, keeping track of the data we want to display
  * A grid panel that provides a way to display the data stored in a data store

Before we start to create each of these, let&#8217;s look at some of the terminology that will be used, because this can be confusing at first:

  * Columns: This refers to a whole column of data, and would contain information only relevant to the display of data down through the entire column, including the heading. In Ext JS, this information is part of the Column Model.
  * Fields: This also refers to an entire column of data, but is refers to the actual data values. With Ext JS, this is used in the reader, for loading data.

Before we get started, you must configure your project to use Ext JS library. If you do not know how to do it, you can check it out this tutorial.

### Setting up a data store:

The first thing we need to do is set up our data, which will be placed into a data store. The data store types available in Ext give us a consistent way of reading different data formats such as XML and JSON, and reading this data in a consistent way throughout all of the Ext widgets. Regardless of whether this data it is JSON, XML, an array, or even a custom data type of your own, it&#8217;s all accessed in the same way thanks to the data store.

Some data stores available, by default, in Ext are:

  * Simple (Array)
  * XML
  * JSON

### Step 1: Adding data to our data store:

In this first example, we are going to use a simple (array) data store, represeting a simple agenda (name, phone, email and birthday date).

The data store needs two things: the data itself, and a description of the data—or what could be thought of as the fields. A reader will be used to read the data from the array, and this is where we define the fields of data contained in our array. The reader acts as an interpreter of sorts; it knows how to interpret a string of data as rows of data to be used with Ext JS.

### Step 2: Defining your data for the data store

The reader needs to know which fields to read in as data for our data store, so we will need to define these.

Fields are defined using an array of objects—or if the data is to be read verbatim, just a string specifying the field name. All except one of our fields in this example can be defined with a simple name. For example, the title field could be defined using an object like this:

{name: &#8216;title&#8217;}

However, in our case, because we are just reading in the data as a string, we can simply pass the field name and save some typing:

&#8216;title&#8217;

The released field is different because we want to treat its data appropriately, as a date type. For each field format type, there may be options to define the format of the data more explicitly. With the date type, there is a dateFormat string that needs to be defined. If you have used PHP, these date format strings will look familiar, because Ext uses the same date format strings that PHP does.

{name: &#8216;released&#8217;, type: &#8216;date&#8217;, dateFormat: &#8216;Y-m-d&#8217;}

Here are some datatypes supported by ExtJS: string, int, float, boolean, date.

### Step 3: Displaying the GrigPanel

The thing that pulls everything together is the GridPanel, which takes care of placing the data into columns and rows, along with adding column headers, and boxing everything together in a neat little package.

### Code:

[code lang=&#8221;js&#8221; ffirstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.onReady(function(){

Ext.BLANK\_IMAGE\_URL = &#8216;/extjs-grid-simple-array/ext-3.0.3/resources/images/default/s.gif&#8217;;

//array with data
      
var myData = [
          
[&#8216;Meyers, Quyn R.&#8217;, &#8216;(943) 570-5141&#8217;, &#8216;Proin@nullamagna.ca&#8217;, &#8217;05/13/1990&#8242;],
		  
[&#8216;Whitney, Tad T.&#8217;, &#8216;(547) 743-0343&#8217;, &#8216;vulputate@acurnaUt.org&#8217;, &#8217;05/10/1987&#8242;],
		  
[&#8216;Lawrence, Flavia J.&#8217;, &#8216;(404) 826-4553&#8217;, &#8216;dapibus.id@accumsan.ca&#8217;, &#8217;01/05/1988&#8242;],
		  
[&#8216;Morales, Susan I.&#8217;, &#8216;(276) 707-8084&#8217;, &#8216;tristique@seacmetus.com&#8217;,&#8217;03/09/1992&#8242;],
		  
[&#8216;Merrill, Desiree Q.&#8217;, &#8216;(911) 546-0559&#8217;, &#8216;dictum.cursus@vel.ca&#8217;, &#8217;01/07/1981&#8242;],
		  
[&#8216;Hampton, Willa Y.&#8217;, &#8216;(729) 562-8303&#8217;, &#8216;nascetur@stellus.ca&#8217;, &#8217;06/17/1991&#8242;],
		  
[&#8216;Brewer, Brynne F.&#8217;, &#8216;(818) 302-4393&#8217;, &#8216;ligula@ullamcorper.org&#8217;, &#8217;04/20/1989&#8242;],
		  
[&#8216;Marsh, Drew D.&#8217;, &#8216;(645) 671-2779&#8217;, &#8216;et.euismod.et@eget.ca&#8217;, &#8217;02/13/1990&#8242;]
      
];

//data store &#8211; description of fields
      
var store = new Ext.data.SimpleStore({
          
fields: [
             
&#8216;name&#8217;,
             
&#8216;phone&#8217;,
             
&#8217;email&#8217;,
             
{name: &#8216;birthday&#8217;, type: &#8216;date&#8217;, dateFormat: &#8216;d/m/Y&#8217;}
          
]
      
});

//read the data from simple array
      
store.loadData(myData);

// create grid
      
var grid = new Ext.grid.GridPanel({
          
store: store,
          
columns: [
              
{header: &quot;NAME&quot;, width: 170, sortable: true, dataIndex: &#8216;name&#8217;},
              
{header: &quot;PHONE #&quot;, width: 150, sortable: true, dataIndex: &#8216;phone&#8217;},
              
{header: &quot;EMAIL&quot;, width: 150, sortable: true, dataIndex: &#8217;email&#8217;},
              
{header: &quot;BIRTHDAY&quot;, width: 100, sortable: true, dataIndex: &#8216;birthday&#8217;,
            	  
renderer: Ext.util.Format.dateRenderer(&#8216;d/m/Y&#8217;)}
          
],
          
title: &#8216;My Contacts&#8217;,
          
autoHeight:true,
          
width:590,
		  
renderTo: document.body,
		  
frame:true
      
});

//render to DIV
      
grid.render(&#8216;grid-simple-array&#8217;);
  
});
  
[/code]

You can download the source code in my Github:  http://github.com/loiane/extjs-grid-simple-array

Reference: Learning Ext JS &#8211; Book