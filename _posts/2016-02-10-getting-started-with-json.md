---
layout: post
title: Getting Started with JSON
published: true
author: Loiane
comments: true
date: 2009-12-14 08:12:45
tags:
    - javascript
    - JSON
categories:
    - json
permalink: /2009/12/getting-started-with-json
---
This post will walk through JSON basic concepts.



### What is JSON?

JSON (JavaScript Object Notation) is a lightweight data-interchange format, widely hailed as the successor to XML in the browser.

It is easy for humans to read and write.

It is easy for machines to parse and generate.

It is based on a subset of the JavaScript Programming Language.

It is lighter than XML.

### JSON versus XML

Ps.: This is only from the javascript eval() point of view. 


  
    
      JSON
    
    
    
      XML
    
  
  
  
    
      JSON object are type
    
    
    
      XML data is typeless
    
  
  
  
    
      JSON types: string, number, array, boolean
    
    
    
      XML data are all string
    
  
  
  
    
      Data is readily accessible as JSON objects
    
    
    
      XML data needs to be parsed
    
  
  
  
    
      Retrieving value is easy
    
    
    
      Retrieving value is difficult
    
  
  
  
    
      JSON is supported by all browsers
    
    
    
      Cross browser XML parsing can be tricky
    
  
  
  
    
      Simple API
    
    
    
      Complex API
    
  
  
  
    
      Supported by many Ajax toolkit
    
    
    
      Not fully supported by Ajax toolkit
    
  
  
  
    
      Fast object de-serialization in JavaScript
    
    
    
      Slower de-serialization in Javascript
    
  
  
  
    
      Fully automated way of de-serializing/serializing JavaScript objects
    
    
    
      Developers have to write javascript code to serialize/se-serialize to/from XML
    
  


### JSON Values

JSON is built on two structures:

  * A collection of name/value pairs. In various languages, this is realized as an _object_, record, struct, dictionary, hash table, keyed list, or associative array.
  * An ordered list of values. In most languages, this is realized as an _array_, vector, list, or sequence.

An _object_ is an unordered set of name/value pairs. An object begins with { (left brace) and ends with } (right brace). Each name is followed by : (colon) and the name/value pairs are separated by , (comma).


  An array is an ordered collection of values. An array begins with [ (left bracket) and ends with ] (right bracket). Values are separated by , (comma).



  A value can be a string in double quotes, or a number, or true or false or null, or an object or an array. These structures can be nested.



  



  A string is a collection of zero or more Unicode characters, wrapped in double quotes, using backslash escapes. A character is represented as a single character string. A string is very much like a C or Java string.



  



  A number is very much like a C or Java number, except that the octal and hexadecimal formats are not used.



  


### JSON Syntax

  * For objects start the object with “{“ and end it with “}”
  * For members (properties), use pairs of string : value and separate them by commas
  * For arrays put the arrays between []
  * For elements put the values directly separated by commas

Example:

[code lang=&#8221;js&#8221;]
  

  
var myFirstJSON = { "firstName" : "Loiane",
                      
"lastName" : "Groner",
                      
"age" : 23 };

document.writeln(myFirstJSON.firstName); // Outputs Loiane
  
document.writeln(myFirstJSON.lastName); // Outputs Groner
  
document.writeln(myFirstJSON.age); // Outputs 23
  

  
[/code]

This object has 3 properties or name/value pairs. The firstName and lastName are strings and age is a number.

The value can be any Javascript object (and remember everything in Javascript is an object so the value can be a string, number, array, function, even other Objects).

[code lang=&#8221;js&#8221;]
  

  
var hobbies = { "tvseries" :
			  
[ //array
				  
{"name": "Lost", "seasons":5 },
				  
{"name": "Heroes", "seasons":4 },
				  
{"name": "Fringe", "seasons":2 },
				  
{"name": "The Office", "seasons":5 },
				  
{"name": "The Big Bang Theory", "seasons":3 },
				  
{"name": "True Blood", "seasons":2 },
				  
{"name": "Battlestar Galactica", "seasons":4 },
				  
{"name": "Sex and the City", "seasons":6 },
				  
{"name": "Friends", "seasons":10 }
			  
]
			  
"games" :
				  
[
				  
"Counter Strike" ,"Sacred 2","The Sims 3","Guitar Hero"
				  
]

};

document.writeln(hobbies.tvseries[0].name); // Outputs Lost
  
document.writeln(hobbies.games[3]); // Outputs Guitar Hero
  

  
[/code]

_hobbies_ is an object. That object has two properties or name/value pair (_tvseries_ and _games_).

_tvseries_ is an array which holds two JSON objects showing the names and seasons of 9 TV Series. Likewise games is also an array which holds four JSON objects showing the names of games I like.

Well, this is just a start. You can study JSON functions and how to convert JSON objects now. You can also study Ajax toolkits taht support JSON.

Happy coding!