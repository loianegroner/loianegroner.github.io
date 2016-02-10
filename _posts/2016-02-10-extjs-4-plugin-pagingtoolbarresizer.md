---
layout: post
title: 'ExtJS 4 Plugin: PagingToolbarResizer'
published: true
author: Loiane
comments: true
date: 2011-09-26 07:09:34
tags:
    - ExtJS 4
    - ExtJS 4 Plugin
    - PagingToolbarResizer
categories:
    - ext-js-4
permalink: /2011/09/extjs-4-plugin-pagingtoolbarresizer
---
For Ext JS 3, I implemented a plugin called PagingToolbarResizer. However, ExtJS 4 introduced some changes on the API and the plugin does not work for this new version, so I ported it to Ext JS 4.

### Download

The complete source code with a complete example of how to use it you can find in my github account: https://github.com/loiane/extjs4-ux-paging-toolbar-resizer

### Demo

Working demo: 

### How to use it

To use it, is is very simple. Simply place the _**ux**_ folder in your project and call the plugin like the following:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;16&#8243;]
  
bbar: Ext.create(&#8216;Ext.PagingToolbar&#8217;, {
      
store: store,
      
displayInfo: true,
      
displayMsg: &#8216;Displaying topics {0} &#8211; {1} of {2}&#8217;,
      
emptyMsg: "No topics to display",
      
items:[
		  
&#8216;-&#8216;, {
		  
text: &#8216;Show Preview&#8217;,
		  
pressed: pluginExpanded,
		  
enableToggle: true,
		  
toggleHandler: function(btn, pressed) {
			  
var preview = Ext.getCmp(&#8216;gv&#8217;).getPlugin(&#8216;preview&#8217;);
			  
preview.toggleExpanded(pressed);
		  
}
      
}],
      
plugins : [new Ext.create(&#8216;Ext.ux.PagingToolbarResizer&#8217;,{options : [ 5, 10, 15, 20, 25 ]})]
  
})
  
[/code]

And then you will have a combobox with some paging values available on the Ext JS Grid Paging Toolbar.



Happy coding!