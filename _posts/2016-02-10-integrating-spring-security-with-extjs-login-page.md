---
layout: post
title: Integrating Spring Security with ExtJS Login Page
published: true
author: Loiane
comments: true
date: 2010-02-01 08:02:28
tags:
    - ExtJS
    - ExtJS + J2EE
    - ExtJS Login Form
    - Spring
    - Spring Security
    - Spring Security Login
categories:
    - extjs
    - spring
    - spring-security
permalink: /2010/02/integrating-spring-security-with-extjs-login-page
---

  


This tutorial will walk through how to configure ExtJS Login form (Ajax login form) instead of default Spring Security login.jsp.

Instead of using login.jsp from spring security, why do not use an ajax login form?

And How to integrate the ExtJS Login Form with Spring Security?

You did try to do it, the user is successfully authenticated, but the user is not redirected to the application main page. How to fix this situation? How to make it work?

It does not matter if you set the default-target-url in applicationContext-security.xml, or set a redirect URL on server side. It will not work this way.

The issue is that ExtJS make Ajax calls, and no redirect will work on server side. You have to redirect it on the client side, which is the ExtJS/javascript code.

First, you need to create the login form. You can use the javascript code provided by ExtJS and you can modify it to work with spring security.

If you take a look at the login.jsp, you will see three key points:

  1. URL / form action: **j\_spring\_security_check**
  2. Username input name: **j_username**
  3. Password input name: **j_password**

That is what you need to customize to make ExtJS Login form works! But do not be too comfortable, there are some issues you need to fix to make it work perfectly.

Take a look how login.js looks like (after customization):

[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
Ext.onReady(function(){
	  
Ext.QuickTips.init();

// Create a variable to hold our EXT Form Panel.

// Assign various config options as seen.
	  
var login = new Ext.FormPanel({
		  
labelWidth:80,
		  
url:&#8217;j\_spring\_security_check&#8217;,
		  
frame:true,
		  
title:&#8217;Please Login&#8217;,

defaultType:&#8217;textfield&#8217;,
		  
width:300,
		  
height:150,
		  
monitorValid:true,
		  
// Specific attributes for the text fields for username / password.
		  
// The "name" attribute defines the name of variables sent to the server.

items:[{
			  
fieldLabel:&#8217;Username&#8217;,
			  
name:&#8217;j_username&#8217;,
			  
allowBlank:false
		  
},{
			  
fieldLabel:&#8217;Password&#8217;,

name:&#8217;j_password&#8217;,
			  
inputType:&#8217;password&#8217;,
			  
allowBlank:false
		  
}],

// All the magic happens after the user clicks the button
		  
buttons:[{

text:&#8217;Login&#8217;,
			  
formBind: true,
			  
// Function that fires when user clicks the button
			  
handler:function(){
			  
login.getForm().submit({

method:&#8217;POST&#8217;,

// Functions that fire (success or failure) when the server responds.
				  
// The server would actually respond with valid JSON,
				  
// something like: response.write "{ success: true}" or

// response.write "{ success: false, errors: { reason: &#8216;Login failed. Try again.&#8217; }}"
				  
// depending on the logic contained within your server script.
				  
// If a success occurs, the user is notified with an alert messagebox,

// and when they click "OK", they are redirected to whatever page
				  
// you define as redirect.

success:function(){
				  
Ext.Msg.alert(&#8216;Status&#8217;, &#8216;Login Successful!&#8217;, function(btn, text){

if (btn == &#8216;ok&#8217;){
						  
window.location = &#8216;main.action&#8217;;
					  
}
				  
});

},

// Failure function, see comment above re: success and failure.
			  
// You can see here, if login fails, it throws a messagebox
			  
// at the user telling him / her as much.

failure:function(form, action){
				  
if(action.failureType == &#8216;server&#8217;){
					  
obj = Ext.util.JSON.decode(action.response.responseText);

Ext.Msg.alert(&#8216;Login Failed!&#8217;, obj.errors.reason);
				  
}else{
					  
Ext.Msg.alert(&#8216;Warning!&#8217;, &#8216;Authentication server is unreachable : &#8216; + action.response.responseText);

}
				  
login.getForm().reset();
			  
}

});
		  
}
		  
}]
	  
});

login.render(&#8216;login&#8217;);

});

[/code]

If you make these changes and try to execute the application with a basic **applicationContext-security.xml** file, the user will be successfully authenticated, but is not going to be redirected.

What are we missing then?

You need to customize AuthenticationProcessingFilter class for spring security to perform actions on login.

The “onSuccessfulAuthentication” and “onUnsuccessfulAuthentication” methods need to return some JSON content. If user is successfully authenticated, then redirect to main page, otherwise, the application will show an error message.

This is **MyAuthenticationProcessingFilter** class:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
package com.loiane.security;

import java.io.IOException;
  
import java.io.Writer;

import javax.servlet.http.HttpServletRequest;
  
import javax.servlet.http.HttpServletResponse;
  
import javax.servlet.http.HttpServletResponseWrapper;

import org.springframework.security.Authentication;
  
import org.springframework.security.AuthenticationException;
  
import org.springframework.security.ui.webapp.AuthenticationProcessingFilter;

public class MyAuthenticationProcessingFilter extends AuthenticationProcessingFilter {

protected void onSuccessfulAuthentication(HttpServletRequest request,
			  
HttpServletResponse response, Authentication authResult)
	  
throws IOException {
		  
super.onSuccessfulAuthentication(request, response, authResult);

HttpServletResponseWrapper responseWrapper = new HttpServletResponseWrapper(response);

Writer out = responseWrapper.getWriter();

String targetUrl = determineTargetUrl( request );
		  
out.write("{success:true, targetUrl : \&#8217;" + targetUrl + "\&#8217;}");
		  
out.close();

}

protected void onUnsuccessfulAuthentication( HttpServletRequest request,
			  
HttpServletResponse response, AuthenticationException failed )
	  
throws IOException {

HttpServletResponseWrapper responseWrapper = new HttpServletResponseWrapper(response);

Writer out = responseWrapper.getWriter();

out.write("{ success: false, errors: { reason: &#8216;Login failed. Try again.&#8217; }}");
		  
out.close();

}

}

[/code]

And this is how **applicationContext-security.xml **looks like**:**

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  







		  

		  

	  



		  

		  

		  

	  





		  

		  

	  



      

          

          

              

              

              

              

	      

	  

  

  
[/code]

Now you can login using ExtJS login form.

I coded a sample application for this example. If you like it, you can download it from my GitHub: http://github.com/loiane/spring-security-extjs-login

Happy coding!