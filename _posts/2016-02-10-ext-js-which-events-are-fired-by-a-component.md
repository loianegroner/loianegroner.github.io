---
layout: post
title: 'Ext JS: Which events are fired by a component?'
published: true
author: Loiane
comments: true
date: 2009-11-27 07:11:21
tags:
    - events ExtJS
    - ExtJS
categories:
    - extjs
permalink: /2009/11/ext-js-which-events-are-fired-by-a-component
---
### **Quick Tip:**

Sometimes, we need to know all events that are fired by an Ext JS component.

It is easy to find out, just use the following code in Firebug:

[code lang=&#8221;js&#8221; toolbar=&#8221;true&#8221;]Ext.util.Observable.capture(Ext.getCmp(&#8216;id\_my\_grid&#8217;), console.info);[/code]

For example: let&#8217;s say that the component you want to analyze has id &#8220;id\_my\_grid&#8221;. Load the page with the component (project) using Firefox, open Firebug, Console tab and use the code above.

Now do some actions with the component and watch the Firebug output.

Happy coding and debuging!