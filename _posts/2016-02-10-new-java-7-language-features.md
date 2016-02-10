---
layout: post
title: New Java 7 Language Features
published: true
author: Loiane
comments: true
date: 2011-07-11 07:07:53
tags:
    - Java
    - Java 7
    - NetBeans 7
    - Project Coin
categories:
    - java
permalink: /2011/07/new-java-7-language-features
image:
    feature: duke_java7.jpg
---

  Last week I was at the Java 7 launch party in Sao Paulo, Brazil and it was really cool. There were a lot of developers on the party to celebrate Java 7 (afterall, we have been waiting for it for 5 years!).



  I took some pictures at the event: http://www.flickr.com/photos/loiane/sets/72157627015952961/



  Video of the event: http://blogs.oracle.com/java/entry/java_7_celebration_begins



  Slides: http://blogs.oracle.com/darcy/resource/ProjectCoin/CoinLaunch.pdf



  Anyways, I was really excited with the event and I went home and started to play with Java 7 language features.



  As I will have to wait for Open JDK for Mac to be released, I had to play with it on Windows. You can download Java 7 here.



  Another issue I found is Eclipse does not have native support for Java 7. They released a plugin, but it still beta. So I decided to play with NetBeans.



  The current version of NetBeans is 7, and it already comes with native support to Java 7. If you install Java 7 and then install NetBeans, it will set Java 7 as the default JDK for development.



  Then I created a Java Project on NetBeans but the source code was still compiling in Java 6. You have to make a small change to make it compile with Java 7.



  All set and I started to play with some languages new features, aka Project Coin.



  Strings in Switch:



  In all examples, I tried first to write code as we usually do, and then convert the code to a Java 7 feature. The cool thing about NetBeans is it already have tips to convert the code.



  First, will compare some Strings using if-else. NetBeans will popup a warning like this:



  



  And if we accept the change, it will convert the code into this:



  



  Following is the code:


Before:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
   
public void testStringInSwitch(String param){

final String JAVA5 = "Java 5";
          
final String JAVA6 = "Java 6";
          
final String JAVA7 = "Java 7";

if (param.equals(JAVA5)){
              
System.out.println(JAVA5);
          
} else if (param.equals(JAVA6)){
              
System.out.println(JAVA6);
          
} else if (param.equals(JAVA7)){
              
System.out.println(JAVA7);
          
}
      
}
  
[/code]

Java7:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
   
public void testStringInSwitch(String param){

final String JAVA5 = "Java 5";
          
final String JAVA6 = "Java 6";
          
final String JAVA7 = "Java 7";

switch (param) {
              
case JAVA5:
                  
System.out.println(JAVA5);
                  
break;
              
case JAVA6:
                  
System.out.println(JAVA6);
                  
break;
              
case JAVA7:
                  
System.out.println(JAVA7);
                  
break;
          
}
      
}
  
[/code]


  Binary Literals:


Before:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public void testBinaryIntegralLiterals(){

int binary = 8;

if (binary == 8){
              
System.out.println(true);
          
} else{
              
System.out.println(false);
          
}
  
}
  
[/code]

Java7:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
   
public void testBinaryIntegralLiterals(){

int binary = 0b1000; //2^3 = 8

if (binary == 8){
              
System.out.println(true);
          
} else{
              
System.out.println(false);
          
}
  
}
  
[/code]


  And if you try to declare a constant number in the code, NetBeans will warn you to convert it into a Binary Literal:



  



  to



  


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public void testUnderscoresNumericLiterals() {

int oneMillion_ = 1\_000\_000;
          
int oneMillion = 1000000;

if (oneMillion_ == oneMillion){
              
System.out.println(true);
          
} else{
              
System.out.println(false);
          
}
      
}
  
[/code]


  Underscore Between Literals:


[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public void testUnderscoresNumericLiterals() {

int oneMillion_ = 1\_000\_000; //new
      
int oneMillion = 1000000;

if (oneMillion_ == oneMillion){
		  
System.out.println(true);
      
} else{
          
System.out.println(false);
      
}
  
}
  
[/code]


  Diamond Syntax:


Before:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public void testDinamond(){

List list = new ArrayList();
      
Map map = new HashMap();
  
}
  
[/code]

Java7:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
public void testDinamond(){
      
List list = new ArrayList();
      
Map map = new HashMap();
  
}
  
[/code]


  When we write code in Java 6 syntax, NetBeans will warn us to convert it into Java 7 Diamond Syntax:



  



  To



  



  Multi-Catch Similar Exceptions:


Before:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
public void testMultiCatch(){

try {
              
throw new FileNotFoundException("FileNotFoundException");
          
} catch (FileNotFoundException fnfo) {
              
fnfo.printStackTrace();
          
} catch (IOException ioe) {
              
ioe.printStackTrace();
     
}
  
[/code]

Java 7:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
public void testMultiCatch(){

try {
              
throw new FileNotFoundException("FileNotFoundException");
          
} catch (FileNotFoundException | IOException fnfo) {
              
fnfo.printStackTrace();
          
}
      
}
  
[/code]


  Same thing here. NetBeans will ask to convert the code into Java 7:



  



  To



  



  Try with Resources:


Before:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
public void testTryWithResourcesStatement() throws FileNotFoundException, IOException{

FileInputStream in = null;
          
try {
              
in = new FileInputStream("java7.txt");

System.out.println(in.read());

} finally {
              
if (in != null) {
                  
in.close();
              
}
          
}

}
  
[/code]

Java 7:

[code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
      
public void testTryWithResourcesStatement() throws FileNotFoundException, IOException{

try (FileInputStream in = new FileInputStream("java7.txt")) {

System.out.println(in.read());

}
      
}
  
[/code]


  Same thing here:



  



  To:



  



  There is also varargs simplification, but I did not understand this one very well to code an example.



  There are another features that were submited, but did not make it into this release:


  * Elvis Operator
  * List and Maps Index
  * Collection Literals
  * Null-Safe navigation


  The code presented on this post is not only theory, I really ran it in my machine. You can get/download the NetBeans project from my github: https://github.com/loiane/Java7HelloWorld



  A quick update: A DZone released today a RefCardz about NetBeans and Java 7 &#8211; it is a pdf and the download is free: http://refcardz.dzone.com/refcardz/netbeans-ide-7-programming
