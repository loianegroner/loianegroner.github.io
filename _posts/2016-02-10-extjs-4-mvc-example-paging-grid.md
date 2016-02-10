---
layout: post
title: 'ExtJS 4 MVC Example: Paging Grid'
published: true
author: Loiane
comments: true
date: 2012-05-22 05:05:50
tags:
    - ExtJS 4
    - ExtJS 4 MVC
categories:
    - ext-js-4
permalink: /2012/05/extjs-4-mvc-example-paging-grid
image:
    feature: extjs4-mvc-paging-grid-loiane.png
---
Another ExtJS 4 MVC Example. Today we are going to port the Paging Grid to MVC.

Let’s get started!


  


## Project’s Structure

[][1]

## Model

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.model.ForumThread&#8217;, {
      
extend: &#8216;Ext.data.Model&#8217;,
      
fields: [
          
&#8216;title&#8217;,
          
&#8216;forumtitle&#8217;,
          
&#8216;forumid&#8217;,
          
&#8216;username&#8217;,
          
{name: &#8216;replycount&#8217;, type: &#8216;int&#8217;},
          
{name: &#8216;lastpost&#8217;, mapping: &#8216;lastpost&#8217;, type: &#8216;date&#8217;, dateFormat: &#8216;timestamp&#8217;},
          
&#8216;lastposter&#8217;,
          
&#8216;excerpt&#8217;,
          
&#8216;threadid&#8217;
      
],
      
idProperty: &#8216;threadid&#8217;
  
});
  
[/code]

## Store

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.store.ForumThreads&#8217;, {
      
extend: &#8216;Ext.data.Store&#8217;,
      
model: &#8216;ExtMVC.model.ForumThread&#8217;,
      
autoLoad: true,
      
remoteSort: true,
      
proxy: {
          
// load using script tags for cross domain, if the data in on the same domain as
          
// this page, an HttpProxy would be better
          
type: &#8216;jsonp&#8217;,
          
url: &#8216;http://www.sencha.com/forum/topics-browse-remote.php&#8217;,
          
reader: {
              
root: &#8216;topics&#8217;,
              
totalProperty: &#8216;totalCount&#8217;
          
},
          
// sends single sort as multi parameter
          
simpleSortMode: true
      
},
      
sorters: [{
          
property: &#8216;lastpost&#8217;,
          
direction: &#8216;DESC&#8217;
      
}]
  
});
  
[/code]

## View &#8211; Grid

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.view.forumThread.ForumThreadGrid&#8217; ,{
      
extend: &#8216;Ext.grid.Panel&#8217;,
      
alias : &#8216;widget.forumthreadgrid&#8217;,

requires: &#8216;Ext.ux.PreviewPlugin&#8217;,

title : &#8216;ExtJS.com &#8211; Browse Forums&#8217;,

disableSelection: true,

loadMask: true,

viewConfig: {
          
id: &#8216;gv&#8217;,
          
trackOver: false,
          
stripeRows: false,
          
plugins: [{
              
ptype: &#8216;preview&#8217;,
              
bodyField: &#8216;excerpt&#8217;,
              
expanded: true,
              
pluginId: &#8216;preview&#8217;
          
}]
      
},

// pluggable renders
      
renderTopic: function(value, p, record) {
          
return Ext.String.format(
              
&#8216;{0}{1} Forum&#8217;,
              
value,
              
record.data.forumtitle,
              
record.getId(),
              
record.data.forumid
          
);
      
},

renderLast: function(value, p, r) {
          
return Ext.String.format(&#8216;{0}
  
by {1}&#8217;, Ext.Date.dateFormat(value, &#8216;M j, Y, g:i a&#8217;), r.get(&#8216;lastposter&#8217;));
      
},

initComponent: function() {

this.store = &#8216;ForumThreads&#8217;;

this.columns = [
          
{
              
id: &#8216;topic&#8217;,
              
text: "Topic",
              
dataIndex: &#8216;title&#8217;,
              
flex: 1,
              
renderer: this.renderTopic,
              
sortable: false
          
},{
              
text: "Author",
              
dataIndex: &#8216;username&#8217;,
              
width: 100,
              
hidden: true,
              
sortable: true
          
},{
              
text: "Replies",
              
dataIndex: &#8216;replycount&#8217;,
              
width: 70,
              
align: &#8216;right&#8217;,
              
sortable: true
          
},{
              
id: &#8216;last&#8217;,
              
text: "Last Post",
              
dataIndex: &#8216;lastpost&#8217;,
              
width: 150,
              
renderer: this.renderLast,
              
sortable: true
          
}];

// paging bar on the bottom
          
this.bbar = Ext.create(&#8216;Ext.PagingToolbar&#8217;, {
              
store: this.store,
              
displayInfo: true,
              
displayMsg: &#8216;Displaying topics {0} &#8211; {1} of {2}&#8217;,
              
emptyMsg: "No topics to display",
              
items:[
                  
&#8216;-&#8216;, {
                  
xtype: &#8216;button&#8217;,
                  
text: &#8216;Show Preview&#8217;,
                  
pressed: true,
                  
action: &#8216;showPreview&#8217;,
                  
enableToggle: true
              
}]
          
});

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
          
&#8216;ExtMVC.view.forumThread.ForumThreadGrid&#8217;
      
],

initComponent: function() {
          
var me = this;

Ext.apply(me, {
              
items: [
                  
{
                      
xtype: &#8216;forumthreadgrid&#8217;
                  
}
              
]
          
});

me.callParent(arguments);
      
}
  
});
  
[/code]

## Controller

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.controller.ForumThreads&#8217;, {
      
extend: &#8216;Ext.app.Controller&#8217;,

stores: [&#8216;ForumThreads&#8217;],

models: [&#8216;ForumThread&#8217;],

views: [&#8216;forumThread.ForumThreadGrid&#8217;],

init: function() {
    	  
this.control({
	          
&#8216;forumthreadgrid button[action=showPreview]&#8217;: {
	        	  
toggle: this.showPreview
	    	  
}
	      
});
      
},

showPreview: function(btn, pressed){

var preview = Ext.ComponentQuery.query(&#8216;forumthreadgrid dataview&#8217;)[0].plugins[0];

preview.toggleExpanded(pressed);
      
}
  
});
  
[/code]

## App.js

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.application({
      
name: &#8216;ExtMVC&#8217;,

paths: { &#8216;Ext.ux&#8217;: &#8216;extjs/ux/&#8217; },

controllers: [
          
&#8216;ForumThreads&#8217;
      
],

autoCreateViewport: true
  
});
  
[/code]

## HTML

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  
Ext JS 4 MVC Examples &#8211; loiane.com




          
.x-grid-cell-topic b {
              
display: block;
          
}
          
.x-grid-cell-topic .x-grid-cell-inner {
              
white-space: normal;
          
}
          
.x-grid-cell-topic a {
              
color: #385F95;
              
text-decoration: none;
          
}
          
.x-grid-cell-topic a:hover {
              
text-decoration:underline;
          
}
		  
.x-grid-cell-topic .x-grid-cell-innerf {
			  
padding: 5px;
		  
}
		  
.x-grid-rowbody {
	          
padding: 0 5px 5px 5px;
		  
}
      







  

  

  
[/code]

## Demo

To see this example running live, please go to: http://loiane.com/extjs/extjs4-mvc-paging-grid/

## Download the Source Code

You can download the full source code from my Github repository: https://github.com/loiane/extjs4-mvc-paging-grid

Happy Coding! ![:)][2]

 [1]: http://loianegroner.com/wp-content/uploads/2012/05/extjs4-mvc-paging-grid-loiane-01.png
 [2]: http://loianegroner.com/wp-includes/images/smilies/icon_smile.gif