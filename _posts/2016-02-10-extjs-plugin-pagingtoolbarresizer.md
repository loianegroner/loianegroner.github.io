---
layout: post
title: 'ExtJS plugin: PagingToolbarResizer'
published: true
author: Loiane
comments: true
date: 2010-08-02 08:08:29
tags:
    - ExtJS
    - ExtJS Plugin
    - PagingToolbar
    - PagingToolbarResizer
categories:
    - extjs
permalink: /2010/08/extjs-plugin-pagingtoolbarresizer
---
Well, this is my first ExtJS plugin. Though it is not an advanced plugin, I&#8217;m very happy and it is a big accomplishment for me!

### The problem:


  ExtJS PagingToolbar Component allows the developer to set a predetermined page size, which is the total number of records that will be displayed on the grid at once.



  Let&#8217;s say we want to display 10,000 (ten thousand) records and we are going to use the paging feature. We are going to display only 50 records at once (per page). If you do the math, it is 200 pages to visualise. No problem so far.



  The user User1 wants to see 100 records per page, and the user User2 wants to see 150 records per page, just like we can choose on Ebay search:



  


### The solution/plugin:


  For this reason, I implemented this plugin PagingToolbarResizer. It adds a combobox (dropdown) on the paging toolbar with some options for page size. It is totally customizable.



  


Let&#8217;s take a look at some configuration options:

  * **_options_**: it can be a store or an array of integers. If you are going to retrieve the options list from database, you should use a store, otherwise you can use a simple array. Defaults to Defaults to [5, 10, 15, 20, 25, 30, 50, 75, 100, 200, 300, 500, 1000].
  * **_displayText_**: The message to display before the combobox (defaults to &#8216;Records per Page&#8217;).
  * **_prependCombo_**: this flag indicates if you want to display the combobox before paging buttons or after them. Defaults to false.

And it is very easy to use:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;20&#8243;]
          
// paging bar on the bottom
          
bbar: new Ext.PagingToolbar({
              
pageSize: 25,
              
store: store,
              
displayInfo: true,
              
displayMsg: &#8216;Displaying topics {0} &#8211; {1} of {2}&#8217;,
              
emptyMsg: "No topics to display",
              
items:[
                  
&#8216;-&#8216;, {
                  
pressed: true,
                  
enableToggle:true,
                  
text: &#8216;Show Preview&#8217;,
                  
cls: &#8216;x-btn-text-icon details&#8217;,
                  
toggleHandler: function(btn, pressed){
                      
var view = grid.getView();
                      
view.showPreview = pressed;
                      
view.refresh();
                  
}
              
}],
              
plugins : [new Ext.ux.plugin.PagingToolbarResizer( {options : [ 5, 10, 15, 20, 25 ], prependCombo: true})]
          
})
  
[/code]

### Links:

  * The &#8220;_official_&#8221; plugin page ****
  * You can check out a working example (it is the same example you are going to find at Sencha website &#8211; ExtJS examples &#8211; PagingToolbar). I just added the plugin.
  * **The thread at ExtJS forum**
  * **GitHub Link**


  You can also download the plugin here:



  http://loianegroner.com/extjs/PagingToolbarResizer.zip



  I hope this will help someone!
