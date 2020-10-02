---
layout: post
title: TerminusDB vs MongoDB
date: 2020-07-14T10:44:15.289Z
description: "Let’s see the main differences between TerminusDB and MongoDB in practice"
image: /assets/images/directory-106815_1920.jpg
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

Modern application requirements have pushed beyond the limitations of DBMS (relational database management systems). NoSQL databases play an important role in cloud software applications, they store data in a format other than relational tables that does not require a fixed schema, they come in a variety of types based on their data model.

There are four main types of databases in the NoSQL (stands for “Not Only SQL" or "Not SQL."):  key-value pair based, wide column, document-oriented and graph-based.


Both TerminusDB and MongoDB are NoSQL databases, both store data as documents, both have a flexible data model, that means that the database schema is flexible and can evolve in the time when the user exigences change, both databases support a rich query language, but TerminusDB is a **version control document system** and a **graph database** as well. 

Now let’s see the main differences between TerminusDB and MongoDB in practice.

if you have never used TerminusDB before, this article includes everything you need to get started with TerminusDB.
[My First TerminusDB Graph Visualisation — Bike Share Data](/2020/01/14/my-first-terminusdb-graph-visualisation-bike-share-data/).

*Note: We are assuming that you already are familiar with MongoDB.*

--------------------

## The Dataset

First look at our dataset, which data we need to store and how this data is related to each other.

In our examples we use the collection of data about the bike journeys between stations in Washington D.C., USA.

![](/blog/assets/images/bike_table.png)

The CSV data used this tutorial is available at [https://terminusdb.com/t/data/bike_tutorial.csv](https://terminusdb.com/t/data/bike_tutorial.csv)

--------------------

## Query language

MongoDB uses the MongoDB Query Language (**MQL**) for accessing the documents.

In TerminusDB we use [**WOQL** (Web Object Query Language)](https://terminusdb.com/docs/woql). It is a unified model query language, WOQL's primary syntax and interchange format is in JSON-LD.
All our example are writing using woql.js a javascript layer that help you to compose WOQL query.

--------------------

## Create Schema

MongoDB stores data as JSON-like documents. JSON (JavaScript Object Notation) documents support embedded fields, so documents can store related information (data or list of data) together, fields can vary from document to document.

You can insert a document in the collection and MongoDB creates that collection on the fly or maybe you can create a collection with a schema validation before .


Let’s see how to create a new collection with specified validation rules. Here a fragment 
for mongoshell:
```js

 db.createCollection("journey", {
   validator: {
      $jsonSchema: { bsonType: "object",
         required: [ "duration", "member_type", "bike_id", "end_time","start_time", "start_station","end_station"],
         properties: {            
            member_type: {bsonType: "string", description: "the member subscription type” },
            bike_id:{ bsonType: "string", description: "....” },
            start_time:{ bsonType: "date, description: "......”},
            start_station: { bsonType: "object", required: [ "station_number","address" ],
               properties: {
                  station_number: {  bsonType: "int", description: "...."  },
                  address: {bsonType: "string", "description": "....."} } },                         
            ...............................
        } }}})

```
We have created our **journey** collection with embedded  the **start_station** and **end_station** data entities (station_number,address) in the journey collection data. For many use cases in MongoDB, the denormalized data model is optimal.


In TerminusDB we have documents as well, so for our dataset we prefer to create three different documents: Station, Journey and Bicycle, every document has a label and a description and is identified by an unique URL. In our documents we have properties to describe the type of data and the documents are related to each other.  Following the link between the documents we can track the relationship between connected data for example **Station** - *many* - **Journey**.

An example of relationship between documents written in woql.js

```js

WOQL.when(true).and(        
   WOQL.doctype("Station")            
       .label("Bike Station")            
       .description("A station where bicycles are deposited"),        
   WOQL.doctype("Bicycle")            
       .label("Bicycle"),        
   WOQL.doctype("Journey")            
       .label("Journey")
       .property("start_station", "Station").label("Start Station")
       .property("end_station", "Station").label("End Station")
       .property("journey_bicycle", "Bicycle").label("Bicycle Used")
…...)         

```
In the **Journey** document the range data type of the **start_station** and **end_station** properties (ObjectProperty) are the **Station** document.

--------------------

## Load Data 

In MongoDB you can’t import a csv file in the journey collection directly. 
We could use mongoimport.  

```shell

mongoimport --db=mybike --collection=journey --type=csv  --ignoreBlanks --headerline --file="./bike_tutorial.csv"

```

But the result will be a different collection schema

```json

{ "_id":"ObjectId(5f05f055c74146b286a61cf0)",
   "Duration":831,
   "Start date":"2018-12-01 00:06:00",
   "End date":"2018-12-01 00:19:52",
   "Start station number":31202,
   "Start station":"14th & R St NW",
  ........}

```

if you like to create the documents from the csv file inside the collection **journey** you have to implement your custom solution, maybe you could using javascript for import the data from the csv file ( csvtojson is a node.js library) and **mongodb** the official MongoDB driver for Node.js to insert the data into the collection.

Here one example to insert a document inside the journey collection using *mongoshell*.

```js

db.journey.insert({"_id":"31203_2018-12-01 00:00:44_31116_2018-12-01 00:13:54_W21477", 
    "start_station":{"station_number":NumberInt(31203),"address": "14th & Rhode Island Ave NW"},
    "end_station":{"station_number":NumberInt(31116),"address":"California St & Florida Ave NW"},
    "start_time":ISODate("2018-12-01 00:00:44"),
    "end_time":ISODate("2018-12-01 00:13:54"),
    "member_type":"Member",
    "duration":NumberInt(790),
    "bike_id":"W21477"})

```

In TerminusDB we have a very cleaner solution to import csv files into the database, with a data integrity check for any newly imported records. We use the woql.js layer for write the query statement to load the data from the csv file.

A fragment of an import query.
```js

const csv = WOQL.get(
    WOQL.as("Start station","v:Start_Station")
        .as("End station", "v:End_Station")
  		......     
        .as("End station number", "v:End_ID")
        .as("Bike number", "v:Bike")
        .as("Member type", "v:Member_Type")
).remote("https://terminusdb.com/t/data/bike_tutorial.csv")

```
We use **WOQL.as(a,b)** to refer the first row of our csv file that contain the column headers in our variables, for example the field **Start station number** refers the variable **v:Start_ID** and the  **Start station** refers the variable **v:Start_Station**

```js

//Clean data for insert
const wrangles = [
    WOQL.typecast("v:Duration", "xsd:integer", "v:Duration_Cast"),
    WOQL.typecast("v:Bike", "xsd:string", "v:Bike_Label"),
    WOQL.typecast("v:Start_Time", "xsd:dateTime", "v:ST_Cast"),
    WOQL.typecast("v:End_Time", "xsd:dateTime", "v:ET_Cast"),
    WOQL.typecast("v:Start_Station", "xsd:string", "v:SS_Label"),
    WOQL.typecast("v:End_Station", "xsd:string", "v:ES_Label"),
    WOQL.idgen("doc:Journey",["v:Start_ID","v:Start_Time","v:Bike"],"v:Journey_ID"),       
    WOQL.idgen("doc:Station",["v:Start_ID"],"v:Start_Station_URL"),
    WOQL.idgen("doc:Station",["v:End_ID"],"v:End_Station_URL"),
    WOQL.idgen("doc:Bicycle",["v:Bike_Label"],"v:Bike_URL"),    
    WOQL.concat("v:Start_ID - v:End_ID @ v:Start_Time","v:J_Label"),
    WOQL.concat("Bike v:Bike from v:Start_Station to v:End_Station at v:Start_Time until v:End_Time","v:Journey_Description")
]; 

```

We cast (WOQL.typecast(a,b,c) method) the variable **v:Start_Station** as string (“xsd:string”) and refer it in a new variable **v:SS_Label**

With WOQL.idgen(a,b,c) we create an unique id for every document. Example for *doc:Station* document we create an id from the *v:Start_ID* and refer it in the *v:Start_Station_URL*

```js

const inserts = WOQL.and(
    WOQL.insert("v:Journey_ID", "Journey")
        .label("v:J_Label")
        .description("v:Journey_Description")
        ....
        .property("start_station", "v:Start_Station_URL")
        .property("end_station", "v:End_Station_URL")
        .property("journey_bicycle", "v:Bike_URL"),
    WOQL.insert("v:Start_Station_URL", "Station")
        .label("v:SS_Label"),
    WOQL.insert("v:End_Station_URL", "Station")
        .label("v:ES_Label"),
    WOQL.insert("v:Bike_URL", "Bicycle")
        .label("v:Bike_Label")
);

```

With WOQL.insert() we insert  the data inside our database. We create a **Station** document for every **v:Start_station_URL** and **v:End_station_URL** value and we link this value inside the **start_station** and **end_station** properties in the Journey document. The relationship between journey document and station document  has been created. The relationship allows TerminusDB to normalize the data across multiple documents.❗
In this article the complete example [My First TerminusDB Graph Visualisation — Bike Share Data](/2020/01/14/my-first-terminusdb-graph-visualisation-bike-share-data/).

--------------------

## Query the Data

Here is an example of query operations using the db.collection.find() method in mongoshell, we select from the **journey** collection all documents where the status **bike_id** equals **W21477**.

![](/blog/assets/images/query-mongo.png)


The following example selects all the **journey** documents where the **journey_bicycle** property is linked at the **bike** document **W21477**

```js

WOQL.triple("v:Journey", "journey_bicycle", "v:Bike").triple("v:Bike", "label", "W21477").read_object("v:Journey", "v:Journey Doc")

```

![](/blog/assets/images/terminusdb-query.png)

Remember that TerminusDB is a graph-database, this means that TerminusDB
creates relationships between the documents and properties inside documents.
TerminusDB can analyze and traverse complex relationships between connected data structures. Let’s keep to practise❗









