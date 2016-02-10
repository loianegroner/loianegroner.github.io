---
layout: post
title: Ajax File Upload with ExtJS and Spring Framework
published: true
author: Loiane
comments: true
date: 2010-03-01 08:03:31
tags:
    - ExtJS
    - ExtJS + J2EE
    - file upload
    - Spring
categories:
    - extjs
    - spring
permalink: /2010/03/ajax-file-upload-with-extjs-and-spring-framework
---

  


This tutorial will walk you through how to implement an ajax file upload form using ExtJS on client side and Spring Framework on server side.

What are you going to need before start this tutorial?

  * ExtJS
  * Spring Framework (MVC) and its dependencies
  * commons-io-1.4.jar
  * commons-fileupload-1.2.jar

First, you need to implement the ExtJS File Upload form. You can use ExtJS File Upload example page to see how it looks like (I&#8217;m using third example).
  
Bellow is the form I implemented (or copied from ExtJS and adapted it to my needs &#8211; spring):

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.onReady(function(){

Ext.QuickTips.init();

var msg = function(title, msg){
          
Ext.Msg.show({
              
title: title,
              
msg: msg,
              
minWidth: 200,
              
modal: true,
              
icon: Ext.Msg.INFO,
              
buttons: Ext.Msg.OK
          
});
      
};

var fp = new Ext.FormPanel({
          
renderTo: &#8216;fi-form&#8217;,
          
fileUpload: true,
          
width: 500,
          
frame: true,
          
title: &#8216;File Upload Form&#8217;,
          
autoHeight: true,
          
bodyStyle: &#8216;padding: 10px 10px 0 10px;&#8217;,
          
labelWidth: 50,
          
defaults: {
              
anchor: &#8216;95%&#8217;,
              
allowBlank: false,
              
msgTarget: &#8216;side&#8217;
          
},
          
items: [{
              
xtype: &#8216;fileuploadfield&#8217;,
              
id: &#8216;form-file&#8217;,
              
emptyText: &#8216;Select a File to import&#8217;,
              
fieldLabel: &#8216;File&#8217;,
              
name: &#8216;file&#8217;,
              
buttonCfg: {
                  
text: &#8221;,
                  
iconCls: &#8216;upload-icon&#8217;
              
}
          
}],
          
buttons: [{
              
text: &#8216;Upload&#8217;,
              
handler: function(){
                  
if(fp.getForm().isValid()){
	                  
fp.getForm().submit({
	                      
url: &#8216;fileUpload/import.action&#8217;,
	                      
waitMsg: &#8216;Uploading your file&#8230;&#8217;,
	                      
success: function(fp, o){
	                          
msg(&#8216;Success&#8217;, &#8216;Processed file on the server&#8217;);
	                      
}
	                  
});
                  
}
              
}
          
},{
              
text: &#8216;Reset&#8217;,
              
handler: function(){
                  
fp.getForm().reset();
              
}
          
}]
      
});

});
  
[/code]

And here is an example of how to use it (html page):

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

  
Spring FileUpload Example with ExtJS Form


	  



      



          
.upload-icon {
              
background: url(&#8216;/extjs-file-import-spring/ext-3.1.1/examples/shared/icons/fam/image_add.png&#8217;) no-repeat 0 0 !important;
          
}
      



	  





	  



  


Spring File Upload Example Integrated with ExtJS FileUpload Form
	  
Click on "Browse" button (image) to select a file and click on Upload button
	  



  

  
[/code]

Before starting to code a FileUploadController, you are going to need a FileUploadBean:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.beans;

import org.springframework.web.multipart.MultipartFile;

public class FileUploadBean {

private MultipartFile file;

public MultipartFile getFile() {
		  
return file;
	  
}

public void setFile(MultipartFile file) {
		  
this.file = file;
	  
}

}
  
[/code]

And here is my FileUploadController (I am simply storing the uploaded file in a directory in my hard drive &#8211; C:). You can add some validation or process the file in this class. You can also add some message to return to client side in case of success or failure.

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.web;

import java.io.FileOutputStream;
  
import java.io.IOException;
  
import java.io.InputStream;
  
import java.io.OutputStream;

import javax.servlet.ServletException;
  
import javax.servlet.http.HttpServletRequest;
  
import javax.servlet.http.HttpServletResponse;

import org.springframework.validation.BindException;
  
import org.springframework.web.bind.ServletRequestDataBinder;
  
import org.springframework.web.multipart.MultipartFile;
  
import org.springframework.web.multipart.support.ByteArrayMultipartFileEditor;
  
import org.springframework.web.servlet.ModelAndView;
  
import org.springframework.web.servlet.mvc.SimpleFormController;

import com.loiane.beans.FileUploadBean;

public class FileUploadController extends SimpleFormController {

protected ModelAndView onSubmit(
			  
HttpServletRequest request,
			  
HttpServletResponse response,
			  
Object command,
			  
BindException errors) throws ServletException, IOException {

// cast the bean
		  
FileUploadBean bean = (FileUploadBean) command;

MultipartFile file = bean.getFile();
		  
String fileName = null;

if (file == null) {
			  
System.out.println("User Did not upload file");
		  
}
		  
else {
			  
System.out.println("Uploaded File Name is :" + file.getOriginalFilename());
		  
}

InputStream inputStream = null;
		  
OutputStream outputStream = null;
		  
if (file.getSize() > 0) {
			  
inputStream = file.getInputStream();
			  
String root = "C:\\";
			  
fileName = root + file.getOriginalFilename();
			  
outputStream = new FileOutputStream(fileName);
			  
int readBytes = 0;
			  
byte[] buffer = new byte[10000];
			  
while ((readBytes = inputStream.read(buffer, 0 , 10000))!=-1){

outputStream.write(buffer, 0, readBytes);
			  
}
			  
outputStream.close();
			  
inputStream.close();
		  
}

return null;

} 

protected void initBinder(HttpServletRequest request, ServletRequestDataBinder binder)
	  
throws ServletException {
		  
// to actually be able to convert Multipart instance to byte[]
		  
// we have to register a custom editor
		  
binder.registerCustomEditor(byte[].class, new ByteArrayMultipartFileEditor());
		  
// now Spring knows how to handle multipart object and convert them
	  
}

}
  
[/code]

You also need to add this in your servelt.xml:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
	  

       

     	  

       

  
[/code]

Very simple!

Here is the source code of my example project: http://github.com/loiane/extjs-file-import-spring

Happy coding!