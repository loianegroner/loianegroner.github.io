---
layout: post
title: >
    How to resize an ExtJS Panel, Grid, Component on Window
    Resize without using Ext.Viewport
published: true
author: Loiane
comments: true
date: 2010-08-24 08:08:56
tags:
    - Ext.Viewport
    - ExtJS
categories:
    - extjs
permalink: >
    /2010/08/how-to-resize-an-extjs-panel-grid-component-on-window-resize-without-using-ext-viewport
---
This post will walk through how to resize an ExtJS Panel, Grid, Component on Window Resize without using Ext.Viewport.


  



  
    Problem:
  
  
  
    You have a legacy page and you want to change an html grid for an ExtJS DataGrid, because it has so many cool features. Or you have a page with some design and you are going to use only one ExtJS Component. In both cases, you also want to render your ExtJS Component to a specific DIV. Also, you want you component to be resized in case you resize the browser window.
  
  
  
    How can you do that if resize a single component in an HTML page it is not the default behavior of an ExtJS Component (except if you use Ext.Viewport)?
  
  
  
    Solution:
  
  
  
    Condor (from ExtJS Community Support Team) developed a plugin that can do that for you.
  
  
  
    I had to spend some time to understand how the plugin works, and I finally got it working as I wanted.
  
  
  
    Well, I recommend you to spend some time reading this thread: http://www.sencha.com/forum/showthread.php?28318 (if you have any issues or questions, please publish it on the thread, so other members can give you the support you need).
  
  
  
    Requirements to make the plugin work:
  
  
  
    Your have to apply the following style to the DIV (the width is up to you, the other styles are mandatory, otherwise it will not work):
  
  
  
    [code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]  [/code]
  
  
  
    If you have any border around your ExtJS component, you have to set a HEIGHT. And you will also have to set a height to your ExtJS component. In this case, autoHeight will not work. If you DO NOT have any border or other design on the ExtJS component side, you do not need to set height and you can use autoHeight. In my case, I put a border on the external DIV, so I have to set Height:
  
  
  
    [code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]  [/code]
  
  
  
    HTML code (all DIVs):
  
  
  
    [code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]          [/code]
  
  
  
    And you need to add the plugin to the component (In this case, I’m using an ExtJS DataGrid):
  
  
  
    [code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;20&#8243;] var grid = new Ext.grid.GridPanel({ store: store, columns: [ {header: &#8216;Company&#8217;, width: 160, sortable: true, dataIndex: &#8216;company&#8217;}, {header: &#8216;Price&#8217;, width: 75, sortable: true, renderer: &#8216;usMoney&#8217;, dataIndex: &#8216;price&#8217;}, {header: &#8216;Change&#8217;, width: 75, sortable: true, renderer: change, dataIndex: &#8216;change&#8217;}, {header: &#8216;% Change&#8217;, width: 75, sortable: true, renderer: pctChange, dataIndex: &#8216;pctChange&#8217;}, {header: &#8216;Last Updated&#8217;, width: 85, sortable: true, renderer: Ext.util.Format.dateRenderer(&#8216;m/d/Y&#8217;), dataIndex: &#8216;lastChange&#8217;} ], stripeRows: true, autoExpandColumn: &#8216;company&#8217;, height: 490, autoWidth:true, title: &#8216;Array Grid&#8217;, // config options for stateful behavior stateful: true, stateId: &#8216;grid&#8217; ,viewConfig:{forceFit:true} ,renderTo: &#8216;reportTabContent&#8217; // render the grid to the specified div in the page ,plugins: [new Ext.ux.FitToParent("reportTabContent")] }); [/code]
  
  
  
    And done! Now you can resize the browser and the component will resize itself!
  
  
  
    I tested it on Firefox, Chrome and IE6.
  
  
  
    You can download my sample project from my GitHub: http://github.com/loiane/extjs-fit-to-parent
  
  
  
    PS.: If you want to use the full browser window, use a Viewport.
  
  
  
    Happy coding!
  