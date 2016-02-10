---
layout: post
title: "How to Display an Image/Link Inside an Ext JS GridPanel's Cell"
published: true
author: Loiane
comments: true
date: 2010-01-11 08:01:11
tags:
    - ExtJS
    - ExtJS + J2EE
    - ExtJS Grid
categories:
    - extjs
permalink: >
    /2010/01/how-to-display-an-imagelink-inside-an-ext-js-gridpanels-cell
---
This tutorial will walk through how you can display an image/link inside an Ext **GridPanel** cell using a renderer function.

[][1]

To explain this approach, I will use a sample **GridPanel** that displays some dummy contact information (name, phone, birthday and email). Together with email data, we will display a link (mailto) and an email link icon/image.

### How to do it

First, we need some data to work with.  An **ArrayStore** with a few dummy records:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
//array with data &#8211; dummy data
  
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
  
[/code]

GirdPanel definition:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
// create grid
  
var grid = new Ext.grid.GridPanel({
      
store: store,
      
columns: [
          
{header: &#8216;NAME&#8217;, width: 170, sortable: true, dataIndex: &#8216;name&#8217;},
          
{header: &#8216;PHONE #&#8217;, width: 150, sortable: true, dataIndex: &#8216;phone&#8217;},
          
{header: &#8216;BIRTHDAY&#8217;, width: 100, sortable: true, dataIndex: &#8216;birthday&#8217;,
        	  
renderer: Ext.util.Format.dateRenderer(&#8216;d/m/Y&#8217;)},
          
{header: &#8216;EMAIL&#8217;, width: 160, sortable: true, dataIndex: &#8217;email&#8217;,
        	  
renderer: renderIcon }
      
],
      
title: &#8216;My Contacts&#8217;,
      
autoHeight:true,
      
width:600,
	  
renderTo: document.body,
	  
frame:true
  
});
  
[/code]

### How it works

[code lang=&#8221;js&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
{header: &#8216;EMAIL&#8217;, width: 160, sortable: true, dataIndex: &#8217;email&#8217;, renderer: renderIcon }
  
[/code]

The insteresting part in the above code is the trend column&#8217;s **mail**.  A renderer function is an interceptor method that can change the grid cell before it is rendered.

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
//image path
  
var IMG\_EMAIL = &#8216;/gridcell-with-image/img/email\_link.png&#8217;;

//renderer function
  
function renderIcon(val) {
      
return &#8216;&#8217;+ &#8216; &#8216; + val +'&#8217;;
  
}
  
[/code]

Inside the renderer function we just put some HTML code to display what we want. Pretty easy!

Now you can create your own renderer functions!

Happy coding!

Download (J2EE project):  http://github.com/loiane/gridcell-with-image

Portuguese: http://www.loiane.com/2010/01/extjs-como-colocar-icone-e-link-nas-celulas-do-grid/

 [1]: http://loianegroner.com/wp-content/uploads/2009/12/extjs-grid-cell-with-image-and-link.png