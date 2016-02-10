---
layout: post
title: >
    Setting Up SSL on Tomcat in 5 minutes
    (https://localhost:8443)
published: true
author: Loiane
comments: true
date: 2011-06-30 07:06:37
tags:
    - https://localhost:8443
    - SSL
    - Tomcat
categories:
    - tomcat
permalink: >
    /2011/06/setting-up-ssl-on-tomcat-in-5-minutes-httpslocalhost8443
image:
    feature: apache-tomcat.png
---
This tutorial will walk you through how to configure **SSL** (**https://localhost:8443** access) on **Tomcat** in 5 minutes.

[][1]

For this tutorial you will need:

  * Java SDK (used version 6 for this tutorial)
  * Tomcat (used version 7 for this tutorial)

The set up consists in 3 basic steps:

  1. Create a **keystore** file using Java
  2. Configure Tomcat to use the keystore
  3. Test it
  4. (Bonus ) Configure your app to work with SSL (access through https://localhost:8443/yourApp)

## 1 &#8211; Creating a Keystore file using Java

Fisrt, open the terminal on your computer and type:

**Windows**:

[code lang=&#8221;bash&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
cd %JAVA_HOME%/bin
  
[/code]

**Linux or Mac OS**:

[code lang=&#8221;bash&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
cd $JAVA_HOME/bin
  
[/code]

The $JAVA_HOME on Mac is located on &#8220;**/System/Library/Frameworks/JavaVM.framework/Versions/{your java version}/Home/**&#8221;

You will change the current directory to the directory Java is installed on your computer. Inside the Java Home directory, cd to the bin folder. Inside the bin folder there is a file named keytool. This guy is responsible for generating the keystore file for us.

Next, type on the terminal:

[code lang=&#8221;bash&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
keytool -genkey -alias tomcat -keyalg RSA
  
[/code]

When you type the command above, it will ask you some questions. First, it will ask you to create a password (My password is &#8220;_password_&#8220;):

[code lang=&#8221;bash&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
loiane:bin loiane$ keytool -genkey -alias tomcat -keyalg RSA
  
Enter keystore password: password
  
Re-enter new password: password
  
What is your first and last name?
    
[Unknown]: Loiane Groner
  
What is the name of your organizational unit?
    
[Unknown]: home
  
What is the name of your organization?
    
[Unknown]: home
  
What is the name of your City or Locality?
    
[Unknown]: Sao Paulo
  
What is the name of your State or Province?
    
[Unknown]: SP
  
What is the two-letter country code for this unit?
    
[Unknown]: BR
  
Is CN=Loiane Groner, OU=home, O=home, L=Sao Paulo, ST=SP, C=BR correct?
    
[no]: yes

Enter key password for
	  
(RETURN if same as keystore password): password
  
Re-enter new password: password
  
[/code]

It will create a .keystore file on your user home directory. On Windows, it will be on: C:\Documents and Settings\[username]; on Mac it will be on /Users/[username] and on Linux will be on /home/[username].

## 2 &#8211; Configuring Tomcat for using the keystore file &#8211; SSL config

Open your Tomcat installation directory and open the **_conf_** folder. Inside this folder, you will find the _**server.xml**_ file. Open it.

Find the following declaration:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

  
&#8211;>
  
[/code]

Uncomment it and modify it to look like the following:

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221; highlight=&#8221;3,4,5&#8243;]
  
Connector SSLEnabled="true" acceptCount="100" clientAuth="false"
      
disableUploadTimeout="true" enableLookups="false" maxThreads="25"
      
port="8443" keystoreFile="/Users/loiane/.keystore" keystorePass="password"
      
protocol="org.apache.coyote.http11.Http11NioProtocol" scheme="https"
      
secure="true" sslProtocol="TLS" />
  
[/code]

Note we add the _**keystoreFile**_, _**keystorePass**_ and changed the _**protocol**_ declarations.

## 3 &#8211; Let&#8217;s test it!

Start tomcat service and try to access https://localhost:8443. You will see Tomcat&#8217;s local home page.

Note if you try to access the default 8080 port it will be working too: http://localhost:8080

## 4 &#8211; BONUS &#8211; Configuring your app to work with SSL (access through https://localhost:8443/yourApp)

To force your web application to work with SSL, you simply need to add the following code to your **web.xml** file (before _web-app_ tag ends):

[code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  

	  

		  
securedapp
		  
/*
	  

	  

		  
CONFIDENTIAL
	  

  

  
[/code]

The _**url pattern**_ is set to **/*** so any page/resource from your application is secure (it can be only accessed with **https**). The _**transport-guarantee**_ tag is set to **CONFIDENTIAL** to make sure your app will work on **SSL**.

If you want to turn off the SSL, you don&#8217;t need to delete the code above from web.xml, simply change **CONFIDENTIAL** to **NONE**.

_**Reference**_:  (this tutorial is a little confusing, that is why I decided to write another one my own).

Happy Coding!

 [1]: http://loianegroner.com/wp-content/uploads/2011/06/apache-tomcat.png