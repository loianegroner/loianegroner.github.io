---
layout: post
title: 'ExtJS 4 Example: Multiline Row in a Grid'
published: true
author: Loiane
comments: true
date: 2012-05-25 05:05:53
tags:
    - ExtJS 4
    - Grid
    - Multiline Row
categories:
    - ext-js-4
permalink: /2012/05/extjs-4-example-multiline-row-in-a-grid
image:
    feature: extjs4-grid-multiline-row-loiane-01.jpg
---
Today&#8217;s post is a quick tip of how we can modify the ExtJS 4 grid to support multiline rows if the content does not fit in a single row.

For example, take a look at the following grid:


  


In the description column, we want to display the following content:

> _Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Sed metus nibh, sodales a, porta at, vulputate eget, dui. Pellentesque ut nisl. Maecenas tortor turpis, interdum non, sodales non, iaculis ac, lacus. Vestibulum auctor, tortor quis iaculis malesuada, libero lectus bibendum purus, sit amet tincidunt quam turpis vel lacus. In pellentesque nisl non sem. Suspendisse nunc sem, pretium eget, cursus a, fringilla vel, urna._
  
>  _Aliquam commodo ullamcorper erat. Nullam vel justo in neque porttitor laoreet. Aenean lacus dui, consequat eu, adipiscing eget, nonummy non, nisi. Morbi nunc est, dignissim non, ornare sed, luctus eu, massa. Vivamus eget quam. Vivamus tincidunt diam nec urna. Curabitur velit._

But the above content does not fit in a single line. What can we do to make it fit and display as many rows as needed?

We just need to add the following CSS in our project&#8217;s html page:

[code lang=&#8221;css&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
.x-grid-row .x-grid-cell-inner {
	  
white-space: normal;
  
}
  
.x-grid-row-over .x-grid-cell-inner {
	  
font-weight: bold;
	  
white-space: normal;
  
}
  
[/code]

And the grid will look like this:


  


Let&#8217;s see the complete code:

### ExtJS 4 Code:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.require([
      
&#8216;Ext.grid.*&#8217;,
      
&#8216;Ext.data.*&#8217;
  
]);

Ext.define(&#8216;Post&#8217;, {
      
extend: &#8216;Ext.data.Model&#8217;,
      
fields: [
         
{name: &#8216;post&#8217;},
         
{name: &#8216;desc&#8217;}
      
]
  
});

Ext.onReady(function() {

// sample static data for the store
      
var myData = [
          
[&#8216;ExtJS 4 Grid Multiline Row&#8217;,
          
&#8216;Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Sed metus nibh,&#8217;+
          
&#8216;sodales a, porta at, vulputate eget, dui. Pellentesque ut nisl. Maecenas tortor turpis,&#8217;+
          
&#8216;interdum non, sodales non, iaculis ac, lacus. Vestibulum auctor, tortor quis iaculis malesuada,&#8217;+
          
&#8216;libero lectus bibendum purus, sit amet tincidunt quam turpis vel lacus. In pellentesque nisl non&#8217;+
          
&#8216;sem. Suspendisse nunc sem, pretium eget, cursus a, fringilla vel, urna.&#8217;+
		  
&#8216;Aliquam commodo ullamcorper erat. Nullam vel justo in neque porttitor laoreet. Aenean lacus dui,&#8217;+
		  
&#8216;consequat eu, adipiscing eget, nonummy non, nisi. Morbi nunc est, dignissim non, ornare sed,&#8217;+
		  
&#8216;luctus eu,&#8217;+
		  
&#8216;massa. Vivamus eget quam. Vivamus tincidunt diam nec urna. Curabitur velit.&#8217;]
      
];

// create the data store
      
var store = Ext.create(&#8216;Ext.data.ArrayStore&#8217;, {
          
model: &#8216;Post&#8217;,
          
data: myData
      
});

// create the Grid
      
var grid = Ext.create(&#8216;Ext.grid.Panel&#8217;, {
          
store: store,
          
stateful: true,
          
collapsible: true,
          
multiSelect: true,
          
columns: [
              
{
                  
text : &#8216;Post&#8217;,
          		  
width : 150,
                  
sortable : false,
                  
dataIndex: &#8216;post&#8217;
              
},
              
{
                  
text : &#8216;Description&#8217;,
                  
flex : 1,
                  
sortable : false,
                  
dataIndex: &#8216;desc&#8217;
              
}
          
],
          
height: 200,
          
width: 600,
          
title: &#8216;Array Grid&#8217;,
          
renderTo: &#8216;grid-example&#8217;,
          
viewConfig: {
              
stripeRows: true,
              
enableTextSelection: true
          
}
      
});
  
});

[/code]

### HTML:

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  
Ext JS 4 Examples &#8211; loiane.com


	  

      



      



	  
.x-grid-row .x-grid-cell-inner {
		  
white-space: normal;
	  
}
	  
.x-grid-row-over .x-grid-cell-inner {
		  
font-weight: bold;
		  
white-space: normal;
	  
}
      



  

	  

  

  

  
[/code]

### Download the complete source code:

You can download or fork the source code from Github repository: https://github.com/loiane/extjs4-grid-multiline-row

### Demo:

To see a live demo of the sample project, please go to: http://loiane.com/extjs/extjs4-grid-multiline-row

Happy coding! 