---
layout: post
title: 'ExtJS 4 MVC Example: XML Grid'
published: true
author: Loiane
comments: true
date: 2012-05-21 05:05:11
tags:
    - ExtJS 4
    - ExtJS 4 MVC
categories:
    - ext-js-4
permalink: /2012/05/extjs-4-mvc-example-xml-grid
image:
    feature: extjs4-mvc-xml-grid-loiane.jpg
---
Starting the first ExtJS 4 MVC Example. Today we are going to port the XML Grid to MVC.

Let’s get started!

[][1]

## Project’s Structure

[][2]

## Model

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.model.Book&#8217;,{
      
extend: &#8216;Ext.data.Model&#8217;,
      
fields: [
          
// set up the fields mapping into the xml doc
          
// The first needs mapping, the others are very basic
          
{name: &#8216;Author&#8217;, mapping: &#8216;ItemAttributes > Author&#8217;},
          
&#8216;Title&#8217;,
          
&#8216;Manufacturer&#8217;,
          
&#8216;ProductGroup&#8217;
      
]
  
});
  
[/code]

## Store

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.store.Books&#8217;, {
      
extend: &#8216;Ext.data.Store&#8217;,
      
model: &#8216;ExtMVC.model.Book&#8217;,
      
autoLoad: true,
      
proxy: {
          
// load using HTTP
          
type: &#8216;ajax&#8217;,
          
url: &#8216;data/sheldon.xml&#8217;,
          
// the return will be XML, so lets set up a reader
          
reader: {
              
type: &#8216;xml&#8217;,
              
// records will have an "Item" tag
              
record: &#8216;Item&#8217;,
              
idProperty: &#8216;ASIN&#8217;,
              
totalRecords: &#8216;@total&#8217;
          
}
      
}
  
});
  
[/code]

## View &#8211; Grid

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.view.stock.BookGrid&#8217; ,{
      
extend: &#8216;Ext.grid.Panel&#8217;,
      
alias : &#8216;widget.bookgrid&#8217;,

title : &#8216;XML Grid&#8217;,

initComponent: function() {

this.store = &#8216;Books&#8217;;

this.columns = [{
              
text: "Author",
              
flex: 1,
              
dataIndex: &#8216;Author&#8217;,
              
sortable: true
          
},
          
{
              
text: "Title",
              
width: 180,
              
dataIndex: &#8216;Title&#8217;,
              
sortable: true
          
},
          
{
              
text: "Manufacturer",
              
width: 115,
              
dataIndex: &#8216;Manufacturer&#8217;,
              
sortable: true
          
},
          
{
              
text: "Product Group",
              
width: 100,
              
dataIndex: &#8216;ProductGroup&#8217;,
              
sortable: true
          
}];

this.callParent(arguments);
      
}
  
});
  
[/code]

## View- Viewport

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
/**
   
* The main application viewport, which displays the whole application
   
* @extends Ext.Viewport
   
*/
  
Ext.define(&#8216;ExtMVC.view.Viewport&#8217;, {
      
extend: &#8216;Ext.Viewport&#8217;,
      
layout: &#8216;fit&#8217;,

requires: [
          
&#8216;ExtMVC.view.book.BookGrid&#8217;
      
],

initComponent: function() {
          
var me = this;

Ext.apply(me, {
              
items: [
                  
{
                      
xtype: &#8216;bookgrid&#8217;
                  
}
              
]
          
});

me.callParent(arguments);
      
}
  
});
  
[/code]

## Controller

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.controller.Books&#8217;, {
      
extend: &#8216;Ext.app.Controller&#8217;,

stores: [&#8216;Books&#8217;],

models: [&#8216;Book&#8217;],

views: [&#8216;book.BookGrid&#8217;],

init: function() {

}
  
});
  
[/code]

## App.js

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.application({
      
name: &#8216;ExtMVC&#8217;,

controllers: [
          
&#8216;Books&#8217;
      
],

autoCreateViewport: true
  
});
  
[/code]

## HTML

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  
Ext JS 4 MVC Examples &#8211; loiane.com


	  

      



      



  

  

  

  
[/code]

## Demo

To see this example running live, please go to: http://loiane.com/extjs/extjs4-mvc-xml-grid

## Download the Source Code

You can download the full source code from my Github repository: https://github.com/loiane/extjs4-mvc-xml-grid

Happy Coding! ![:)][3]

 [1]: http://loianegroner.com/wp-content/uploads/2012/05/extjs4-mvc-xml-grid-loiane.jpg
 [2]: http://loianegroner.com/wp-content/uploads/2012/05/extjs4-mvc-xml-grid.png
 [3]: http://loianegroner.com/wp-includes/images/smilies/icon_smile.gif