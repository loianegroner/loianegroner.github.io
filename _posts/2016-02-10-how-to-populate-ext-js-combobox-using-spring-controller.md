---
layout: post
title: How to Populate Ext JS ComboBox using Spring Controller
published: true
author: Loiane
comments: true
date: 2010-05-19 08:05:21
tags:
    - ExtJS
    - ExtJS + J2EE
    - ExtJS ComboBox
    - JSON
    - Spring
    - Spring MVC
categories:
    - extjs
permalink: >
    /2010/05/how-to-populate-ext-js-combobox-using-spring-controller
---
This tutorial will walk through how to populate an ExtJS ComboBox using a Spring Controller.


  To populate ExtJS ComboBox using Spring Controller, you need create an Ajax request using Ext.data.HttpProxy and as response you can return JSON Objects (or an XML &#8211; in this example, I will use JSON). And using a JsonReader, you can read values and popolate ComboBox. It is the same logic used to populate an ExtJS DataGrid.



  Frameworks/Libraries I&#8217;m using in this tutorial:



  
    ExtJS (you can download the lastest version here);
  
  
    Spring MVC 3
  
  
    Jackson (used to return JSON from Spring Controller)
  



  If you are using Spring 2.5, you can see my other examples of how to return JSON from Spring using Spring-JSON lib.



  I am going to create a combobox to list all states of Brazil. The screenshot of this tutorial is given bellow:


[][1]

First step is to create  a POJO (Java Object with getters and setters methods only &#8211; and constructor):

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public class State {

private String code;
	  
private String name;

public State(String code, String name) {
		  
super();
		  
this.code = code;
		  
this.name = name;
	  
}

//get and set methods
  
}
  
[/code]

Following is the Controller. The request is mapped to loadStates() method, which retrieves states from a source (I&#8217;m using a method for academic puporses, but you can retrieve data from a database):

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
@Controller
  
public class StateController {

private StateService stateService;

@RequestMapping(value="getStates.json", method = RequestMethod.GET)
	  
public @ResponseBody Map loadStates() {

HashMap modelMap = new HashMap();
		  
modelMap.put("states", stateService.getBrazilianStates());

return modelMap;
	  
}

@Autowired
	  
public void setStateService(StateService stateService) {
		  
this.stateService = stateService;
	  
}
  
}
  
[/code]


  The @ResponseBody converts automaticaly the Map object into a JSON object, because of Jackson. Find out about Spring 3 Ajax simplifications here. This is the JSON object the loadStates method returns to the browser:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
{"states":[{"name":"Acre","code":"AC"},{"name":"Alagoas","code":"AL"},{"name":"Amapá","code":"AP"},{"name":"Amazonas","code":"AM"},{"name":"Bahia","code":"BA"},{"name":"Ceará","code":"CE"},{"name":"Distrito Federal","code":"DF"},{"name":"Espírito Santo","code":"ES"},{"name":"Goiás","code":"GO"},{"name":"Maranhão","code":"MA"},{"name":"Mato Grosso","code":"MT"},{"name":"Mato Grosso do Sul","code":"MS"},{"name":"Minas Gerais","code":"MG"},{"name":"Pará","code":"PA"},{"name":"Paraíba","code":"PB"},{"name":"Paraná","code":"PR"},{"name":"Pernambuco","code":"PE"},{"name":"Piauí","code":"PI"},{"name":"Rio de Janeiro","code":"RJ"},{"name":"Rio Grande do Norte","code":"RN"},{"name":"Rio Grande do Sul","code":"RS"},{"name":"Rondônia","code":"RO"},{"name":"Roraima","code":"RR"},{"name":"Santa Catarina","code":"SC"},{"name":"São Paulo","code":"SP"},{"name":"Sergipe","code":"SE"},{"name":"Tocantins","code":"TO"}]}
  
[/code]

The content above is retrieved from this method in the StateService class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
@Service
  
public class StateService {

public List getBrazilianStates(){

List states = new ArrayList();

states.add(new State("AC","Acre"));
		  
states.add(new State("AL","Alagoas"));
		  
states.add(new State("AP","Amapá"));
		  
states.add(new State("AM","Amazonas"));
		  
states.add(new State("BA","Bahia"));
		  
states.add(new State("CE","Ceará"));
		  
states.add(new State("DF","Distrito Federal"));
		  
states.add(new State("ES","Espírito Santo"));
		  
states.add(new State("GO","Goiás"));
		  
states.add(new State("MA","Maranhão"));
		  
states.add(new State("MT","Mato Grosso"));
		  
states.add(new State("MS","Mato Grosso do Sul"));
		  
states.add(new State("MG","Minas Gerais"));
		  
states.add(new State("PA","Pará"));
		  
states.add(new State("PB","Paraíba"));
		  
states.add(new State("PR","Paraná"));
		  
states.add(new State("PE","Pernambuco"));
		  
states.add(new State("PI","Piauí"));
		  
states.add(new State("RJ","Rio de Janeiro"));
		  
states.add(new State("RN","Rio Grande do Norte"));
		  
states.add(new State("RS","Rio Grande do Sul"));
		  
states.add(new State("RO","Rondônia"));
		  
states.add(new State("RR","Roraima"));
		  
states.add(new State("SC","Santa Catarina"));
		  
states.add(new State("SP","São Paulo"));
		  
states.add(new State("SE","Sergipe"));
		  
states.add(new State("TO","Tocantins"));

return states;
	  
}
  
}
  
[/code]

Finally, here is the ExtJS code for combobox:

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.onReady(function(){

var store = new Ext.data.Store({
		  
proxy: new Ext.data.HttpProxy({
			  
url: &#8216;getStates.json&#8217;
		  
}),
		  
reader: new Ext.data.JsonReader({
			  
root:&#8217;states&#8217;
		  
},
		  
[{name: &#8216;code&#8217;},
		   
{name: &#8216;name&#8217;}
		  
])
	  
});

var combo = new Ext.form.ComboBox({
		  
id: &#8216;statesCombo&#8217;,
		  
store: store,
		  
displayField: &#8216;name&#8217;,
		  
valueField: &#8216;code&#8217;,
		  
hiddenName : &#8216;codeId&#8217;,
		  
typeAhead: true,
		  
mode: &#8216;local&#8217;,
		  
fieldLabel: &#8216;States of Brazil&#8217;,
		  
anchor: &#8216;100%&#8217;,
		  
forceSelection: true,
		  
triggerAction: &#8216;all&#8217;,
		  
emptyText:&#8217;Select a state&#8230;&#8217;,
		  
selectOnFocus:true
	  
});

var stateForm = new Ext.FormPanel({
		  
frame:true,
		  
url: &#8216;saveState.json&#8217;,
		  
title: &#8216;Combo Box Example&#8217;,
		  
bodyStyle:&#8217;padding:5px 5px 0&#8242;,
		  
width: 250,
		  
labelAlign: &#8216;top&#8217;,
		  
layout: &#8216;form&#8217;,
		  
items: [combo]
	  
});

store.load();

stateForm.render(document.body);

});
  
[/code]


  You can download the complete project from my GitHub repository: http://github.com/loiane/extjs-combobox I developed this sample project on Eclipse, and I used TomCat as webserver.


Happy coding!

 [1]: http://loianegroner.com/wp-content/uploads/2010/05/ExtJS_ComboBox_SpringController_Loiane_01.gif