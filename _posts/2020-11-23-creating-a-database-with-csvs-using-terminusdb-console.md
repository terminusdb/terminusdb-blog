---
layout: post
title: Creating a database with CSVs using TerminusDB console
date: 2020-11-23T13:55:43.719Z
description: How to create a database by importing CSVs from your local file system
image: /blog/assets/uploads/photo-1529078155058-5d716f45d604.jpg
author:
  - value: Kitzkan
    label: Kitty Jose
tags:
  - database csvs import
---
Hello everybody! I am going to write a tutorial on how to work with CSVs using TerminusDB. That's right, TerminusDB 4.0 now has the feature in which you can import and export CSVs, Let's dive into this.

### How to create a database with CSVs



1. Open up the TerminusDB console and create a database
2. Fill in the Id, Name and description of your database
3. You will find a button Create Database from CSVs

![create database with csvs](/blog/assets/uploads/1-createdb.png "create database with csvs")

4. Click on this button to load which ever CSVs you wish to import to your database, In this blog I am going to use [The Bike Share Data](https://terminusdb.com/blog/2020/09/01/my-first-terminusdb-3-0-graph-bike-share-data/)
5. I have added 2 CSVs into my database as shown below

![different csvs loaded into db](/blog/assets/uploads/2-createdbwithcsvpreview.png "different csvs loaded into db")

6. And that's it, go ahead and click on Create New database, which includes the 2 CSVs in it 😊😊😊😊😊

### **What happens when you import CSVs into your Database?**



On creating our database, Go to the Documents Page

![document page view](/blog/assets/uploads/2-dbdocumentpage.png "document page view")

Each of our CSV files that were loaded on create becomes a separate Document Type. All the columns in the CSV file have been converted into properties and a schema is auto-generated.

Each CSV is of doctype CSV and has rows hanging off them, we can use these auto generated column properties to query the database.

You can also add more CSVs once you have created your database by clicking the Add CSVs

Go to the Schema Page to view the property types which have been generated off the columns

![comaprison between excel and loaded csv](/blog/assets/uploads/3-trasnformcsvtoproperties.png "comaprison between excel and loaded csv")

### Let’s Query the Database



Get CSV Id’s, Query for type scm:CSV

```
and (
    triple('v:CSV ID', 'type','scm:CSV'),
    triple('v:CSV ID', 'label', 'v:CSV Name')
)
```

![query database to get csv Id](/blog/assets/uploads/4-querycsvid.png "query database to get csv Id")

This query simply gives you the ID and name of available CSVs in the Database. Take a note of the CSV Id you're interested in, also visit the Schema page to get the Property Id’s your interested in as well.

Open a new Query Pane and try the below query which shows the duration from start station to end station

```
const id="doc:CSV_bike_tutorial-1.csv"

WOQL.and (
  .limit(1).triple('v:CSV ID', 'type', 'scm:CSV')
    .eq('v:CSV ID', id)
    .triple('v:CSV ID', 'scm:csv_row', 'v:CSV Row')
    .triple('v:CSV Row', 'type', 'v:Row Type'),
  .quad('v:Property', 'domain', 'v:Row Type', 'schema/main')
    .quad('v:Property', 'label', 'v:Property Name' ,'schema/main')
)
```

This query gets all the property Id and name from the schema graph.

Next, let's query to display a few columns of our CSV, I plan to display Start Station Number, Start Station, End Station Number, End Station and Duration

```
const id="doc:CSV_bike_tutorial-1.csv"

const startStationNumber = "scm:column_Start%20station%20number"
const StartStation = "scm:column_Start%20station"
const endStationNumber = "scm:column_End%20station%20number"
const endStation = "scm:column_End%20station"
const duration = "scm:column_Duration"

and (
	triple('v:CSV ID', 'type','scm:CSV').eq('v:CSV ID', id)
      .triple('v:CSV ID', 'scm:csv_row', 'v:CSV Row')
      .triple('v:CSV Row', startStationNumber, 'v:Start Station Number')
      .triple('v:CSV Row', StartStation, 'v:Start Station')
      .triple('v:CSV Row', endStationNumber, 'v:End Station Number')
      .triple('v:CSV Row', endStation, 'v:End Station')
      .triple('v:CSV Row', duration, 'v:Duration')
)
```

All the rows of CSV matching the column names will be displayed on running this query

So yeah that's it, you can do all sorts of query on your CSV



### Export your CSV



Now that we have imported our CSVs into our database we can do whatever we like or modify it however we want and Export this CSV back.

Go to Documents, and filter for all CSV types to get a download option as shown and download the CSV.

![export csv from document page ](/blog/assets/uploads/5-export.png "export csv from document page ")

So here we go, this tutorial shows a very simple way to import  CSVs into TerminusDB and play with them and do all sorts of useful cool queries, whatever you like.

Earlier we had to do all the steps mentioned in [TerminusDB: Importing, Reordering & Exporting a CSV](https://terminusdb.com/blog/2020/07/13/terminusdb-importing-reordering-exporting-a-csv/) not anymore!

Now it's just a few button clicks and voilà!