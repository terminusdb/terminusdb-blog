---
layout: post
title: "Graph Databases, TerminusDB vs Neo4j"
date: 2020-09-24
description: "Let’s see the main differences between TerminusDB and Neo4j in practice"
image: /blog/assets/images/clint-adair-unsplash.jpg

author:
  - value: francesca
    label: Francesca Bitto
tags:
  - graph
  - Terminus
  - Database
  - Data
  - Development
---

Are the Graph Databases the future?  Structured and unstructured data coming from multiple sources can have an immense value if you can find relationships between them, Graph databases hold the relationships between data as a priority.

Inter-connected data is the future and probably the easy way to model interconnected data is using Graph Databases

In this tutorial will see two different way to model and query data using native graph databases technology.

if you have never used TerminusDB before, this article includes everything you need to get started with TerminusDB.
[My First TerminusDB Graph Visualisation — Bike Share Data](/2020/01/14/my-first-terminusdb-graph-visualisation-bike-share-data/).

Now let’s see how TerminusDB handles tasks compared to Neo4j in practice.

*Note: We are assuming you are already familiar with the basic process of designing a property graph data model using Neo4j*


## Graph Database Main Concepts


Graph databases  are databases that use graph structures for semantic queries with nodes, edges (relationship), and properties to represent and store data. The relationships allow data in the store to be linked together. 


#### Neo4J

- Node 
- Label (:Person)
- Relationship :KNOWS
- Property {name}

![](/blog/assets/images/neo4j-graph-know.png)

In Neo4j the main components of the property graph model are nodes and relationships, in our example, **Maria**, **Anna**, **Tom** and **Jim** are our nodes and **KNOWS** is our relationship between nodes. 

Nodes and Relationships can have properties, properties are name-value pairs that provide additional details to nodes and relationships. You can group similar nodes together by assigning a node label (:Person). A node can have zero to many labels. 


#### TerminusDB

 - OrdinaryClass 
 - DocumentClass (doc:Person)
 - ObjectProperty (knows:doc:Person)
 - DatatypeProperty (name:String)

![](/blog/assets/images/terminusdb-graph-knows.png)


In TerminusDB everything is a Class object - objects can have properties and some of these properties may link to other objects, in our example **Maria**, **Anna**, **Tom**, and **Jim** are our Document Object the range data type of the **knows** property (ObjectProperty) is the **Person** document.

Classes can be subclasses of other classes, which means that they inherit all the parent’s definitions (much like inheritance in object-oriented programming).

the type of data that the property points to. The range can either be a simple datatype literal (e.g. an integer or string) or it can be a class.

## Query language

Neo4j uses **Cypher** to store and retrieve data from the graph database. Cypher is 
a graph query language and the best way to interact with Neo4j. 

In TerminusDB we use [**WOQL** (Web Object Query Language)](https://terminusdb.com/docs/woql). It is a unified model query language, WOQL's primary syntax and interchange format is in JSON-LD.
All our example are writing using woql.js a javascript layer that help you to compose WOQL query.

## Schema

A schema in Neo4j refers to indexes and constraints. Neo4j is often described as schema optional, meaning that it is not necessary to create indexes and constraints. Index and Constraint can be added at any time.

*In our example we create the constaint **person_unique**, the properties **name** and **born** have to exist on all the nodes with label **Person** and the combination of the property values is unique.*

```sql
CREATE CONSTRAINT person_unique
ON (n:Person) ASSERT (n.name, n.born) IS NODE KEY
```

TerminusDB is schema optional but for schema checking is better create a schema before insert data, with terminusDB you can change your schema at any time. 
Remember TerminusDB is a graph database that stores data like Git. **TerminusDB allows for the whole suite of revision control features: branch, merge, squash, rollback, blame, and time-travel.**

[**If you like to read more ...**](https://terminusdb.com/docs/quickstart/)

```js
WOQL.doctype("Person")            
    .label("Person Name")            
    .description("A Person Document")
	.property("name", "string").label("Name").cardinality(1)
	.property("date_of_born", "date").label("Date Of Born").cardinality(1)
	.property("knows", "Person").label("Knows")
```

Using the WOQL.js api, we create the **Person** Document class. This object has 3 properties, with three different datatypes. The data type range in **knows** property (ObjectProperty) is the **Person** document. The cardinality 1 for *name* and *born* said that the combination of the property values is unique for every Document instance.

## Insert Data 

Here an example of Cypress syntax for insert data

We add 4 nodes of type (label) **Person** and relationships **KNOWS** between the nodes.

```sql

CREATE (maria:Person { name:"Maria",born:date('1978-12-03')})
CREATE (anna:Person { name:"Anna",born:date('1974-02-10')})
CREATE (tom:Person { name:"Tom",born:date('1975-06-23')})
CREATE (jim:Person { name:"Jim",born:date('1974-07-20')})

MERGE (maria)-[r:KNOWS { label: maria.name + ' knows ' + anna.name }]->(anna)
MERGE (anna)-[r01:KNOWS { label: anna.name + ' knows ' + tom.name }]->(tom)
MERGE (tom)-[r02:KNOWS { label: tom.name + ' knows ' + maria.name }]->(maria)
MERGE (tom)-[r03:KNOWS { label: tom.name + ' knows ' + jim.name }]->(jim)

RETURN maria.name, anna.name, tom.name, jim.name

```


Let's see how add Documents and relationships connection in WOQL

TerminusDB is accessible in very easy with JavaScript using the woql.js layer

We create **Person** documents and we link this Documents using the property **"knows"** property in **Person** document. Our relationship link between document has been created

```js
and(
    idgen("doc:Person",["Maria","1978-12-03"],"v:Maria"),
    idgen("doc:Person",["Anna","1974-02-10"],"v:Anna"),
    idgen("doc:Person",["Tom","1975-06-23"],"v:Tom"),
    idgen("doc:Person",["Jim","1974-07-20"],"v:Jim"),
 	
    insert("v:Maria", "Person").label("Maria")
  		.property("date_of_born", {'@value':"1978-12-03",'@type': 'xsd:date'})
		.property("name", "Maria")
       	.property("knows", "v:Anna"),

    insert("v:Anna", "Person").label("Anna")
  		.property("date_of_born", {'@value':"1974-02-10",'@type': 'xsd:date'})
		.property("name", "Anna")
       	.property("knows", "v:Tom"),

    insert("v:Tom", "Person").label("Tom")
  		.property("date_of_born", {'@value':"1975-06-23",'@type': 'xsd:date'})
		.property("name", "Tom")
       	.property("knows", "v:Maria"),

     insert("v:Jim", "Person").label("Jim")
  		.property("date_of_born", {'@value':"1974-07-20",'@type': 'xsd:date'})
		.property("name", "Jim")
       	.property("knows", "v:Tom")
)
```

## Add a new node type and relationship

In Neo4j we add a node with the label **City**, a new constaint and we connect our nodes **Person** and **City** with **CITY_OF_BORN** relationship 

```sql
CREATE(dublin:City { name:"Dublin"})

CREATE CONSTRAINT city_name ON (city:City) ASSERT city.name IS UNIQUE

MATCH (a:Person),(b:City)
WHERE b.name = 'Dublin'
MERGE (a)-[r:CITY_OF_BORN { label: a.name + ' born in ' + b.name }]->(b)
RETURN type(r), r.label

```

In TerminusDB we create a new Document Object **City**

```js
WOQL.doctype("City").label("City Name").description("A City name")
```

We add a new property **city_of_born** in the Document **Person** with range type **City**
Here our relationship between **Person**->**City**

```js
add_property("city_of_born","City").domain("Person")
```

Now update the data!!

```js 
and(
    idgen("doc:City",["Dublin"],"v:City_id"),
    insert("v:City_id", "City").label("Dublin"),
  	triple('v:Person','type','scm:Person'),
	add_triple("v:Person","city_of_born","v:City_id")
)

```

## Creating hierarchies 

In our graph we like to add another kind of Person called Doctor, this person need to have all the property of Person plus patient

In Neo4j If we like to said that one node is a Person and a Doctor we can add a multi label to this node,

```sql

create (Freud:Person:Doctor {name:'Freud', born:'1976-08-25'})

MATCH (a:Person),(b:Doctor)
WHERE a.name = 'Maria' AND b.name="Freud"
MERGE (a)-[r:IS_PATIENT_OF { label: a.name + ' is patient of ' + b.name }]->(b)
RETURN type(r), r.label

MATCH (a:Person),(b:Doctor)
WHERE a.name = 'Tom' AND b.name="Freud"
MERGE (a)-[r:IS_PATIENT_OF { label: a.name + ' is patient of ' + b.name }]->(b)
RETURN type(r), r.label

```


In terminusDB classes can be subclasses of other classes, so let’s add a subclass for scm:Person called scm:Doctor.

A Doctor shares all of the properties available to a Person, but it also has a patient. 

```js

WOQL.doctype("Doctor")
  .parent("Person")
  .label("Doctor")
  .description("A Doctor is a person with patients")
  .property("patient", "Person")

WOQL.and(
    idgen("doc:Doctor",["Freud","1976-08-25"],"v:Freud"),
    insert("v:Freud", "Doctor").label("Freud")
  	.property("date_of_born", {'@value':"1976-08-25",'@type': 'xsd:date'})
	.property("name", "Freud")
	.property("patient","doc:Person_Maria_1978-12-03")
	.property("patient","doc:Person_Tom_1975-06-23")
       	
)

```

## Query the Data

Now, how we write our query for getting how the **Person** **knows** each others

Here an Cypher example

```sql

MATCH (person:Person)-[:KNOWS]-(otherPerson:Person)
RETURN person.name,otherPerson.name

```

Let's see the TerminusDB woql query using woql.js 

```js

or(triple('v:Person', 'knows', 'v:OtherPerson'),
   triple('v:OtherPerson','knows','v:Person')
)

```

Now, how do we get only people who know each other and are patients of the doctor **Freud** ?

Neo4j Cypher query example

```sql



```

TerminusDB query example

```Js

and(
	triple('v:Person', 'knows', 'v:OtherPerson'),
	triple("v:Doc",'patient','v:Person'),
	triple("v:Doc",'patient','v:OtherPerson'),
  	triple("v:Doc",'name','v:Freud')
)

```


If you like to jump in anothe intersting tutorial and keep to build new TerminusDB graph data model
[Bank Tutorial](https://terminusdb.com/blog/2020/07/27/taking-terminusdb-to-the-bank/)


