---
layout: post
title: Ext Designer Review
published: true
author: Loiane
comments: true
date: 2010-04-06 08:04:35
tags:
    - Ext Designer
    - ExtJS
    - review
categories:
    - extjs
permalink: /2010/04/ext-designer-review
---

  A couple of weeks ago, I decided to download Ext Designer and play around with it. In this post, I am going to write a review about this application.



  Disclaimer: I am not being paid to do this and no one asked me to. Everything I say/write here is my honest opinion.



  
    Designer is a desktop application that helps you create interfaces faster than ever in an easy-to-use, drag-and-drop environment.
  



  The ExtJS Team created a Preview screencast in which they mock up some interfaces.



  It really is cool. There is a post on ExtJS Blog you can check out all the features of this great tool.



  Despite being a great tool with all those amazing features, there are some features and ExtJS components I missed in the application.



  First, I created a simple ExtJS application to illustrate what I mean.



  



  It generates the code for each component you create:



  Personal Info – Field Set:



  



  Name Text Field:



  



  If you take a look, you will see the name text field within the personal info field set is declared in a different way.



  If you want to export the whole code to your favorite IDE, so you can integrate ExtJS with a programming language or platform (e.g.: PHP, Java, .NET), you just need to select the root component and click on &#8220;Export Code to Disk&#8221; or &#8220;Export code to ClipBoard&#8221;:


[][1]


  Look now at the code generated from the project I created – it is the same code you get when you choose the export function:


[code lang=&#8221;js&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]
  
MyWindowUi = Ext.extend(Ext.Window, {
      
title: &#8216;Ext Designer Test&#8217;,
      
width: 400,
      
height: 250,
      
initComponent: function() {
          
this.items = [
              
{
                  
xtype: &#8216;tabpanel&#8217;,
                  
activeTab: 1,
                  
items: [
                      
{
                          
xtype: &#8216;panel&#8217;,
                          
title: &#8216;Grid&#8217;,
                          
items: [
                              
{
                                  
xtype: &#8216;grid&#8217;,
                                  
title: &#8216;My Grid&#8217;,
                                  
columns: [
                                      
{
                                          
xtype: &#8216;gridcolumn&#8217;,
                                          
header: &#8216;Name&#8217;,
                                          
sortable: true,
                                          
resizable: true,
                                          
width: 100,
                                          
dataIndex: &#8216;name&#8217;
                                      
},
                                      
{
                                          
xtype: &#8216;gridcolumn&#8217;,
                                          
header: &#8216;Phone #&#8217;,
                                          
sortable: true,
                                          
resizable: true,
                                          
width: 100,
                                          
dataIndex: &#8216;phone&#8217;
                                      
},
                                      
{
                                          
xtype: &#8216;gridcolumn&#8217;,
                                          
header: &#8216;Address&#8217;,
                                          
sortable: true,
                                          
resizable: true,
                                          
width: 100,
                                          
dataIndex: &#8216;address&#8217;
                                      
}
                                  
],
                                  
bbar: {
                                      
xtype: &#8216;paging&#8217;
                                  
}
                              
}
                          
]
                      
},
                      
{
                          
xtype: &#8216;panel&#8217;,
                          
title: &#8216;Form&#8217;,
                          
items: [
                              
{
                                  
xtype: &#8216;fieldset&#8217;,
                                  
title: &#8216;Personal Info&#8217;,
                                  
layout: &#8216;form&#8217;,
                                  
items: [
                                      
{
                                          
xtype: &#8216;textfield&#8217;,
                                          
fieldLabel: &#8216;Name&#8217;,
                                          
anchor: &#8216;100%&#8217;,
                                          
name: &#8216;nameField&#8217;
                                      
},
                                      
{
                                          
xtype: &#8216;textarea&#8217;,
                                          
fieldLabel: &#8216;Address&#8217;,
                                          
anchor: &#8216;100%&#8217;,
                                          
name: &#8216;addressField&#8217;
                                      
},
                                      
{
                                          
xtype: &#8216;textfield&#8217;,
                                          
fieldLabel: &#8216;Phone #&#8217;,
                                          
anchor: &#8216;100%&#8217;,
                                          
name: &#8216;phoneField&#8217;
                                      
}
                                  
]
                              
},
                              
{
                                  
xtype: &#8216;button&#8217;,
                                  
text: &#8216;Save&#8217;
                              
}
                          
]
                      
},
                      
{
                          
xtype: &#8216;panel&#8217;,
                          
title: &#8216;Tree&#8217;,
                          
items: [
                              
{
                                  
xtype: &#8216;treepanel&#8217;,
                                  
title: &#8216;My Tree&#8217;,
                                  
root: {
                                      
text: &#8216;Root Node&#8217;
                                  
},
                                  
loader: {

}
                              
}
                          
]
                      
}
                  
]
              
}
          
];
          
MyWindowUi.superclass.initComponent.call(this);
      
}
  
});
  
[/code]


  It generated a single code block! Lucky me it is a small and simple project! Imagine the code generated from a big project? Not friendly if you need to maintain this project in the future.



  This is the first thing I think they could improve in Ext Designer. If the tool creates a class for each component already, why do not use the reference to it to generate the whole code? And they could export all the classes in different files. I think this is the way most of ExtJS developers does when they have to develop a big project.



  The second thing I did not like is that the code is read only. I am a developer, and sometimes I just want to set a property in the code. It may be faster than searching the property in the Component Config tab, after all, I am used to set the properties in the code already. I understand this can be sort of a protection if a person does not know ExtJS and he/she can mess up with the code trying to change anything.



  Third: I missed some components, such as radio group and graphs, among others. Maybe this is something they can implement for a near future?



  The coolest feature in my opinion is the preview function. It is really awesome!



  In general, Ext Designer is a great tool and I love it! It is very intuitive.



  I do not recommend it if you are learning ExtJS. I think it is important to understand how ExtJS works and the components patterns before you get started with a visual tool. However, it is great if you are an ExtJS developer already. It can save you a lot of time. It is also great to prototype and design applications (if there is a design team in your company that do not know ExtJS programming).



  I definitely recommend buying it.



  You can download the full version (with all the features) and try it for 15 days for free (all you need to do is to register as a user on the ExtJS forum). After the evaluation period, you have to get a license.



  Happy coding!



  Updated: ExtJS quoted this review on ExtJS Notes (Thank You!) &#8211; http://notes.extjs.com/post/501611295/ext-designer-is-a-great-tool


 [1]: http://loianegroner.com/wp-content/uploads/2010/04/Test_Ext_Designer_Loiane_04.gif