---
layout: post
title: The Power of Web Object Query Language
date: 2020-07-24T15:40:51.468Z
description: WOQL is a powerful and sophisticated query language which allows
  you to concisely express complex patterns over arbitrary data structures. What
  makes it so expressive and easy to use is the radical simplicity of the core
  underlying concepts.
image: /blog/assets/uploads/the-power-of-woql.png
author:
  - value: kevin
    label: Kevin Feeney
tags:
  - Database
---
WOQL is a powerful and sophisticated query language which allows you to concisely express complex patterns over arbitrary data structures. What makes it so expressive and easy to use is the radical simplicity of the core underlying concepts.

To unleash the power of WOQL, you just need to understand two very simple things.

### ***Rule 1: Triples all the way down***

In Terminus DB every single fragment of information is universally stored and accessible as triples. Triples are the universal and fundamental atom of information in Terminus DB—objects, documents and everything else is stored as collections of triples, and it is always possible to break any object down in a reliable and deterministic way into a precise set of triples. TerminusDB also adheres to the RDF standard, which means all triples have a regular structure, interpretation and strong norms. From a query writer point of view, this regularity is very helpful; you don’t have to care about the low-level structure of the data (what table it is stored in, how many columns it contains, etc.), you just have to care about the patterns of meaning you are interested in.

A triple is just a data structure with three slots to which we can assign values. Each row in the table below shows a different view of what each of the three slots means.

##### Three is the Magic Number: Understanding Triples

*\| Form | Triple Slot 1 | Triple Slot 2 | Triple Slot 3 | |—|—|—|—| |Terminus DB Terminology |Object ID | Property | Value |*

*\|Triple Example 1 | joe | date_born | 1/2/34 |* |Example 1 Interpretation | The record with ID joe | has a property called date_born | with Value 1/2/34 |* 

*\|Triple Example 2 | joe | parent | mary |*  |Example 2 Interpretation | The record with ID joe | has a property named parent | with Value mary |*

Every triple with the same ID is interpreted as being about the same thing. So if we add triples with different properties to our database which have the same IDs, they will be interpreted as representing different properties of the same thing. That’s how we build up information about things—just add properties to the appropriate record ID. The magic of triples is that the Value of a triple can be another record ID (as in the joe mother Mary example), and IDs can appear in either the first or the third slot of the triple.

Writing the above examples into TerminusDB with WOQL

*WOQL.add_triple('joe', 'date_born', '1/2/34')* 

.add_triple('joe', 'parent', 'mary')*

### ***Rule 2: Unify All The Things***

The second fundamental concept behind WOQL is unification. This is an old computer science concept that has been polished and refined for 60 years and is defined in obscure mathematical jargon, but it is remarkably simple and intuitive to understand by example.

##### WOQL Variables

In WOQL we have the concept of variables, which are normally represented by a string that starts with v: —we use them to store the results of queries. For example, we might define variables like v:First Name , v:Family Name, v:Date of Birth for a query to find somebody’s basic identifying information.

If we use a variable in a triple query, TerminusDB will show us every possible value that exists in the database that could possibly fill that variable in that position in the query.

SINGLE VARIABLE TRIPLE PATTERN EXAMPLES 

Adding a variable in the first slot of the triple will find the IDs of all the Objects that have a specific property set to the specified value: *WOQL.triple('v:Person ID', 'date_born', '1/2/34')*

Adding a variable in the second slot will find the names of all the properties that the specified object has which contains that value: *WOQL.triple('joe', 'v:Property List', 10)  // returns list of all the properties of Joe that have a value equal to 10* 

 Putting a variable in the third slot will find the value(s) of the specified property for the specified object ID: *WOQL.triple('joe', 'parent', "v:Joes Parents")*  

*TWO VARIABLE TRIPLE PATTERN EXAMPLES* 

Putting a variable in the first two slots of the triple, will find all object IDs and properties in the database that have the specified value

 *WOQL.triple('v:Object ID', 'v:Properties', 10) //list of all objects and property names that have the value 10.* Putting variables in the first and third slots will find the names of all the properties that the specified object has which have that value

*WOQL.triple('v:Object ID', 'date_born', 'v:Date of Births')  // returns list of all object ids and the values of their date_born properties*\
Entering variables in the second and third slot finds all the properties and their values for the object with the specified ID: *WOQL.triple('joe', 'v:Joes Properties', "v:Property Values")*  

The third and final pattern is easier still; adding variables in all three slots will match every single triple in the database. In fact, because this is so useful, WOQL provides a special built-in shortcut for generating this pattern: *WOQL.star() //generates a triple query with variables in all 3 spots - returns all triples in the database*

##### Logical Operators

Single triple pattern matching, as shown above is certainly neat, but there’s a limited number of things that can be expressed as a single pattern of triples, even with all our variables turned on. However, WOQL also provides logical operators, AND, OR which allows you to combine as many basic patterns as you need into incredibly expressive queries in very simple forms.

The most useful operator is logical AND—*WOQL.and()*—which behaves as we would logically expect; if all of the patterns defined within a WOQL.and() can be filled by the database, all of the results will be returned, otherwise, no results will be returned.

The other basic logical operators: OR—*WOQL.or()*—and NOT—*WOQL.not()*—are also very useful and are interpreted also as expected. In the first case, the query will return the first result that it matches in a list of possibilities, in the second case, the query will only return true if the pattern specified is not found in the database.

It is amazing how many things you can readily express simply by combining these patterns in different ways. Extreme simplicity and absolute regularity in the little things allows extreme elegance of description in the big things.

### ***That’s not all folks***

Another advantage of a simple and regular underlying architecture is that it becomes much easier to build extra functionality on top. In addition to the basic ideas presented here, WOQL also has a broad set of built-in operators and libraries which cover arithmetic, mathematical, date and time, taxonomy specific patterns, aggregation, ordering, grouping, geographical and a wide range of other functions out of the box. This allows you to move a lot of your logic into the Database Layer and out of your application code. The database should be the only thing that cares about the structure of the storage layer, the rest of us care about getting the data we want!

The rest of this chapter contains lots of information about all the functions and operators that WOQL provides and how you can access them, but before you leave, we have a question to ask. I want to ask my database for the full records of all living people whose direct ancestors were born in Italy before 1850 (assuming I have the records of course) along with their lineage.

*In WOQL my query would look like this: WOQL.("v:Living Person Record ID", "status", "alive")    	 .path("v:Living Person Record ID", "parent+", "v:Italian Ancestor", "v:Ancestry Line")    	
.triple("v:Italian Ancestor", "date_born", "v:Date of Birth")*\
.less("v:Date of Birth", 1850) .triple("v:Italian Ancestor", "country_born", "Italy")
.get_object("v:Living Person Record ID", "v:Full Record")*

How would you ask your database this question?