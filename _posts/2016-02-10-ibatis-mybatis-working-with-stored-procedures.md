---
layout: post
title: 'IBatis (MyBatis): Working with Stored Procedures'
published: true
author: Loiane
comments: true
date: 2011-03-29 08:03:39
tags:
    - iBatis
    - Java
    - MyBatis
    - mySQL
    - stored procedure
categories:
    - ibatis-mybatis
permalink: /2011/03/ibatis-mybatis-working-with-stored-procedures
image:
    feature: stored_procedure_loiane.png
---

  
    This tutorial will walk you through how to setup iBatis (MyBatis) in a simple Java project and will present how to work with stored procedures using MySQL.
  
  
  
    The goal os this tutorial is to demonstrate how to execute/call stored procedures using iBatis/MyBatis.
  
  
  
    
  
  
  
    
  
  
  
    Pre-Requisites
  
  
  
    For this tutorial I am using:
  
  
  
    IDE: Eclipse (you can use your favorite one) DataBase: MySQL Libs/jars: Mybatis, MySQL conector and JUnit (for testing)
  
  
  
    This is how your project should look like:
  
  
  
    
  
  
  
    
  
  
  
    Sample Database
  
  
  
    Please run the script into your database before getting started with the project implementation. You will find the script (with dummy data) inside the sql folder.
  
  
  
    
  
  
  
    As we are going to work with stored procedures, you will also have to execute a script with procedures. Here are the procedures:
  
  
  
    [code lang=&#8221;sql&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;] USE `blog_ibatis`; DROP procedure IF EXISTS `getTotalCity`; DELIMITER $$ USE `blog_ibatis`$$ CREATE PROCEDURE `blog_ibatis`.`getTotalCity` (OUT total INTEGER) BEGIN SELECT count(*) into total FROM city; END $$ DELIMITER ;
  
  
  
    &#8212; &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;
  
  
  
    USE `blog_ibatis`; DROP procedure IF EXISTS `getTotalCityStateId`; DELIMITER $$ USE `blog_ibatis`$$ CREATE PROCEDURE `blog_ibatis`.`getTotalCityStateId` (IN stateId SMALLINT, OUT total INTEGER) BEGIN SELECT count(*) into total FROM city WHERE state_id = stateId; END $$ DELIMITER ;
  
  
  
    &#8212; &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;
  
  
  
    USE `blog_ibatis`; DROP procedure IF EXISTS `getStates`; DELIMITER $$ USE `blog_ibatis`$$ CREATE PROCEDURE `blog_ibatis`.`getStates` () BEGIN SELECT state_id, state_code, state_name FROM state; END $$ DELIMITER ; [/code]
  
  
  
    1 &#8211; SPMapper &#8211; XML
  
  
  
    I did not find anything on the user manual about how to call stored procedures, so I decided to search on the mailing list. And I found some tips of how to call stores procedures.
  
  
  
    On the previous version, iBatis has a special XML tag for stored procedures. But there is no XML tag for it on current MyBatis version (version 3).
  
  
  
    To call a stored procedure usgin MyBatis/iBatis 3 you will have to follow some tips:
  
  
  
    
      Must set the statement type to CALLABLE 
    
    
      Must use the JDBC standard escape sequence for stored procedures:  {call xxx (parm1, parm2)}
    
    
      Must set the MODE of all parameters (IN, OUT, INOUT)
    
    
      All IN, OUT, and INOUT parameters must be a part of the parameterType or parameterMap (discouraged).  The only exception is if you are using a Map  as a parameter object. In that case you do not need to add OUT parameters  to the map before calling, MyBatis will add them for you automatically.
    
    
      resultType or resultMap (more typically) is only used if the procedure returns  a result set.
    
    
      IMPORTANT: Oracle ref cursors are usually returned as parameters,  NOT directly from the stored proc. So with ref cursors, resultMap and/or  resultType is usually not used.
    
  
  
  
    First Example:
  
  
  
    We want to call the procedure getTotalCity and this procedure only have one OUT parameter, and no IN/INOUT parameter. How to do it?
  
  
  
    We are going to ser inline parameters in this first example. To use inline parameters, create a POJO class to represent your parameters, set the parameterType to the class you created and you are going to use this notation to represent each parameter:
  
  
  
    #{parameterName, mode=OUT, jdbcType=INTEGER}
  
  
  
    
      mode can be IN, OUT, INOUT
    
    
      and specify the jdbcType of your parameter
    
  
  
  
    To create the Mybatis XML configuration, you can use the select ou update tag. Do not forget to set the statementType to CALLABLE.
  
  
  
    Here is how our MyBatis statement is going to look like:
  
  
  
    [code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]  { CALL getTotalCity(#{total, mode=OUT, jdbcType=INTEGER})}  [/code]
  
  
  
    And this is the POJO class which represents the parameter for getTotalCity procedure:
  
  
  
    [code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;] package com.loiane.model;
  
  
  
    public class Param {
  
  
  
    private int total;
  
  
  
    public int getTotal() { return total; }
  
  
  
    public void setTotal(int total) { this.total = total; } } [/code]
  
  
  
    Second Example:
  
  
  
    Now we are going to try to call the same stored procedure we demonstrated on the first example, but we are going to use a parameterMap, like you used to do in version 2.x.
  
  
  
    A very important note: this is discouraged, please use inline parameters.
  
  
  
    Let&#8217;s declare the Param POJO class as a parameterMap:
  
  
  
    [code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]    [/code]
  
  
  
    And the stored procedure statment:
  
  
  
    [code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]  { CALL getTotalCity(?) }  [/code]
  
  
  
    Note that now we use &#8220;?&#8221; (question mark) to represent each parameter.
  
  
  
    Third Example:
  
  
  
    Now we are going to call a stored procedure with IN and OUT parameters. Let&#8217;s follow the same rules as the fisrt example.
  
  
  
    We are going to use inline parameters and we are going to create a POJO class to represent our parameter.
  
  
  
    MyBatis code:
  
  
  
    [code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]  { CALL getTotalCityStateId( #{stateId, mode=IN, jdbcType=INTEGER}, #{total, mode=OUT, jdbcType=INTEGER})}  [/code]
  
  
  
    Param2 POJO:
  
  
  
    [code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;] package com.loiane.model;
  
  
  
    public class Param2 {
  
  
  
    private int total; private int stateId;
  
  
  
    public int getTotal() { return total; } public void setTotal(int total) { this.total = total; } public int getStateId() { return stateId; } public void setStateId(int stateId) { this.stateId = stateId; } } [/code]
  
  
  
    Fourth Example:
  
  
  
    Now let&#8217;s try to retrieve a resultSet from the stored procedure. For this we are going to use a resultMap.
  
  
  
    [code lang=&#8221;xml&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;]     
  
  
  
     { CALL getStates()}  [/code]
  
  
  
    State POJO class:
  
  
  
    [code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;] package com.loiane.model;
  
  
  
    public class State {
  
  
  
    private int id; private String code; private String name;
  
  
  
    public int getId() { return id; } public void setId(int id) { this.id = id; } public String getCode() { return code; } public void setCode(String code) { this.code = code; } public String getName() { return name; } public void setName(String name) { this.name = name; } } [/code]
  
  
  
    2- SPMapper &#8211; Annotations
  
  
  
    Now let&#8217;s try to do the same thing we did using XML config.
  
  
  
    Annotation for First Example (XML):
  
  
  
    [code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;] @Select(value= "{ CALL getTotalCity( #{total, mode=OUT, jdbcType=INTEGER} )}") @Options(statementType = StatementType.CALLABLE) Object callGetTotalCityAnnotations(Param param); [/code]
  
  
  
    It is very similiar to a simple select statement, but we have to set the statement type to CALLABLE. To do it, we can use the annotation @Options.
  
  
  
    With annotations, we can only use inline parameters, so we will not be able to represent the second exemple using annotations.
  
  
  
    Annotation for Third Example (XML):
  
  
  
    The explanation is the same as first example, I am just going to list the code:
  
  
  
    [code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;] @Select(value= "{ CALL getTotalCityStateId( #{stateId, mode=IN, jdbcType=INTEGER}, #{total, mode=OUT, jdbcType=INTEGER})}") @Options(statementType = StatementType.CALLABLE) Object callGetTotalCityStateIdAnnotations(Param2 param2); [/code]
  
  
  
    Annotation for Fourth Example (XML):
  
  
  
    I tried to set the fourth example with annotation, but the only thing I&#8217;ve got is this:
  
  
  
    [code lang=&#8221;java&#8221; firstline=&#8221;1&#8243; toolbar=&#8221;true&#8221; collapse=&#8221;false&#8221; wraplines=&#8221;false&#8221;] //TODO: set resultMap with annotations /*@Select(value= "{ CALL getTotalCityStateId()}") @Options(statementType = StatementType.CALLABLE) /*@Results(value = { @Result(property="id", column="state_id"), @Result(property="name", column="state_name"), @Result(property="code", column="state_code"), })*/ List callGetStatesAnnotations(); [/code]
  
  
  
    And it does not work. I tried to search on the mailing list, no luck. I could not find a way to represent a resultMap with annotation and stored procedures. I don&#8217;t know if it is a limitation. If you have any clue how to do it, please leave a comment, I will appreciate it! 
  
  
  
    Download
  



  
    If you want to download the complete sample project, you can get it from my GitHub account: https://github.com/loiane/ibatis-stored-procedures
  
  
  
    If you want to download the zip file of the project, just click on download:
  
  
  
    
  
  
  
    
  
  
  
    There are more articles about iBatis to come. Stay tooned!
  
  
  
    Happy Coding! 
  
