---
layout: post
title: >
    Populating an ExtJS DataGrid + RowExpander using Spring MVC
    3
published: true
author: Loiane
comments: true
date: 2010-05-13 10:05:42
tags:
    - ExtJS
    - ExtJS + J2EE
    - ExtJS Grid
    - ExtJS Plugin
    - JSON
    - RowExpander
    - Spring 3
    - Spring MVC
categories:
    - extjs
permalink: >
    /2010/05/populating-an-extjs-datagrid-rowexpander-using-spring-mvc-3
---

  This tutorial will walk through how to implement an ExtJS DataGrid with RowExpander plugin using Spring MVC Framework version 3 in the server side.


[][1]


  Sometimes you need to show more information than fits within the grid, as an expansion of the record/row. ExtJS DataGrid component provides a plugin called RowExpander, which does exactly what its name says and I just described before.



  It is very simple to use.



  Let’s say you want to show information about some books. The following class represents your data (it is a POJO):


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public class Book {

private int id;
	  
private String title;
	  
private String publisher;
	  
private String ISBN10;
	  
private String ISBN13;
	  
private String link;
	  
private String description;
  
}
  
[/code]


  But you do not want to show all the attributes within the grid. You just want to show the book title and the publisher, and if the user wants to know more about that book, he/she has to expand the row. First, let’s declare a JSON store to get data from server. You can configure the store normally, as all the data is going to be a column in the datagrid.


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
	  
var store = new Ext.data.Store({
		  
proxy: new Ext.data.HttpProxy({
			  
url: &#8216;getBooks.action&#8217;
		  
}),
		  
reader: new Ext.data.JsonReader({
			  
root:&#8217;books&#8217;
		  
},
		  
[{name: &#8216;id&#8217;},
		   
{name: &#8216;title&#8217;},
		   
{name: &#8216;publisher&#8217;},
		   
{name: &#8216;isbn10&#8217;},
		   
{name: &#8216;isbn13&#8217;},
		   
{name: &#8216;link&#8217;},
		   
{name: &#8216;description&#8217;},
		  
])
	  
});
  
[/code]

And a method in the server to retrieve the data:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
@Controller
  
public class BookController {

private BookService bookService;

/**
	   
* Get all the books from "database" to display on
	   
* ExtJS data grid
	   
* @return JSON object
	   
*/
	  
@RequestMapping(value="getBooks.action", method = RequestMethod.GET)
	  
public @ResponseBody Map view() {

List books = bookService.getBooks();

Map modelMap = new HashMap();
		  
modelMap.put("books", books);

return modelMap;
	  
}

@Autowired
	  
public void setBookService(BookService bookService) {
		  
this.bookService = bookService;
	  
}
  
}
  
[/code]


  
    To learn more about Spring 3 annotations and how to retrieve data from server in JSON data format, please read: http://blog.springsource.com/2010/01/25/ajax-simplifications-in-spring-3-0/
  


Let’s declare the datagrid:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;9,16&#8243;]
      
var gridBooks = new Ext.grid.GridPanel({
          
store: store,
          
cm: new Ext.grid.ColumnModel({
              
defaults: {
                  
sortable: true,
                  
width: 200
              
},
              
columns: [
                  
expander,
                  
{header: "Title", dataIndex: &#8216;title&#8217;},
                  
{header: "Publisher", dataIndex: &#8216;publisher&#8217;}
              
]
          
}),
          
width: 430,
          
height: 270,
          
plugins: expander,
          
title: &#8216;ExtJS Books&#8217;,
          
renderTo: &#8216;gridBooks&#8217;
      
});
  
[/code]


  You need to declare the rowExpander plugin as a column (first one – line  9), just like you need to do when you use ColumnModel (checkbox), and you also need to declare it as a plugin (line 16).


And now, let’s take a look how to declare the plugin:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
var expander = new Ext.ux.grid.RowExpander({
          
tpl : new Ext.Template(
              
&#8216;ISBN10: {isbn10}&#8217;,
              
&#8216;ISBN13: {isbn13}&#8217;,
              
&#8216;Link: {link}&#8217;,
              
&#8216;Description: {description}&#8217;
          
)
      
});
  
[/code]


  Let you imagination flow and create the expansion you want using HTML or anything Ext.Template enables you to do!


Don’t forget to import RowExpander javascript file:

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;13&#8243;]
  

  

  

  
Row Expander + Spring


	  



	  

	  





	  

  

  

	  

	  

  

  

  
[/code]


  You can download the complete project from my GitHub repository: http://github.com/loiane/extjs-rowexpander


Happy Coding!

 [1]: http://loianegroner.com/wp-content/uploads/2010/05/ExtJS_RowExpander_Spring3_Loiane_01.gif