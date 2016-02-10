---
layout: post
title: ExtJS 4 File Upload + Spring MVC 3 Example
published: true
author: Loiane
comments: true
date: 2011-07-18 07:07:32
tags:
    - Ext JS 4
    - Spring MVC 3
categories:
    - ext-js-4
    - spring
permalink: /2011/07/extjs-4-file-upload-spring-mvc-3-example
---

  This tutorial will walk you through out how to use the Ext JS 4 File Upload Field in the front end and Spring MVC 3 in the back end.



  This tutorial is also an update for the tutorial Ajax File Upload with ExtJS and Spring Framework, implemented with Ext JS 3 and Spring MVC 2.5.



  



  Ext JS File Upload Form



  First, we will need the Ext JS 4 File upload form. This one is the same as showed in Ext JS 4 docs.


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.onReady(function(){

Ext.create(&#8216;Ext.form.Panel&#8217;, {
          
title: &#8216;File Uploader&#8217;,
          
width: 400,
          
bodyPadding: 10,
          
frame: true,
          
renderTo: &#8216;fi-form&#8217;,
          
items: [{
              
xtype: &#8216;filefield&#8217;,
              
name: &#8216;file&#8217;,
              
fieldLabel: &#8216;File&#8217;,
              
labelWidth: 50,
              
msgTarget: &#8216;side&#8217;,
              
allowBlank: false,
              
anchor: &#8216;100%&#8217;,
              
buttonText: &#8216;Select a File&#8230;&#8217;
          
}],

buttons: [{
              
text: &#8216;Upload&#8217;,
              
handler: function() {
                  
var form = this.up(&#8216;form&#8217;).getForm();
                  
if(form.isValid()){
                      
form.submit({
                          
url: &#8216;upload.action&#8217;,
                          
waitMsg: &#8216;Uploading your file&#8230;&#8217;,
                          
success: function(fp, o) {
                              
Ext.Msg.alert(&#8216;Success&#8217;, &#8216;Your file has been uploaded.&#8217;);
                          
}
                      
});
                  
}
              
}
          
}]
      
});
  
});
  
[/code]


  HTML Page



  Then in the HTML page, we will have a div where we are going to render the Ext JS form. This page also contains the required javascript imports:


[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

  
Spring FileUpload Example with ExtJS 4 Form


	  

      



	  



  


Click on "Browse" button (image) to select a file and click on Upload button


  

  

  
[/code]


  FileUpload Bean



  We will also need a FileUpload Bean to represent the file as a multipart file:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

import org.springframework.web.multipart.commons.CommonsMultipartFile;

/**
   
* Represents file uploaded from extjs form
   
*
   
* @author Loiane Groner
   
* http://loiane.com
   
* http://loianegroner.com
   
*/
  
public class FileUploadBean {

private CommonsMultipartFile file;

public CommonsMultipartFile getFile() {
		  
return file;
	  
}

public void setFile(CommonsMultipartFile file) {
		  
this.file = file;
	  
}
  
}
  
[/code]


  File Upload Controller



  Then we will need a controller. This one is implemented with Spring MVC 3.


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.controller;

import org.springframework.stereotype.Controller;
  
import org.springframework.validation.BindingResult;
  
import org.springframework.validation.ObjectError;
  
import org.springframework.web.bind.annotation.RequestMapping;
  
import org.springframework.web.bind.annotation.RequestMethod;
  
import org.springframework.web.bind.annotation.ResponseBody;

import com.loiane.model.ExtJSFormResult;
  
import com.loiane.model.FileUploadBean;

/**
   
* Controller &#8211; Spring
   
*
   
* @author Loiane Groner
   
* http://loiane.com
   
* http://loianegroner.com
   
*/
  
@Controller
  
@RequestMapping(value = "/upload.action")
  
public class FileUploadController {

@RequestMapping(method = RequestMethod.POST)
	  
public @ResponseBody String create(FileUploadBean uploadItem, BindingResult result){

ExtJSFormResult extjsFormResult = new ExtJSFormResult();

if (result.hasErrors()){
			  
for(ObjectError error : result.getAllErrors()){
				  
System.err.println("Error: " + error.getCode() + " &#8211; " + error.getDefaultMessage());
			  
}

//set extjs return &#8211; error
			  
extjsFormResult.setSuccess(false);

return extjsFormResult.toString();
		  
}

// Some type of file processing&#8230;
		  
System.err.println("&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-");
		  
System.err.println("Test upload: " + uploadItem.getFile().getOriginalFilename());
		  
System.err.println("&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-");

//set extjs return &#8211; sucsess
		  
extjsFormResult.setSuccess(true);

return extjsFormResult.toString();

}
  
[/code]


  Ext JS Form Return



  Some people asked me how to return something to the form to display a message to the user. We can implement a POJO with a success property. The success property is the only thing Ext JS needs as a return:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.model;

/**
   
* A simple return message for Ext JS
   
*
   
* @author Loiane Groner
   
* http://loiane.com
   
* http://loianegroner.com
   
*/
  
public class ExtJSFormResult {

private boolean success;

public boolean isSuccess() {
		  
return success;
	  
}
	  
public void setSuccess(boolean success) {
		  
this.success = success;
	  
}

public String toString(){
		  
return "{success:"+this.success+"}";
	  
}
  
}
  
[/code]


  Spring Config



  Don&#8217;t forget to add the multipart file config in the Spring config file:


[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      

      

          

          

      

  
[/code]


  NullPointerException



  I also got some questions about NullPointerException. Make sure the fileupload field name has the same name as the CommonsMultipartFile property in the FileUploadBean class:



  ExtJS:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;3&#8243;]
  
{
	  
xtype: &#8216;filefield&#8217;,
	  
name: &#8216;file&#8217;,
	  
fieldLabel: &#8216;File&#8217;,
	  
labelWidth: 50,
	  
msgTarget: &#8216;side&#8217;,
	  
allowBlank: false,
	  
anchor: &#8216;100%&#8217;,
	  
buttonText: &#8216;Select a File&#8230;&#8217;
  
}
  
[/code]


  Java:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;3&#8243;]
  
public class FileUploadBean {

private CommonsMultipartFile file;
  
}
  
[/code]


  These properties ALWAYS have to match!



  You can still use the Spring MVC 2.5 code with the Ext JS 4 code presented in this tutorial.



  Download



  You can download the source code from my Github repository (you can clone the project or you can click on the download button on the upper right corner of the project page): https://github.com/loiane/extjs4-file-upload-spring



  You can also download the source code form the Google Code repository: http://code.google.com/p/extjs4-file-upload-spring/



  Both repositories have the same source. Google Code is just an alternative.



  Happy Coding! 
