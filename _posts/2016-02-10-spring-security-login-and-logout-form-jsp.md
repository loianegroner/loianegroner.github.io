---
layout: post
title: 'Spring Security: Login and Logout Form JSP'
published: true
author: Loiane
comments: true
date: 2010-01-25 08:01:52
tags:
    - login
    - logout
    - Spring
    - Spring Security
categories:
    - spring
    - spring-security
permalink: /2010/01/spring-security-login-and-logout-form-jsp
---

  



  When you configure spring security in your web application you can configure your login.jsp in the applicationContext-security.xml.



  But how this page looks like? If you search around, you are not going to find it easily. There is many articles about how to configure spring security, but a few ones list login.jsp.



  If you take a look in the spring security distribution file, you are going to find an example application, and inside it: login.jsp and logout link.


### login.jsp

[code lang=&#8221;html&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  

  

  



    

      
Login
    



      
Login


        

          
Your login attempt was not successful, try again.
          
Reason: .
        

      



        

          
User: