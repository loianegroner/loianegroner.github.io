---
layout: post
title: 'ExtJS and Spring MVC Framework: CRUD DataGrid Example'
published: true
author: Loiane
comments: true
date: 2010-03-22 10:03:55
tags:
    - CRUD Grid
    - DataDrop
    - Ext.data.JsonWriter
    - ExtJS
    - ExtJS + J2EE
    - ExtJS Grid
    - json-lib-ext-spring
    - RowEditor
    - Spring
    - Spring MVC
categories:
    - extjs
permalink: >
    /2010/03/extjs-and-spring-mvc-framework-crud-datagrid-example
---

  



  This tutorial will walk through how to implement a CRUD (Create, Read, Update, Delete) DataGrid using ExtJS and Spring Framework.



  What do we usually want to do with data?



  
    Create (Insert)
  
  
    Read / Retrieve (Select)
  
  
    Update (Update)
  
  
    Delete / Destroy (Delete)
  



  Until ExtJS 3.0 we only could READ data using a dataGrid. If you wanted to update, insert or delete, you had to do some code to make these actions work. Now ExtJS 3.0 (and newest versions) introduces the ext.data.writer, and you do not need all that work to have a CRUD Grid.



  So… What do I need to add in my code to make all these things working together?



  In this example, I’m going to use JSON as data format exchange between the browser and the server.



  First, you need an Ext.data.JsonWriter:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
   
// The new DataWriter component.
      
var writer = new Ext.data.JsonWriter({
          
encode: true,
          
writeAllFields: false
      
});
  
[/code]

Where writeAllFields identifies that we want to write all the fields from the record to the database. If you have a fancy ORM then maybe you can set this to false. For example. This is my record type declaration:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
var Contact = Ext.data.Record.create([
	  
{name: &#8216;id&#8217;},
      
{
          
name: &#8216;name&#8217;,
          
type: &#8216;string&#8217;
      
}, {
          
name: &#8216;phone&#8217;,
          
type: &#8216;string&#8217;
      
}, {
          
name: &#8217;email&#8217;,
          
type: &#8216;string&#8217;
      
}, {
          
name: &#8216;birthday&#8217;,
          
type: &#8216;date&#8217;,
          
dateFormat: &#8216;m/d/Y&#8217;
      
}]);
  
[/code]


  If I only update the name of any contact in the datagrid, the app will only send the contact id and contact name back to server. In another words, the app will only send the updated data to the server + id (in update cases) and only the contact id in delete cases. Try to change it to true and enable Firebug plugin in your browser to see what happens. Now you need to setup a proxy like this one:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
var proxy = new Ext.data.HttpProxy({
          
api: {
              
read : &#8216;contact/view.action&#8217;,
              
create : &#8216;contact/create.action&#8217;,
              
update: &#8216;contact/update.action&#8217;,
              
destroy: &#8216;contact/delete.action&#8217;
          
}
      
});
  
[/code]

FYI, this is how my reader looks like:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
var reader = new Ext.data.JsonReader({
          
totalProperty: &#8216;total&#8217;,
          
successProperty: &#8216;success&#8217;,
          
idProperty: &#8216;id&#8217;,
          
root: &#8216;data&#8217;,
          
messageProperty: &#8216;message&#8217; // 


	  

  
[/code]

Add the plugin on you grid declaration:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
var editor = new Ext.ux.grid.RowEditor({
          
saveText: &#8216;Update&#8217;
      
});

// create grid
      
var grid = new Ext.grid.GridPanel({
          
store: store,
          
columns: [
              
{header: "NAME",
               
width: 170,
               
sortable: true,
               
dataIndex: &#8216;name&#8217;,
               
editor: {
                  
xtype: &#8216;textfield&#8217;,
                  
allowBlank: false
              
}},
              
{header: "PHONE #",
               
width: 150,
               
sortable: true,
               
dataIndex: &#8216;phone&#8217;,
               
editor: {
                   
xtype: &#8216;textfield&#8217;,
                   
allowBlank: false
              
}},
              
{header: "EMAIL",
               
width: 150,
               
sortable: true,
               
dataIndex: &#8217;email&#8217;,
               
editor: {
                  
xtype: &#8216;textfield&#8217;,
                  
allowBlank: false
              
}},
              
{header: "BIRTHDAY",
               
width: 100,
               
sortable: true,
               
dataIndex: &#8216;birthday&#8217;,
               
renderer: Ext.util.Format.dateRenderer(&#8216;m/d/Y&#8217;),
               
editor: new Ext.form.DateField ({
                  
allowBlank: false,
                  
format: &#8216;m/d/Y&#8217;,
                  
maxValue: (new Date())
              
})}
          
],
          
plugins: [editor],
          
title: &#8216;My Contacts&#8217;,
          
height: 300,
          
width:610,
		  
frame:true,
		  
tbar: [{
              
iconCls: &#8216;icon-user-add&#8217;,
              
text: &#8216;Add Contact&#8217;,
              
handler: function(){
                  
var e = new Contact({
                      
name: &#8216;New Guy&#8217;,
                      
phone: &#8216;(000) 000-0000&#8217;,
                      
email: &#8216;new@loianetest.com&#8217;,
                      
birthday: &#8217;01/01/2000&#8242;
                  
});
                  
editor.stopEditing();
                  
store.insert(0, e);
                  
grid.getView().refresh();
                  
grid.getSelectionModel().selectRow(0);
                  
editor.startEditing(0);
              
}
          
},{
              
iconCls: &#8216;icon-user-delete&#8217;,
              
text: &#8216;Remove Contact&#8217;,
              
handler: function(){
                  
editor.stopEditing();
                  
var s = grid.getSelectionModel().getSelections();
                  
for(var i = 0, r; r = s[i]; i++){
                      
store.remove(r);
                  
}
              
}
          
},{
              
iconCls: &#8216;icon-user-save&#8217;,
              
text: &#8216;Save All Modifications&#8217;,
              
handler: function(){
                  
store.save();
              
}
          
}]
      
});
  
[/code]

Finally, you need some server side code. This is how my Controller looks like:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.web;

import java.util.HashMap;
  
import java.util.List;
  
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
  
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
  
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

import com.loiane.model.Contact;
  
import com.loiane.service.ContactService;

public class ContactController extends MultiActionController {

private ContactService contactService;

public ModelAndView view(HttpServletRequest request,
			  
HttpServletResponse response) throws Exception {

try{

List contacts = contactService.getContactList();

return getModelMap(contacts);

} catch (Exception e) {

return getModelMapError("Error trying to retrieve contacts.");
		  
}
	  
}

public ModelAndView create(HttpServletRequest request,
			  
HttpServletResponse response) throws Exception {

try{

Object data = request.getParameter("data");

List contacts = contactService.create(data);

return getModelMap(contacts);

} catch (Exception e) {

return getModelMapError("Error trying to create contact.");
		  
}
	  
}

public ModelAndView update(HttpServletRequest request,
			  
HttpServletResponse response) throws Exception {
		  
try{

Object data = request.getParameter("data");

List contacts = contactService.update(data);

return getModelMap(contacts);

} catch (Exception e) {

return getModelMapError("Error trying to update contact.");
		  
}
	  
}

public ModelAndView delete(HttpServletRequest request,
			  
HttpServletResponse response) throws Exception {

try{

String data = request.getParameter("data");

contactService.delete(data);

Map modelMap = new HashMap(3);
			  
modelMap.put("success", true);

return new ModelAndView("jsonView", modelMap);

} catch (Exception e) {

return getModelMapError("Error trying to delete contact.");
		  
}
	  
}

/**
	   
* Generates modelMap to return in the modelAndView
	   
* @param contacts
	   
* @return
	   
*/
	  
private ModelAndView getModelMap(List contacts){

Map modelMap = new HashMap(3);
		  
modelMap.put("total", contacts.size());
		  
modelMap.put("data", contacts);
		  
modelMap.put("success", true);

return new ModelAndView("jsonView", modelMap);
	  
}

/**
	   
* Generates modelMap to return in the modelAndView in case
	   
* of exception
	   
* @param msg message
	   
* @return
	   
*/
	  
private ModelAndView getModelMapError(String msg){

Map modelMap = new HashMap(2);
		  
modelMap.put("message", msg);
		  
modelMap.put("success", false);

return new ModelAndView("jsonView",modelMap);
	  
}

/**
	   
* Spring use &#8211; DI
	   
* @param dadoService
	   
*/
	  
public void setContactService(ContactService contactService) {
		  
this.contactService = contactService;
	  
}

}
  
[/code]

If you want to see all the code (complete project will all the necessary files to run this app), download it from my GitHub repository: http://github.com/loiane/extjs-crud-grid

I just want to make one more observation: You can also use dataWriter to save the data dragged from an excel file and dropped into the grid (remember DataDrop plugin?). I also included this plugin in this code project (check out my Github repository).

Happy coding!


  Updated: I posted a new CRUD DataGrid example, using Spring MVC 3 and Hibernate 3.5: http://loianegroner.com/2010/09/extjs-spring-mvc-3-and-hibernate-3-5-crud-datagrid-example/
