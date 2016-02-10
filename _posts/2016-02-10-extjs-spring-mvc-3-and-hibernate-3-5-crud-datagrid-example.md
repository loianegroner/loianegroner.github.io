---
layout: post
title: 'ExtJS, Spring MVC 3 and Hibernate 3.5: CRUD DataGrid Example'
published: true
author: Loiane
comments: true
date: 2010-09-02 08:09:27
tags:
    - CRUD Grid
    - ExtJS
    - ExtJS + J2EE
    - ExtJS Grid
    - Hibernate
    - hibernate 3
    - hibernate annotations
    - Spring
    - Spring 3
    - Spring MVC
categories:
    - extjs
    - hibernate
    - spring
permalink: >
    /2010/09/extjs-spring-mvc-3-and-hibernate-3-5-crud-datagrid-example
---

  



  This tutorial will walk through how to implement a CRUD (Create, Read, Update, Delete) DataGrid using ExtJS, Spring MVC 3 and Hibernate 3.5.



  What do we usually want to do with data?



  
    Create (Insert)
  
  
    Read / Retrieve (Select)
  
  
    Update (Update)
  
  
    Delete / Destroy (Delete)
  



  Until ExtJS 3.0 we only could READ data using a dataGrid. If you wanted to update, insert or delete, you had to do some code to make these actions work. Now ExtJS 3.0 (and newest versions) introduces the ext.data.writer, and you do not need all that work to have a CRUD Grid.



  So… What do I need to add in my code to make all these things working together?



  In this example, I’m going to use JSON as data format exchange between the browser and the server.



  ExtJS Code



  First, you need an Ext.data.JsonWriter:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
   
// The new DataWriter component.
      
var writer = new Ext.data.JsonWriter({
          
encode: true,
          
writeAllFields: true
      
});
  
[/code]

Where writeAllFields identifies that we want to write all the fields from the record to the database. If you have a fancy ORM then maybe you can set this to false. In this example, I&#8217;m using Hibernate, and we have saveOrUpate method &#8211; in this case, we need all fields to updated the object in database, so we have to ser writeAllFields to true. This is my record type declaration:

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
      
}]);
  
[/code]


  Now you need to setup a proxy like this one:


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
              
}})}
          
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
                      
email: &#8216;new@loianetest.com&#8217;
                  
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

### Java code

Finally, you need some server side code.

#### Controller:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.web;

@Controller
  
public class ContactController {

private ContactService contactService;

@RequestMapping(value="/contact/view.action")
	  
public @ResponseBody Map view() throws Exception {

try{

List contacts = contactService.getContactList();

return getMap(contacts);

} catch (Exception e) {

return getModelMapError("Error retrieving Contacts from database.");
		  
}
	  
}

@RequestMapping(value="/contact/create.action")
	  
public @ResponseBody Map create(@RequestParam Object data) throws Exception {

try{

List contacts = contactService.create(data);

return getMap(contacts);

} catch (Exception e) {

return getModelMapError("Error trying to create contact.");
		  
}
	  
}

@RequestMapping(value="/contact/update.action")
	  
public @ResponseBody Map update(@RequestParam Object data) throws Exception {
		  
try{

List contacts = contactService.update(data);

return getMap(contacts);

} catch (Exception e) {

return getModelMapError("Error trying to update contact.");
		  
}
	  
}

@RequestMapping(value="/contact/delete.action")
	  
public @ResponseBody Map delete(@RequestParam Object data) throws Exception {

try{

contactService.delete(data);

Map modelMap = new HashMap(3);
			  
modelMap.put("success", true);

return modelMap;

} catch (Exception e) {

return getModelMapError("Error trying to delete contact.");
		  
}
	  
}

private Map getMap(List contacts){

Map modelMap = new HashMap(3);
		  
modelMap.put("total", contacts.size());
		  
modelMap.put("data", contacts);
		  
modelMap.put("success", true);

return modelMap;
	  
}

private Map getModelMapError(String msg){

Map modelMap = new HashMap(2);
		  
modelMap.put("message", msg);
		  
modelMap.put("success", false);

return modelMap;
	  
}

@Autowired
	  
public void setContactService(ContactService contactService) {
		  
this.contactService = contactService;
	  
}

}
  
[/code]

Some observations:


  In Spring 3, we can get the objects from requests directly in the method parameters using @RequestParam. I don&#8217;t know why, but it did not work with ExtJS. I had to leave as an Object and to the JSON-Object parser myself. That is why I&#8217;m using a Util class &#8211; to parser the object from request into my POJO class. If you know how I can replace Object parameter from controller methods, please, leave a comment, because I&#8217;d really like to know that! 


#### Service Class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.service;

@Service
  
public class ContactService {

private ContactDAO contactDAO;
	  
private Util util;

@Transactional(readOnly=true)
	  
public List getContactList(){

return contactDAO.getContacts();
	  
}

@Transactional
	  
public List create(Object data){

List newContacts = new ArrayList();

List list = util.getContactsFromRequest(data);

for (Contact contact : list){
			  
newContacts.add(contactDAO.saveContact(contact));
		  
}

return newContacts;
	  
}

@Transactional
	  
public List update(Object data){

List returnContacts = new ArrayList();

List updatedContacts = util.getContactsFromRequest(data);

for (Contact contact : updatedContacts){
			  
returnContacts.add(contactDAO.saveContact(contact));
		  
}

return returnContacts;
	  
}

@Transactional
	  
public void delete(Object data){

//it is an array &#8211; have to cast to array object
		  
if (data.toString().indexOf(&#8216;[&#8216;) > -1){

List deleteContacts = util.getListIdFromJSON(data);

for (Integer id : deleteContacts){
				  
contactDAO.deleteContact(id);
			  
}

} else { //it is only one object &#8211; cast to object/bean

Integer id = Integer.parseInt(data.toString());

contactDAO.deleteContact(id);
		  
}
	  
}

@Autowired
	  
public void setContactDAO(ContactDAO contactDAO) {
		  
this.contactDAO = contactDAO;
	  
}

@Autowired
	  
public void setUtil(Util util) {
		  
this.util = util;
	  
}
  
}
  
[/code]

#### Contact Calss &#8211; POJO:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

@JsonAutoDetect
  
@Entity
  
@Table(name="CONTACT")
  
public class Contact {

private int id;
	  
private String name;
	  
private String phone;
	  
private String email;

@Id
	  
@GeneratedValue
	  
@Column(name="CONTACT_ID")
	  
public int getId() {
		  
return id;
	  
}

public void setId(int id) {
		  
this.id = id;
	  
}

@Column(name="CONTACT_NAME", nullable=false)
	  
public String getName() {
		  
return name;
	  
}

public void setName(String name) {
		  
this.name = name;
	  
}

@Column(name="CONTACT_PHONE", nullable=false)
	  
public String getPhone() {
		  
return phone;
	  
}

public void setPhone(String phone) {
		  
this.phone = phone;
	  
}

@Column(name="CONTACT_EMAIL", nullable=false)
	  
public String getEmail() {
		  
return email;
	  
}

public void setEmail(String email) {
		  
this.email = email;
	  
}
  
}
  
[/code]

#### DAO Class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

@Repository
  
public class ContactDAO implements IContactDAO{

private HibernateTemplate hibernateTemplate;

@Autowired
	  
public void setSessionFactory(SessionFactory sessionFactory) {
		  
hibernateTemplate = new HibernateTemplate(sessionFactory);
	  
}

@SuppressWarnings("unchecked")
	  
@Override
	  
public List getContacts() {
		  
return hibernateTemplate.find("from Contact");
	  
}

@Override
	  
public void deleteContact(int id){
		  
Object record = hibernateTemplate.load(Contact.class, id);
		  
hibernateTemplate.delete(record);
	  
}

@Override
	  
public Contact saveContact(Contact contact){
		  
hibernateTemplate.saveOrUpdate(contact);
		  
return contact;
	  
}
  
}
  
[/code]

#### Util Class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.util;

@Component
  
public class Util {

public List getContactsFromRequest(Object data){

List list;

//it is an array &#8211; have to cast to array object
		  
if (data.toString().indexOf(&#8216;[&#8216;) > -1){

list = getListContactsFromJSON(data);

} else { //it is only one object &#8211; cast to object/bean

Contact contact = getContactFromJSON(data);

list = new ArrayList();
			  
list.add(contact);
		  
}

return list;
	  
}

private Contact getContactFromJSON(Object data){
		  
JSONObject jsonObject = JSONObject.fromObject(data);
		  
Contact newContact = (Contact) JSONObject.toBean(jsonObject, Contact.class);
		  
return newContact;
	  
}
  
)
	  
private List getListContactsFromJSON(Object data){
		  
JSONArray jsonArray = JSONArray.fromObject(data);
		  
List newContacts = (List) JSONArray.toCollection(jsonArray,Contact.class);
		  
return newContacts;
	  
}

public List getListIdFromJSON(Object data){
		  
JSONArray jsonArray = JSONArray.fromObject(data);
		  
List idContacts = (List) JSONArray.toCollection(jsonArray,Integer.class);
		  
return idContacts;
	  
}
  
}
  
[/code]

If you want to see all the code (complete project will all the necessary files to run this app), download it from my GitHub repository: http://github.com/loiane/extjs-crud-grid-spring-hibernate

This was a requested post. I&#8217;ve got a lot of comments from my previous CRUD Grid example and some emails. I made some adjustments to current code, but the idea is still the same. I hope I was able answer all the questions. 

Happy coding!