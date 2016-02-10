---
layout: post
title: 'ExtJS 4 MVC Example: Basic Array Grid'
published: true
author: Loiane
comments: true
date: 2012-03-29 12:03:48
tags:
    - ExtJS 4 MVC
    - ExtJS 4 MVC Examples
    - ExtJS MVC
    - MVC
    - Sencha
categories:
    - ext-js-4
permalink: /2012/03/extjs-4-mvc-example-basic-array-grid
image:
    feature: extjs4-basic-array-grid-mvc.jpg
---
Starting the first ExtJS 4 MVC Example. Today we are going to port the Basic Array Grid to MVC.

Let&#8217;s get started!


  


## Project&#8217;s Structure

[][1]

## Model

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;Ext4Example.model.Stock&#8217;, {
	  
extend: &#8216;Ext.data.Model&#8217;,
	  
fields: [
	      
{name: &#8216;company&#8217;},
          
{name: &#8216;price&#8217;, type: &#8216;float&#8217;},
          
{name: &#8216;change&#8217;, type: &#8216;float&#8217;},
          
{name: &#8216;pctChange&#8217;, type: &#8216;float&#8217;},
          
{name: &#8216;lastChange&#8217;, type: &#8216;date&#8217;, dateFormat: &#8216;m-d-Y&#8217;}
      
]
  
});
  
[/code]

## Store

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;Ext4Example.store.Stocks&#8217;, {
      
extend: &#8216;Ext.data.ArrayStore&#8217;,
      
model: &#8216;Ext4Example.model.Stock&#8217;,
      
data: [
          
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
          
[&#8216;McDonald\&#8217;s Corporation&#8217;, 36.76, 0.86, 2.40, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Merck & Co., Inc.&#8217;, 40.96, 0.41, 1.01, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Microsoft Corporation&#8217;, 25.84, 0.14, 0.54, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Pfizer Inc&#8217;, 27.96, 0.4, 1.45, &#8216;9/1 12:00am&#8217;],
          
[&#8216;The Coca-Cola Company&#8217;, 45.07, 0.26, 0.58, &#8216;9/1 12:00am&#8217;],
          
[&#8216;The Home Depot, Inc.&#8217;, 34.64, 0.35, 1.02, &#8216;9/1 12:00am&#8217;],
          
[&#8216;The Procter & Gamble Company&#8217;, 61.91, 0.01, 0.02, &#8216;9/1 12:00am&#8217;],
          
[&#8216;United Technologies Corporation&#8217;, 63.26, 0.55, 0.88, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Verizon Communications&#8217;, 35.57, 0.39, 1.11, &#8216;9/1 12:00am&#8217;],
          
[&#8216;Wal-Mart Stores, Inc.&#8217;, 45.45, 0.73, 1.63, &#8216;9/1 12:00am&#8217;]
      
]
  
});
  
[/code]

## View &#8211; Grid

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;Ext4Example.view.stock.StockGrid&#8217; ,{
      
extend: &#8216;Ext.grid.Panel&#8217;,
      
alias : &#8216;widget.stockgrid&#8217;,

title : &#8216;Json Grid&#8217;,

/**
       
* Custom function used for column renderer
       
* @param {Object} val
       
*/
      
change: function(val) {
          
if (val > 0) {
              
return &#8216;&#8217; + val + &#8216;&#8217;;
          
} else if (val < 0) {
              
return &#8216;&#8217; + val + &#8216;&#8217;;
          
}
          
return val;
      
},

/**
       
* Custom function used for column renderer
       
* @param {Object} val
       
*/
      
pctChange: function(val) {
          
if (val > 0) {
              
return &#8216;&#8217; + val + &#8216;%&#8217;;
          
} else if (val < 0) {
              
return &#8216;&#8217; + val + &#8216;%&#8217;;
          
}
          
return val;
      
},

viewConfig: {
          
stripeRows: true
      
},

initComponent: function() {

this.store = &#8216;Stocks&#8217;;

this.columns = [{
              
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
              
renderer : this.change,
              
dataIndex: &#8216;change&#8217;
          
},
          
{
              
text : &#8216;% Change&#8217;,
              
width : 75,
              
sortable : true,
              
renderer : this.pctChange,
              
dataIndex: &#8216;pctChange&#8217;
          
},
          
{
              
text : &#8216;Last Updated&#8217;,
              
width : 85,
              
sortable : true,
              
renderer : Ext.util.Format.dateRenderer(&#8216;m/d/Y&#8217;),
              
dataIndex: &#8216;lastChange&#8217;
          
},
          
{
              
xtype: &#8216;actioncolumn&#8217;,
              
width: 50,
              
items: [{
                  
iconCls: &#8216;icon-delete&#8217;,
                  
tooltip: &#8216;Sell stock&#8217;,
              
}, {
                  
getClass: function(v, meta, rec) {
                      
if (rec.get(&#8216;change&#8217;) < 0) {
                          
this.items[1].tooltip = &#8216;Hold stock&#8217;;
                          
return &#8216;icon-alert&#8217;;
                      
} else {
                          
this.items[1].tooltip = &#8216;Buy stock&#8217;;
                          
return &#8216;icon-buy&#8217;;
                      
}
                  
}
              
}]
          
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
  
Ext.define(&#8216;Ext4Example.view.Viewport&#8217;, {
      
extend: &#8216;Ext.Viewport&#8217;,
      
layout: &#8216;fit&#8217;,

requires: [
          
&#8216;Ext4Example.view.stock.StockGrid&#8217;
      
],

initComponent: function() {
          
var me = this;

Ext.apply(me, {
              
items: [
                  
{
                      
xtype: &#8216;stockgrid&#8217;
                  
}
              
]
          
});

me.callParent(arguments);
      
}
  
});
  
[/code]

## Controller

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;Ext4Example.controller.Stocks&#8217;, {
      
extend: &#8216;Ext.app.Controller&#8217;,

stores: [&#8216;Stocks&#8217;],

models: [&#8216;Stock&#8217;],

views: [&#8216;stock.StockGrid&#8217;],

init: function() {
          
this.control({
              
&#8216;stockgrid actioncolumn&#8217;: {
                  
click: this.onAction
              
}
          
});
      
},

onAction: function(view,cell,row,col,e){

var m = e.getTarget().className.match(/\bicon-(\w+)\b/);

var rec = this.getStocksStore().getAt(row);

if(m){
              
switch(m[1]){
                  
case &#8216;buy&#8217;:
                      
alert("Buy " + rec.get(&#8216;company&#8217;));
                      
break;
                  
case &#8216;alert&#8217;:
                      
alert("Hold " + rec.get(&#8216;company&#8217;));
                      
break;
                  
case &#8216;delete&#8217;:
                      
alert("Sell " + rec.get(&#8216;company&#8217;));
                      
break;
              
}
          
}
      
}
  
});
  
[/code]

## App.js

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.application({
      
name: &#8216;Ext4Example&#8217;,

controllers: [
          
&#8216;Stocks&#8217;
      
],

autoCreateViewport: true
  
});
  
[/code]

## HTML

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  
Ext JS 4 Examples &#8211; loiane.com


	  

      



      

      



  

  

  
[/code]

## CSS

[code lang=&#8221;css&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
/\* style rows on mouseover \*/
  
.x-grid-row-over .x-grid-cell-inner {
	  
font-weight: bold;
  
}
  
/\* shared styles for the ActionColumn icons \*/
  
.x-action-col-cell img {
	  
height: 16px;
	  
width: 16px;
	  
cursor: pointer;
  
}
  
/\* custom icon for the "buy" ActionColumn icon \*/
  
.x-action-col-cell img.icon-buy {
	  
background-image: url(../icons/accept.gif);
  
}
  
/\* custom icon for the "alert" ActionColumn icon \*/
  
.x-action-col-cell img.icon-alert {
	  
background-image: url(../icons/error.gif);
  
}
  
/\* custom icon for the "alert" ActionColumn icon \*/
  
.x-action-col-cell img.icon-delete {
	  
background-image: url(../icons/delete.gif);
  
}
  
[/code]

## Download the Source Code

You can download the full source code from my Github repository: https://github.com/loiane/extjs4-mvc-basic-array-grid

Happy Coding! 

 [1]: http://loianegroner.com/wp-content/uploads/2012/03/extjs4-mvc-basic-array-grid.png