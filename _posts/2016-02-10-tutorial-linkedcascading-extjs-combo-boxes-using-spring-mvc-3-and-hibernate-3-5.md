---
layout: post
title: 'Tutorial: Linked/Cascading ExtJS Combo Boxes using Spring MVC 3 and Hibernate 3.5'
published: true
author: Loiane
comments: true
date: 2010-10-01 08:10:29
tags:
    - cascading comboboxes
    - ExtJS
    - ExtJS + J2EE
    - ExtJS ComboBox
    - hibernate 3
    - linked comboboxes
    - nested comboboxes
    - Spring
    - Spring 3
    - Spring MVC
categories:
    - extjs
permalink: >
    /2010/10/tutorial-linkedcascading-extjs-combo-boxes-using-spring-mvc-3-and-hibernate-3-5
---

  This post will walk you through how to implement ExtJS linked/cascading/nested combo boxes using Spring MVC 3 and Hibernate 3.5



  I am going to use the classic linked combo boxes: state and cities. In this example, I am going to use States and Cities from Brazil! 😉



  



  What is our main goal? When we select a state from the first combo box, the application will load the second combo box with the cities that belong to the selected state.



  There are two ways to implement it.



  The first one is to load all the information you need for both combo boxes, and when user selects a state, the application will filter the cities combo box according to the selected state.



  The second one is to load information only to populate the state combo box. When user selects a state, the application will retrieve all the cities that belong to the selected state from database.



  Which one is best? It depends on the amount of data you have to retrieve from your database. For example: you have a combo box that lists all the countries in the world. And the second combo box represents all the cities in the world. In this case, scenario number 2 is the best option, because you will have to retrieve a large amount of data from the database.



  Ok. Let’s get into the code. I’ll show how to implement both scenarios.



  First, let me explain a little bit of how the project is organized:



  



  Let’s take a look at the Java code.


#### BaseDAO:

Contains the hibernate template used for CityDAO and StateDAO.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import org.hibernate.SessionFactory;
  
import org.springframework.beans.factory.annotation.Autowired;
  
import org.springframework.orm.hibernate3.HibernateTemplate;
  
import org.springframework.stereotype.Repository;

@Repository
  
public abstract class BaseDAO {

private HibernateTemplate hibernateTemplate;

public HibernateTemplate getHibernateTemplate() {
		  
return hibernateTemplate;
	  
}

@Autowired
	  
public void setSessionFactory(SessionFactory sessionFactory) {
		  
hibernateTemplate = new HibernateTemplate(sessionFactory);
	  
}
  
}
  
[/code]

#### CityDAO:


  Contains two methods: one to retrieve all cities from database (used in scenario #1), and one method to retrieve all the cities that belong to a state (used in scenario #2).


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import java.util.List;

import org.hibernate.criterion.DetachedCriteria;
  
import org.hibernate.criterion.Restrictions;
  
import org.springframework.stereotype.Repository;

import com.loiane.model.City;

@Repository
  
public class CityDAO extends BaseDAO{

public List getCityListByState(int stateId) {

DetachedCriteria criteria = DetachedCriteria.forClass(City.class);
		  
criteria.add(Restrictions.eq("stateId", stateId));

return this.getHibernateTemplate().findByCriteria(criteria);

}

public List getCityList() {

DetachedCriteria criteria = DetachedCriteria.forClass(City.class);

return this.getHibernateTemplate().findByCriteria(criteria);

}
  
}
  
[/code]

#### StateDAO:

Contains only one method to retrieve all the states from database.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.dao;

import java.util.List;

import org.hibernate.criterion.DetachedCriteria;
  
import org.springframework.stereotype.Repository;

import com.loiane.model.State;

@Repository
  
public class StateDAO extends BaseDAO{

public List getStateList() {

DetachedCriteria criteria = DetachedCriteria.forClass(State.class);

return this.getHibernateTemplate().findByCriteria(criteria);

}
  
}
  
[/code]

#### City:

Represents the City POJO, represents the City table.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

import javax.persistence.Column;
  
import javax.persistence.Entity;
  
import javax.persistence.GeneratedValue;
  
import javax.persistence.Id;
  
import javax.persistence.Table;

import org.codehaus.jackson.annotate.JsonAutoDetect;

@JsonAutoDetect
  
@Entity
  
@Table(name="CITY")
  
public class City {

private int id;
	  
private int stateId;
	  
private String name;

//getters and setters
  
}
  
[/code]

#### State:

Represents the State POJO, represents the State table.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

import javax.persistence.Column;
  
import javax.persistence.Entity;
  
import javax.persistence.GeneratedValue;
  
import javax.persistence.Id;
  
import javax.persistence.Table;

import org.codehaus.jackson.annotate.JsonAutoDetect;

@JsonAutoDetect
  
@Entity
  
@Table(name="STATE")
  
public class State {

private int id;
	  
private int countryId;
	  
private String code;
	  
private String name;

//getters and setters
  
}

[/code]

#### CityService:


  Contains two methods: one to retrieve all cities from database (used in scenario #1), and one method to retrieve all the cities that belong to a state (used in scenario #2). Only makes a call to CityDAO class.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
  
import org.springframework.stereotype.Service;

import com.loiane.dao.CityDAO;
  
import com.loiane.model.City;

@Service
  
public class CityService {

private CityDAO cityDAO;

public List getCityListByState(int stateId) {
		  
return cityDAO.getCityListByState(stateId);
	  
}

public List getCityList() {
		  
return cityDAO.getCityList();
	  
}

@Autowired
	  
public void setCityDAO(CityDAO cityDAO) {
		  
this.cityDAO = cityDAO;
	  
}
  
}
  
[/code]

#### StateService:

Contains only one method to retrieve all the states from database. Makes a call to StateDAO.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
  
import org.springframework.stereotype.Service;

import com.loiane.dao.StateDAO;
  
import com.loiane.model.State;

@Service
  
public class StateService {

private StateDAO stateDAO;

public List getStateList() {
		  
return stateDAO.getStateList();
	  
}

@Autowired
	  
public void setStateDAO(StateDAO stateDAO) {
		  
this.stateDAO = stateDAO;
	  
}
  
}
  
[/code]

#### CityController:


  Contains two methods: one to retrieve all cities from database (used in scenario #1), and one method to retrieve all the cities that belong to a state (used in scenario #2). Only makes a call to CityService class. Both methods return a JSON object that looks like this:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
{"data":[
           
{"stateId":1,"name":"Acrelândia","id":1},
           
{"stateId":1,"name":"Assis Brasil","id":2},
           
{"stateId":1,"name":"Brasiléia","id":3},
           
{"stateId":1,"name":"Bujari","id":4},
           
{"stateId":1,"name":"Capixaba","id":5},
           
{"stateId":1,"name":"Cruzeiro do Sul","id":6},
           
{"stateId":1,"name":"Epitaciolândia","id":7},
           
{"stateId":1,"name":"Feijó","id":8},
           
{"stateId":1,"name":"Jordão","id":9},
           
{"stateId":1,"name":"Mâncio Lima","id":10},
  
]}
  
[/code]

Class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.web;

import java.util.HashMap;
  
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
  
import org.springframework.stereotype.Controller;
  
import org.springframework.web.bind.annotation.RequestMapping;
  
import org.springframework.web.bind.annotation.RequestParam;
  
import org.springframework.web.bind.annotation.ResponseBody;

import com.loiane.service.CityService;

@Controller
  
@RequestMapping(value="/city")
  
public class CityController {

private CityService cityService;

@RequestMapping(value="/getCitiesByState.action")
	  
public @ResponseBody Map getCitiesByState(@RequestParam int stateId) throws Exception {

Map modelMap = new HashMap(3);

try{

modelMap.put("data", cityService.getCityListByState(stateId));

return modelMap;

} catch (Exception e) {

e.printStackTrace();

modelMap.put("success", false);

return modelMap;
		  
}
	  
}

@RequestMapping(value="/getAllCities.action")
	  
public @ResponseBody Map getAllCities() throws Exception {

Map modelMap = new HashMap(3);

try{

modelMap.put("data", cityService.getCityList());

return modelMap;

} catch (Exception e) {

e.printStackTrace();

modelMap.put("success", false);

return modelMap;
		  
}
	  
}

@Autowired
	  
public void setCityService(CityService cityService) {
		  
this.cityService = cityService;
	  
}
  
}
  
[/code]

#### StateController:


  Contains only one method to retrieve all the states from database. Makes a call to StateService. The method returns a JSON object that looks like this:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
{"data":[
           
{"countryId":1,"name":"Acre","id":1,"code":"AC"},
           
{"countryId":1,"name":"Alagoas","id":2,"code":"AL"},
           
{"countryId":1,"name":"Amapá","id":3,"code":"AP"},
           
{"countryId":1,"name":"Amazonas","id":4,"code":"AM"},
           
{"countryId":1,"name":"Bahia","id":5,"code":"BA"},
           
{"countryId":1,"name":"Ceará","id":6,"code":"CE"},
           
{"countryId":1,"name":"Distrito Federal","id":7,"code":"DF"},
           
{"countryId":1,"name":"Espírito Santo","id":8,"code":"ES"},
           
{"countryId":1,"name":"Goiás","id":9,"code":"GO"},
           
{"countryId":1,"name":"Maranhão","id":10,"code":"MA"},
           
{"countryId":1,"name":"Mato Grosso","id":11,"code":"MT"},
           
{"countryId":1,"name":"Mato Grosso do Sul","id":12,"code":"MS"},
           
{"countryId":1,"name":"Minas Gerais","id":13,"code":"MG"},
           
{"countryId":1,"name":"Pará","id":14,"code":"PA"},
  
]}
  
[/code]

Class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.web;

import java.util.HashMap;
  
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
  
import org.springframework.stereotype.Controller;
  
import org.springframework.web.bind.annotation.RequestMapping;
  
import org.springframework.web.bind.annotation.ResponseBody;

import com.loiane.service.StateService;

@Controller
  
@RequestMapping(value="/state")
  
public class StateController {

private StateService stateService;

@RequestMapping(value="/view.action")
	  
public @ResponseBody Map view() throws Exception {

Map modelMap = new HashMap(3);

try{

modelMap.put("data", stateService.getStateList());

return modelMap;

} catch (Exception e) {

e.printStackTrace();

modelMap.put("success", false);

return modelMap;
		  
}
	  
}

@Autowired
	  
public void setStateService(StateService stateService) {
		  
this.stateService = stateService;
	  
}
  
}
  
[/code]


  Inside WebContent folder we have:



  
    ext-3.2.1 – contains all ExtJS Files
  
  
    js – contains all javascript files I implemented for this example. liked-comboboxes-local.js contains the combo boxes for  scenario #1; liked-comboboxes-remote.js contains the combo boxes for  scenario #2; linked-comboboxes.js contains a tab panel that contains both scenarios.
  



  Now let’s take a look at the ExtJS code.



  Scenario Number 1:



  Retrieve all the data from database to populate both combo boxes. Will user filter on cities combo box.



  liked-comboboxes-local.js:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
	  
var localForm = new Ext.FormPanel({
         
width: 400
         
,height: 300
         
,style:&#8217;margin:16px&#8217;
         
,bodyStyle:&#8217;padding:10px&#8217;
         
,title:&#8217;Linked Combos &#8211; Local Filtering&#8217;
         
,defaults: {xtype:&#8217;combo&#8217;}
         
,items:[{
              
fieldLabel:&#8217;Select State&#8217;
             
,displayField:&#8217;name&#8217;
             
,valueField:&#8217;id&#8217;
             
,store: new Ext.data.JsonStore({
	       		  
url: &#8216;state/view.action&#8217;,
	              
remoteSort: false,
	              
autoLoad:true,
	              
idProperty: &#8216;id&#8217;,
	              
root: &#8216;data&#8217;,
	              
totalProperty: &#8216;total&#8217;,
	              
fields: [&#8216;id&#8217;,&#8217;name&#8217;]
	          
})
             
,triggerAction:&#8217;all&#8217;
             
,mode:&#8217;local&#8217;
             
,listeners:{select:{fn:function(combo, value) {
                 
var comboCity = Ext.getCmp(&#8216;combo-city-local&#8217;);
                 
comboCity.clearValue();
                 
comboCity.store.filter(&#8216;stateId&#8217;, combo.getValue());
                 
}}
             
}

},{
              
fieldLabel:&#8217;Select City&#8217;
             
,displayField:&#8217;name&#8217;
             
,valueField:&#8217;id&#8217;
             
,id:&#8217;combo-city-local&#8217;
             
,store: new Ext.data.JsonStore({
       		     
url: &#8216;city/getAllCities.action&#8217;,
                 
remoteSort: false,
                 
autoLoad:true,
                 
idProperty: &#8216;id&#8217;,
                 
root: &#8216;data&#8217;,
                 
totalProperty: &#8216;total&#8217;,
                 
fields: [&#8216;id&#8217;,&#8217;stateId&#8217;,&#8217;name&#8217;]
             
})
             
,triggerAction:&#8217;all&#8217;
             
,mode:&#8217;local&#8217;
             
,lastQuery:&#8221;
         
}]
     
});
  
[/code]

The state combo box is declared on lines 9 to 28.

The city combo box is declared on lines 31 to 46.

Note that both stores are loaded when we load the page, as we can see in lines 15 and 38 (autoload:true).

The state combo box has **select event listener** that, when executed, filters the cities combo (the child combo) based on the currently selected state. You can see it on lines 23 to 28.

Cities combo has **lastQuery:&#8221;&#8221;**. This is to fool internal combo filtering routines on the first page load. The cities combo just thinks that it has already been expanded once.


  Scenario Number 2:


Retrieve all the state data from database to populate state combo. When user selects a state, application will retrieve from database only selected information.

liked-comboboxes-remote.js:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
var dataBaseForm = new Ext.FormPanel({
         
width: 400
         
,height: 200
         
,style:&#8217;margin:16px&#8217;
         
,bodyStyle:&#8217;padding:10px&#8217;
         
,title:&#8217;Linked Combos &#8211; Database&#8217;
         
,defaults: {xtype:&#8217;combo&#8217;}
         
,items:[{
              
fieldLabel:&#8217;Select State&#8217;
             
,displayField:&#8217;name&#8217;
             
,valueField:&#8217;id&#8217;
             
,store: new Ext.data.JsonStore({
	       		  
url: &#8216;state/view.action&#8217;,
	              
remoteSort: false,
	              
autoLoad:true,
	              
idProperty: &#8216;id&#8217;,
	              
root: &#8216;data&#8217;,
	              
totalProperty: &#8216;total&#8217;,
	              
fields: [&#8216;id&#8217;,&#8217;name&#8217;]
	          
})
             
,triggerAction:&#8217;all&#8217;
             
,mode:&#8217;local&#8217;
             
,listeners: {
                 
select: {
                     
fn:function(combo, value) {
                         
var comboCity = Ext.getCmp(&#8216;combo-city&#8217;);
                         
//set and disable cities
                         
comboCity.setDisabled(true);
                         
comboCity.setValue(&#8221;);
                         
comboCity.store.removeAll();
                         
//reload city store and enable city combobox
                         
comboCity.store.reload({
                             
params: { stateId: combo.getValue() }
                         
});
                         
comboCity.setDisabled(false);
       			    
}
                 
}
       		  
}
         
},{
              
fieldLabel:&#8217;Select City&#8217;
             
,displayField:&#8217;name&#8217;
             
,valueField:&#8217;id&#8217;
             
,disabled:true
             
,id:&#8217;combo-city&#8217;
             
,store: new Ext.data.JsonStore({
       		     
url: &#8216;city/getCitiesByState.action&#8217;,
                 
remoteSort: false,
                 
idProperty: &#8216;id&#8217;,
                 
root: &#8216;data&#8217;,
                 
totalProperty: &#8216;total&#8217;,
                 
fields: [&#8216;id&#8217;,&#8217;stateId&#8217;,&#8217;name&#8217;]
             
})
             
,triggerAction:&#8217;all&#8217;
             
,mode:&#8217;local&#8217;
             
,lastQuery:&#8221;
         
}]
  
});
  
[/code]

The state combo box is declared on lines 9 to 38.

The city combo box is declared on lines 40 to 55.

Note that only state combo store is loaded when we load the page, as we can see at the line 15 (autoload:true).

The state combo box has the **select event listener** that, when executed, reloads the data for cities store (passes stateId as parameter) based on the currently selected state. You can see it on lines 24 to 38.

Cities combo has **lastQuery:&#8221;&#8221;**. This is to fool internal combo filtering routines on the first page load. The cities combo just thinks that it has already been expanded once.

You can download the complete project from my **GitHub** repository:

I used Eclipse IDE + TomCat 7 to develop this sample project.

References: [http://www.sencha.com/learn/Tutorial:Linked\_Combos\_Tutorial\_for\_Ext_2][1]

Happy coding!

 [1]: http://www.sencha.com/learn/Tutorial:Linked_Combos_Tutorial_for_Ext_2