---
layout: post
title: 'ExtJS 4: Grid Printer Plugin'
published: true
author: Loiane
comments: true
date: 2011-09-07 07:09:27
tags:
    - Ext JS
    - Ext JS 4
    - ExtJS
    - ExtJS 4
    - ExtJS Grid
    - ExtJS Plugin
    - grid printer
    - grid printer plugin
    - print button
categories:
    - ext-js-4
permalink: /2011/09/extjs-4-grid-printer-plugin
---

  Ed Spencer created a plugin that is capable of creating a print version of an ExtJS grid. This plugin was originally created for ExtJS 3.x. I ported it to ExtJS 4, in case someone need it.



  The plugin can be downloaded on the following link: https://github.com/loiane/extjs4-ux-gridprinter/archives/master



  The zip file contains the source code of the plugin plus an example of how to use it. To run the example page, simply open it on any browser (you will need an internet connection).



  To use the plugin in your project, copy the ux folder and paste it on your project, as the following sample project I created:



  Plugin Code:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
/**
   
* @class Ext.ux.grid.Printer
   
* @author Ed Spencer (edward@domine.co.uk)
   
* Helper class to easily print the contents of a grid. Will open a new window with a table where the first row
   
* contains the headings from your column model, and with a row for each item in your grid&#8217;s store. When formatted
   
* with appropriate CSS it should look very similar to a default grid. If renderers are specified in your column
   
* model, they will be used in creating the table. Override headerTpl and bodyTpl to change how the markup is generated
   
*
   
* Usage:
   
*
   
* 1 &#8211; Add Ext.Require Before the Grid code
   
* Ext.require([
   
* &#8216;Ext.ux.grid.GridPrinter&#8217;,
   
* ]);
   
*
   
* 2 &#8211; Declare the Grid
   
* var grid = Ext.create(&#8216;Ext.grid.Panel&#8217;, {
   
* columns: //some column model,
   
* store : //some store
   
* });
   
*
   
* 3 &#8211; Print!
   
* Ext.ux.grid.Printer.print(grid);
   
*
   
* Original url: http://edspencer.net/2009/07/printing-grids-with-ext-js.html
   
*
   
* Modified by Loiane Groner (me@loiane.com) &#8211; September 2011 &#8211; Ported to Ext JS 4
   
* http://loianegroner.com (English)
   
* http://loiane.com (Portuguese)
   
*/
  
Ext.define("Ext.ux.grid.Printer", {

requires: &#8216;Ext.XTemplate&#8217;,

statics: {
		  
/**
		   
* Prints the passed grid. Reflects on the grid&#8217;s column model to build a table, and fills it using the store
		   
* @param {Ext.grid.Panel} grid The grid to print
		   
*/
		  
print: function(grid) {
			  
//We generate an XTemplate here by using 2 intermediary XTemplates &#8211; one to create the header,
			  
//the other to create the body (see the escaped {} below)
			  
var columns = grid.columns;

//build a useable array of store data for the XTemplate
			  
var data = [];
			  
grid.store.data.each(function(item) {
				  
var convertedData = [];

//apply renderers from column model
				  
for (var key in item.data) {
					  
var value = item.data[key];

Ext.each(columns, function(column) {
						  
if (column.dataIndex == key) {
							  
convertedData[key] = column.renderer ? column.renderer(value) : value;
						  
}
					  
}, this);
				  
}

data.push(convertedData);
			  
});

//use the headerTpl and bodyTpl markups to create the main XTemplate below
			  
var headings = Ext.create(&#8216;Ext.XTemplate&#8217;, this.headerTpl).apply(columns);
			  
var body = Ext.create(&#8216;Ext.XTemplate&#8217;, this.bodyTpl).apply(columns);

var htmlMarkup = [
				  
&#8216;&#8217;,
				  
&#8216;&#8217;,
				    
&#8216;&#8217;,
				      
&#8216;&#8217;,
				      
&#8216;&#8217;,
				      
&#8216;&#8217; + grid.title + &#8216;&#8217;,
				    
&#8216;&#8217;,
				    
&#8216;&#8217;,
				      
&#8216;&#8217;,
				        
headings,
				        
&#8216;&#8217;,
				          
body,
				        
&#8216;&#8217;,
				      
&#8216;&#8217;,
				    
&#8216;&#8217;,
				  
&#8216;&#8217;
			  
];

var html = Ext.create(&#8216;Ext.XTemplate&#8217;, htmlMarkup).apply(data); 

//open up a new printing window, write to it, print it and close
			  
var win = window.open(&#8221;, &#8216;printgrid&#8217;);

win.document.write(html);

if (this.printAutomatically){
				  
win.print();
				  
win.close();
			  
}
		  
},

/**
		   
* @property stylesheetPath
		   
* @type String
		   
* The path at which the print stylesheet can be found (defaults to &#8216;ux/grid/gridPrinterCss/print.css&#8217;)
		   
*/
		  
stylesheetPath: &#8216;ux/grid/gridPrinterCss/print.css&#8217;,

/**
		   
* @property printAutomatically
		   
* @type Boolean
		   
* True to open the print dialog automatically and close the window after printing. False to simply open the print version
		   
* of the grid (defaults to true)
		   
*/
		  
printAutomatically: true,

/**
		   
* @property headerTpl
		   
* @type {Object/Array} values
		   
* The markup used to create the headings row. By default this just uses  elements, override to provide your own
		   
*/
		  
headerTpl: [
			  
&#8216;&#8217;,
				  
&#8216;&#8217;,
					  
&#8216;{text}&#8217;,
				  
&#8216;&#8217;,
			  
&#8216;&#8217;
		  
],

/**
		   
* @property bodyTpl
		   
* @type {Object/Array} values
		   
* The XTemplate used to create each row. This is used inside the &#8216;print&#8217; function to build another XTemplate, to which the data
		   
* are then applied (see the escaped dataIndex attribute here &#8211; this ends up as "{dataIndex}")
		   
*/
		  
bodyTpl: [
			  
&#8216;&#8217;,
				  
&#8216;&#8217;,
					  
&#8216;\{{dataIndex}\}&#8217;,
				  
&#8216;&#8217;,
			  
&#8216;&#8217;
		  
]
	  
}
  
});
  
[/code]


  Usage:



  To use the plugin you can create a button and in the handler function you can call the static function of the plugin (you need to pass a grid instance as argument to the print function).


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;131,132&#8243;]
  
Ext.Loader.setConfig({enabled: true});

Ext.require([
      
&#8216;Ext.grid.*&#8217;,
      
&#8216;Ext.data.*&#8217;,
      
&#8216;Ext.ux.grid.Printer&#8217;,
  
]);

Ext.onReady(function() {

// sample static data for the store
      
var myData = [
          
[&#8216;3m Co&#8217;, 71.72, 0.02, 0.03, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Alcoa Inc&#8217;, 29.01, 0.42, 1.47, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Altria Group Inc&#8217;, 83.81, 0.28, 0.34, &#8216;9/1 12:00am&#8217;],
          
[&#8216;American Express Company&#8217;, 52.55, 0.01, 0.02, &#8216;9/1 12:00am&#8217;],
          
[&#8216;American International Group, Inc.&#8217;, 64.13, 0.31, 0.49, &#8216;9/1 12:00am&#8217;],
          
[&#8216;AT&T Inc.&#8217;, 31.61, -0.48, -1.54, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Boeing Co.&#8217;, 75.43, 0.53, 0.71, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Caterpillar Inc.&#8217;, 67.27, 0.92, 1.39, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Citigroup, Inc.&#8217;, 49.37, 0.02, 0.04, &#8216;9/1 12:00am&#8217;],
          
[&#8216;E.I. du Pont de Nemours and Company&#8217;, 40.48, 0.51, 1.28, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Exxon Mobil Corp&#8217;, 68.1, -0.43, -0.64, &#8216;9/1 12:00am&#8217;],
          
[&#8216;General Electric Company&#8217;, 34.14, -0.08, -0.23, &#8216;9/1 12:00am&#8217;],
          
[&#8216;General Motors Corporation&#8217;, 30.27, 1.09, 3.74, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Hewlett-Packard Co.&#8217;, 36.53, -0.03, -0.08, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Honeywell Intl Inc&#8217;, 38.77, 0.05, 0.13, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Intel Corporation&#8217;, 19.88, 0.31, 1.58, &#8216;9/1 12:00am&#8217;],
          
[&#8216;International Business Machines&#8217;, 81.41, 0.44, 0.54, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Johnson & Johnson&#8217;, 64.72, 0.06, 0.09, &#8216;9/1 12:00am&#8217;],
          
[&#8216;JP Morgan & Chase & Co&#8217;, 45.73, 0.07, 0.15, &#8216;9/1 12:00am&#8217;],
          
[&#8216;McDonalds Corporation&#8217;, 36.76, 0.86, 2.40, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Merck & Co., Inc.&#8217;, 40.96, 0.41, 1.01, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Microsoft Corporation&#8217;, 25.84, 0.14, 0.54, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Pfizer Inc&#8217;, 27.96, 0.4, 1.45, &#8216;9/1 12:00am&#8217;],
          
[&#8216;The Coca-Cola Company&#8217;, 45.07, 0.26, 0.58, &#8216;9/1 12:00am&#8217;],
          
[&#8216;The Home Depot, Inc.&#8217;, 34.64, 0.35, 1.02, &#8216;9/1 12:00am&#8217;],
          
[&#8216;The Procter & Gamble Company&#8217;, 61.91, 0.01, 0.02, &#8216;9/1 12:00am&#8217;],
          
[&#8216;United Technologies Corporation&#8217;, 63.26, 0.55, 0.88, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Verizon Communications&#8217;, 35.57, 0.39, 1.11, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Wal-Mart Stores, Inc.&#8217;, 45.45, 0.73, 1.63, &#8216;9/1 12:00am&#8217;]
      
];

/**
       
* Custom function used for column renderer
       
* @param {Object} val
       
*/
      
function change(val) {
          
if (val > 0) {
              
return &#8216;&#8217; + val + &#8216;&#8217;;
          
} else if (val < 0) {
              
return &#8216;&#8217; + val + &#8216;&#8217;;
          
}
          
return val;
      
}

/**
       
* Custom function used for column renderer
       
* @param {Object} val
       
*/
      
function pctChange(val) {
          
if (val > 0) {
              
return &#8216;&#8217; + val + &#8216;%&#8217;;
          
} else if (val < 0) {
              
return &#8216;&#8217; + val + &#8216;%&#8217;;
          
}
          
return val;
      
}

// create the data store
      
var store = Ext.create(&#8216;Ext.data.ArrayStore&#8217;, {
          
fields: [
             
{name: &#8216;company&#8217;},
             
{name: &#8216;price&#8217;, type: &#8216;float&#8217;},
             
{name: &#8216;change&#8217;, type: &#8216;float&#8217;},
             
{name: &#8216;pctChange&#8217;, type: &#8216;float&#8217;},
             
{name: &#8216;lastChange&#8217;, type: &#8216;date&#8217;, dateFormat: &#8216;n/j h:ia&#8217;}
          
],
          
data: myData
      
});

// create the Grid
      
var grid = Ext.create(&#8216;Ext.grid.Panel&#8217;, {
          
store: store,
          
stateful: true,
          
stateId: &#8216;stateGrid&#8217;,
          
columns: [
              
{
                  
text : &#8216;Company&#8217;,
                  
flex : 1,
                  
sortable : false,
                  
dataIndex: &#8216;company&#8217;
              
},
              
{
                  
text : &#8216;Price&#8217;,
                  
width : 75,
                  
sortable : true,
                  
renderer : &#8216;usMoney&#8217;,
                  
dataIndex: &#8216;price&#8217;
              
},
              
{
                  
text : &#8216;Change&#8217;,
                  
width : 75,
                  
sortable : true,
                  
renderer : change,
                  
dataIndex: &#8216;change&#8217;
              
},
              
{
                  
text : &#8216;% Change&#8217;,
                  
width : 75,
                  
sortable : true,
                  
renderer : pctChange,
                  
dataIndex: &#8216;pctChange&#8217;
              
},
              
{
                  
text : &#8216;Last Updated&#8217;,
                  
width : 85,
                  
sortable : true,
                  
renderer : Ext.util.Format.dateRenderer(&#8216;m/d/Y&#8217;),
                  
dataIndex: &#8216;lastChange&#8217;
              
}
          
],
          
height: 350,
          
width: 600,
          
title: &#8216;Array Grid with Print Option&#8217;,
          
renderTo: Ext.getBody(),
          
tbar: [{
              
text: &#8216;Print&#8217;,
              
iconCls: &#8216;icon-print&#8217;,
              
handler : function(){
            	  
Ext.ux.grid.Printer.printAutomatically = false;
            	  
Ext.ux.grid.Printer.print(grid);
              
}
          
}]
      
});
  
});
  
[/code]


  There are some options you can customize, such as the stylesheet path and the printAutomatically.



  And when we execute the code above, we will get a grid with a print button like the following:



  



  And when we click on the Print button, a new page will be opened with the following content:



  



  Demo



  Demo: http://loianegroner.com/extjs/examples/extjs4-ux-gridprinter/



  Github repository: https://github.com/loiane/extjs4-ux-gridprinter



  The original plugin (ExtJS 3.x) &#8211; by Ed Spencer:  http://edspencer.net/2009/07/printing-grids-with-ext-js.html



  Happy Coding! 
