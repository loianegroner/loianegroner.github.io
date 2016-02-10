---
layout: post
title: 'ExtJS 4 MVC Example: Ajax with XML Forms'
published: true
author: Loiane
comments: true
date: 2012-05-24 05:05:23
tags:
    - ExtJS 4
    - ExtJS 4 MVC
categories:
    - ext-js-4
permalink: /2012/05/extjs-4-mvc-example-ajax-with-xml-forms
image:
    feature: extjs4-mvc-ajax-xml-form-loiane.jpg
---
Another ExtJS 4 MVC Example. Today we are going to port the  Ajax with XML Forms to MVC.

Let’s get started!

[][1]

## Project’s Structure

[][2]

## Model &#8211; Contact

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.model.Contact&#8217;,{
      
extend: &#8216;Ext.data.Model&#8217;,
      
fields: [
          
{name: &#8216;first&#8217;, mapping: &#8216;name > first&#8217;},
          
{name: &#8216;last&#8217;, mapping: &#8216;name > last&#8217;},
          
&#8216;company&#8217;,
          
&#8217;email&#8217;,
          
&#8216;state&#8217;,
          
{name: &#8216;dob&#8217;, type: &#8216;date&#8217;, dateFormat: &#8216;m/d/Y&#8217;}
      
]
  
});
  
[/code]

## Model &#8211; FieldError

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.model.FieldError&#8217;,{
      
extend: &#8216;Ext.data.Model&#8217;,
      
fields: [&#8216;id&#8217;, &#8216;msg&#8217;]
  
});
  
[/code]

## Model &#8211; State

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.model.State&#8217;,{
      
extend: &#8216;Ext.data.Model&#8217;,
      
fields: [&#8216;abbr&#8217;, &#8216;state&#8217;]
  
});
  
[/code]

## Store &#8211; States

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.store.States&#8217;,{
      
extend: &#8216;Ext.data.ArrayStore&#8217;,

model: &#8216;ExtMVC.model.State&#8217;,

data: [
          
[&#8216;AL&#8217;, &#8216;Alabama&#8217;, &#8216;The Heart of Dixie&#8217;],
          
[&#8216;AK&#8217;, &#8216;Alaska&#8217;, &#8216;The Land of the Midnight Sun&#8217;],
          
[&#8216;AZ&#8217;, &#8216;Arizona&#8217;, &#8216;The Grand Canyon State&#8217;],
          
[&#8216;AR&#8217;, &#8216;Arkansas&#8217;, &#8216;The Natural State&#8217;],
          
[&#8216;CA&#8217;, &#8216;California&#8217;, &#8216;The Golden State&#8217;],
          
[&#8216;CO&#8217;, &#8216;Colorado&#8217;, &#8216;The Mountain State&#8217;],
          
[&#8216;CT&#8217;, &#8216;Connecticut&#8217;, &#8216;The Constitution State&#8217;],
          
[&#8216;DE&#8217;, &#8216;Delaware&#8217;, &#8216;The First State&#8217;],
          
[&#8216;DC&#8217;, &#8216;District of Columbia&#8217;, "The Nation&#8217;s Capital"],
          
[&#8216;FL&#8217;, &#8216;Florida&#8217;, &#8216;The Sunshine State&#8217;],
          
[&#8216;GA&#8217;, &#8216;Georgia&#8217;, &#8216;The Peach State&#8217;],
          
[&#8216;HI&#8217;, &#8216;Hawaii&#8217;, &#8216;The Aloha State&#8217;],
          
[&#8216;ID&#8217;, &#8216;Idaho&#8217;, &#8216;Famous Potatoes&#8217;],
          
[&#8216;IL&#8217;, &#8216;Illinois&#8217;, &#8216;The Prairie State&#8217;],
          
[&#8216;IN&#8217;, &#8216;Indiana&#8217;, &#8216;The Hospitality State&#8217;],
          
[&#8216;IA&#8217;, &#8216;Iowa&#8217;, &#8216;The Corn State&#8217;],
          
[&#8216;KS&#8217;, &#8216;Kansas&#8217;, &#8216;The Sunflower State&#8217;],
          
[&#8216;KY&#8217;, &#8216;Kentucky&#8217;, &#8216;The Bluegrass State&#8217;],
          
[&#8216;LA&#8217;, &#8216;Louisiana&#8217;, &#8216;The Bayou State&#8217;],
          
[&#8216;ME&#8217;, &#8216;Maine&#8217;, &#8216;The Pine Tree State&#8217;],
          
[&#8216;MD&#8217;, &#8216;Maryland&#8217;, &#8216;Chesapeake State&#8217;],
          
[&#8216;MA&#8217;, &#8216;Massachusetts&#8217;, &#8216;The Spirit of America&#8217;],
          
[&#8216;MI&#8217;, &#8216;Michigan&#8217;, &#8216;Great Lakes State&#8217;],
          
[&#8216;MN&#8217;, &#8216;Minnesota&#8217;, &#8216;North Star State&#8217;],
          
[&#8216;MS&#8217;, &#8216;Mississippi&#8217;, &#8216;Magnolia State&#8217;],
          
[&#8216;MO&#8217;, &#8216;Missouri&#8217;, &#8216;Show Me State&#8217;],
          
[&#8216;MT&#8217;, &#8216;Montana&#8217;, &#8216;Big Sky Country&#8217;],
          
[&#8216;NE&#8217;, &#8216;Nebraska&#8217;, &#8216;Beef State&#8217;],
          
[&#8216;NV&#8217;, &#8216;Nevada&#8217;, &#8216;Silver State&#8217;],
          
[&#8216;NH&#8217;, &#8216;New Hampshire&#8217;, &#8216;Granite State&#8217;],
          
[&#8216;NJ&#8217;, &#8216;New Jersey&#8217;, &#8216;Garden State&#8217;],
          
[&#8216;NM&#8217;, &#8216;New Mexico&#8217;, &#8216;Land of Enchantment&#8217;],
          
[&#8216;NY&#8217;, &#8216;New York&#8217;, &#8216;Empire State&#8217;],
          
[&#8216;NC&#8217;, &#8216;North Carolina&#8217;, &#8216;First in Freedom&#8217;],
          
[&#8216;ND&#8217;, &#8216;North Dakota&#8217;, &#8216;Peace Garden State&#8217;],
          
[&#8216;OH&#8217;, &#8216;Ohio&#8217;, &#8216;The Heart of it All&#8217;],
          
[&#8216;OK&#8217;, &#8216;Oklahoma&#8217;, &#8216;Oklahoma is OK&#8217;],
          
[&#8216;OR&#8217;, &#8216;Oregon&#8217;, &#8216;Pacific Wonderland&#8217;],
          
[&#8216;PA&#8217;, &#8216;Pennsylvania&#8217;, &#8216;Keystone State&#8217;],
          
[&#8216;RI&#8217;, &#8216;Rhode Island&#8217;, &#8216;Ocean State&#8217;],
          
[&#8216;SC&#8217;, &#8216;South Carolina&#8217;, &#8216;Nothing Could be Finer&#8217;],
          
[&#8216;SD&#8217;, &#8216;South Dakota&#8217;, &#8216;Great Faces, Great Places&#8217;],
          
[&#8216;TN&#8217;, &#8216;Tennessee&#8217;, &#8216;Volunteer State&#8217;],
          
[&#8216;TX&#8217;, &#8216;Texas&#8217;, &#8216;Lone Star State&#8217;],
          
[&#8216;UT&#8217;, &#8216;Utah&#8217;, &#8216;Salt Lake State&#8217;],
          
[&#8216;VT&#8217;, &#8216;Vermont&#8217;, &#8216;Green Mountain State&#8217;],
          
[&#8216;VA&#8217;, &#8216;Virginia&#8217;, &#8216;Mother of States&#8217;],
          
[&#8216;WA&#8217;, &#8216;Washington&#8217;, &#8216;Green Tree State&#8217;],
          
[&#8216;WV&#8217;, &#8216;West Virginia&#8217;, &#8216;Mountain State&#8217;],
          
[&#8216;WI&#8217;, &#8216;Wisconsin&#8217;, "America&#8217;s Dairyland"],
          
[&#8216;WY&#8217;, &#8216;Wyoming&#8217;, &#8216;Like No Place on Earth&#8217;]
      
]
  
});
  
[/code]

## View &#8211; Form

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.view.contact.ContactForm&#8217; ,{
      
extend: &#8216;Ext.form.Panel&#8217;,
      
alias : &#8216;widget.contactform&#8217;,

frame: true,
      
title:&#8217;XML Form&#8217;,
      
bodyPadding: 5,
      
waitMsgTarget: true,

fieldDefaults: {
              
labelAlign: &#8216;right&#8217;,
              
labelWidth: 85,
              
msgTarget: &#8216;side&#8217;
       
},

items: [{
          
xtype: &#8216;fieldset&#8217;,
          
title: &#8216;Contact Information&#8217;,
          
defaultType: &#8216;textfield&#8217;,
          
defaults: {
              
width: 280
          
},
          
items: [{
                  
fieldLabel: &#8216;First Name&#8217;,
                  
emptyText: &#8216;First Name&#8217;,
                  
name: &#8216;first&#8217;
              
}, {
                  
fieldLabel: &#8216;Last Name&#8217;,
                  
emptyText: &#8216;Last Name&#8217;,
                  
name: &#8216;last&#8217;
              
}, {
                  
fieldLabel: &#8216;Company&#8217;,
                  
name: &#8216;company&#8217;
              
}, {
                  
fieldLabel: &#8216;Email&#8217;,
                  
name: &#8217;email&#8217;,
                  
vtype:&#8217;email&#8217;
              
}, {
                  
xtype: &#8216;combobox&#8217;,
                  
fieldLabel: &#8216;State&#8217;,
                  
name: &#8216;state&#8217;,
                  
store: &#8216;States&#8217;,
                  
valueField: &#8216;abbr&#8217;,
                  
displayField: &#8216;state&#8217;,
                  
typeAhead: true,
                  
queryMode: &#8216;local&#8217;,
                  
emptyText: &#8216;Select a state&#8230;&#8217;
              
}, {
                  
xtype: &#8216;datefield&#8217;,
                  
fieldLabel: &#8216;Date of Birth&#8217;,
                  
name: &#8216;dob&#8217;,
                  
allowBlank: false,
                  
maxValue: new Date()
              
}
          
]
      
}],

buttons: [{
          
text: &#8216;Load&#8217;,
          
action: &#8216;load&#8217;
      
}, {
          
text: &#8216;Submit&#8217;,
          
disabled: true,
          
formBind: true,
          
action: &#8216;submit&#8217;
      
}]

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
          
&#8216;ExtMVC.view.contact.ContactForm&#8217;
      
],

initComponent: function() {
          
var me = this;

Ext.apply(me, {
              
items: [
                  
{
                      
xtype: &#8216;contactform&#8217;,
                      
reader : Ext.create(&#8216;Ext.data.reader.Xml&#8217;, {
                          
model: &#8216;ExtMVC.model.Contact&#8217;,
                          
record : &#8216;contact&#8217;,
                          
successProperty: &#8216;@success&#8217;
                      
}),
                      
errorReader: Ext.create(&#8216;Ext.data.reader.Xml&#8217;, {
                          
model: &#8216;ExtMVC.model.FieldError&#8217;,
                          
record : &#8216;field&#8217;,
                          
successProperty: &#8216;@success&#8217;
                      
})
                  
}
              
]
          
});

me.callParent(arguments);
      
}
  
});
  
[/code]

## Controller

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.define(&#8216;ExtMVC.controller.Contacts&#8217;, {
      
extend: &#8216;Ext.app.Controller&#8217;,

models: [&#8216;Contact&#8217;, &#8216;FieldError&#8217;, &#8216;State&#8217;],

stores: [&#8216;States&#8217;],

views: [&#8216;contact.ContactForm&#8217;],

refs: [{
          
ref: &#8216;contactForm&#8217;,
          
selector: &#8216;contactform&#8217;
      
}],

init: function() {

this.control({
        	  
&#8216;contactform button[action=load]&#8217;: {
        		  
click: this.loadFormData
        	  
},
        	  
&#8216;contactform button[action=submit]&#8217;: {
        		  
click: this.submitFormData
        	  
}
          
});
      
},

loadFormData: function() {

this.getContactForm().getForm().load({
              
url: &#8216;data/xml-form-data.xml&#8217;,
              
waitMsg: &#8216;Loading&#8230;&#8217;
		  
});
	  
},

submitFormData: function() {

this.getContactForm().getForm().submit({
              
url: &#8216;data/xml-form-errors.xml&#8217;,
              
submitEmptyText: false,
              
waitMsg: &#8216;Saving Data&#8230;&#8217;,

success: function(form, action) {
                 
Ext.Msg.alert(&#8216;Success&#8217;, action.result.msg);
              
},
              
failure: function(form, action) {
                  
Ext.Msg.alert(&#8216;Failed&#8217;, action.result ? action.result.msg : &#8216;No response&#8217;);
              
}
          
});
	  
}
  
});
  
[/code]

## App.js

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.application({
      
name: &#8216;ExtMVC&#8217;,

controllers: [
          
&#8216;Contacts&#8217;
      
],

autoCreateViewport: true
  
});
  
[/code]

## HTML

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

	  
Ext JS 4 MVC Examples &#8211; loiane.com


	  

      



      



  

  

  

  
[/code]

## Demo

To see this example running live, please go to:  http://loiane.com/extjs/extjs4-mvc-ajax-xml-form/

## Download the Source Code

You can download the full source code from my Github repository: 

### More ExtJS 4 MVC Examples:

http://loianegroner.com/2012/03/extjs-4-mvc-examples/

Happy Coding! ![:)][3]

 [1]: http://loianegroner.com/wp-content/uploads/2012/05/extjs4-mvc-ajax-xml-form-loiane.jpg
 [2]: http://loianegroner.com/wp-content/uploads/2012/05/extjs4-mvc-ajax-xml-form-loiane-011.png
 [3]: http://loianegroner.com/wp-includes/images/smilies/icon_smile.gif