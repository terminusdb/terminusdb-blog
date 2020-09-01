---
layout: post
date: 2020-09-01T18:34:56.791Z
author:
  - value: luke
    label: Luke Feeney
cover_image: https://miro.medium.com/max/10944/1*wHpEF7tcwMY0OqIC33mxKA.jpeg
title: My First TerminusDB 3.0 Graph — Bike Share Data
description: "Bike tutorial with 3.0. Terminators rule ok. "
image: /blog/assets/uploads/0-zba-5cqjbrrvqsal.jpeg
tags:
  - Database
  - Data
  - Graph
  - Visualization
---
In this tutorial, we will start using TerminusDB 3.0, TerminusDB Console and its query client. This is a step-by-step guide to create your first knowledge graph.

We’re assuming you already have TerminusDB installed, but if you haven’t go to our [download center](https://terminusdb.com/hub/download). It’s very easy to get started.

In this tutorial, we are going to import data from CSVs about bike journeys between stations in Washington D.C., USA, build a knowledge graph from the data, analyze and visualize it. The data is taken from Capital Bike Share (available [here](https://www.capitalbikeshare.com/system-data) — thanks folks) and the CSV data used this tutorial is available at <https://terminusdb.com/t/data/bike_tutorial.csv>

<iframe width="560" height="315" src="https://www.youtube.com/embed/6BK5tPHKryc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



- - -

# Create a Database

Open up the TerminusDB console (https://127.0.0.1:6363/ — go to our [download center](https://terminusdb.com/hub/download) to get up and running). Click on `Create` to start.

Click on `Create` to start and select 'TerminusHub Database’ as we want to share this with the world.

You can specify an id for the database, to make it memorable, let’s make it `1stdb` (note that Terminus IDs are URLs and they cannot have spaces!). As a title, enter the name you want to give your Database, something meaningful like `My First Database`. Then you can add a short description to your database, like `It is my first database using TerminusDB`.

![Create Database](/blog/assets/uploads/capture.jpg)

Click the `Create On TerminusDB` button at the top right, and you’ll automatically go to the main database page. Something like this:

![Successfully created Database](/blog/assets/uploads/db-home.jpg)

Now we have a database, let’s have a look at the data we want to import. Click on the `Query` link in the left hand menu and you should see a query page that looks like the screen below.

- - -

- - -

# Let’s Create a Schema

The schema allows you to organize data into meaningful objects, and it ensures data integrity — nothing goes into your database that is not in the schema. This is a TerminusDB super power — and ensures you derive long term value from your data.

The TerminusDB Console provides a schema editor using WOQL.js. Remaining on the query page, copy this WOQL.js query into the text box:

```js
WOQL.and(         
    WOQL.doctype("Station")             
        .label("Bike Station")             
        .description("A station where bicycles are deposited"),         
    WOQL.doctype("Bicycle")             
        .label("Bicycle"),         
    WOQL.doctype("Journey")             
        .label("Journey")             
        .property("start_station", "Station")
            .label("Start Station")             
        .property("end_station", "Station")
            .label("End Station")                                                
        .property("duration", "integer")
            .label("Journey Duration")         
        .property("start_time", "dateTime")
            .label("Time Started")
        .property("end_time", "dateTime")
            .label("Time Ended")
        .property("journey_bicycle", "Bicycle")
            .label("Bicycle Used")
)
```

Add a commit message in the box above (e.g. “Add Schema”) and click `Run Query`

![](/blog/assets/uploads/add-schema.jpg)

It worked!

![](/blog/assets/uploads/success-schema.jpg)

Let’s stop to review this schema-building query:

We perform all operations within the `and`.

So here’s the operations we have performed:

1. We created three different document types (given by the `doctype`function): `Station` , `Journey` and `Bicycle`
2. We added `label` (names) or `description` (short descriptions) to them.
3. We created properties for `Journey`, we do that by using the `property` function after `Journey` with the first argument as the name of the property and the second argument as the type (or range) of the property.
4. For each property, you have to provide an id and the type of that property in `property`, as with class you can add a `label` to it as well.

We can view the result by clicking the ‘Schema’ tab:

![showing classes in schema](/blog/assets/uploads/schema-view.jpg)

- - -

# Let’s Load in the Data

Now let’s get to the good part. We’ll now load the data from the CSV. We’re going to progressively extend the query to import the data, cleaning it and matching it as we go. WOQL is a highly composable language, you can combine queries arbitrarily using logical ANDs and ORs.

**Let’s build the next query in steps and only hit**`Run Query`**at the end of the query (full query is available at the bottom of the section)**

Go back to the `Query` page, and copy in the following query:

```js
//Read data from CSV
const csv = WOQL.get(
    WOQL.as("Start station","v:Start_Station")
        .as("End station", "v:End_Station")
        .as("Start date", "v:Start_Time")
        .as("End date", "v:End_Time")
        .as("Duration", "v:Duration")
        .as("Start station number", "v:Start_ID")
        .as("End station number", "v:End_ID")
        .as("Bike number", "v:Bike")
        .as("Member type", "v:Member_Type")
).remote("https://terminusdb.com/t/data/bike_tutorial.csv")
//Transform data into correct shape for insert
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
    WOQL.idgen("doc:Bicycle",["v:Bike_Label"],"v:Bike_URL"),    WOQL.concat("v:Start_ID - v:End_ID @ v:Start_Time","v:J_Label"),
    WOQL.concat("Bike v:Bike from v:Start_Station to v:End_Station at v:Start_Time until v:End_Time","v:Journey_Description")
]; 
//Combine with logical and
WOQL.and(csv, ...wrangles)
```

We’ll explain each of the steps in this query in turn. As a first step, we saved the part of the query that imports the data from CSV in a `const` variable named `csv`. Then we create a list of WOQL operators and save it in another constvariable called `wrangles`, we combine the two parts of the query with a `WOQL.and` operator.

The `wrangles` clause uses 3 WOQL functions to transform the data into the correct form for input. In each case, the function creates a new variable as output — the last argument in each case. The `idgen` function generates IDs for our three document types `Journey`, `Station`, and `Bicycle`. The first argument is the prefix that will be used, the second is a list of variables which combine to give a unique identity for the id. For example, in `Journey` we use 3 fields in the `csv` (`Start_ID`, `Start_Time` and `Bike`) to generate a unique id `Journey_ID`.

Besides generating IDs, we also create new fields with new data types, for example, we use `typecast` to cast `Duration` into `integer` and store it as `Duration_Cast`. We can also use `concat` to contract new text formatted with variables in the fields — for example, to create `Journey_Label`.

We’ll insert them in the graph by adding `triples`! Triples are the atomic data entity in the RDF data model. We add the following to the query:

```js
const inputs = WOQL.and(csv, ...wrangles)
const inserts = WOQL.and(
    WOQL.insert("v:Journey_ID", "Journey")
        .label("v:J_Label")
        .description("v:Journey_Description")
        .property("start_time", "v:ST_Cast")
        .property("end_time", "v:ET_Cast")
        .property("duration", "v:Duration_Cast")
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

If we hit `Run Query` for this section and you should see the following in the `Results Viewer` tab:

![](/blog/assets/uploads/load-in-the-data-1.jpg)

Click on the `table` drop down menu in the top right of the results and switch to `graph` view:

![](/blog/assets/uploads/graph-of-data-load.jpg)

Now you can see your results in graph form.

We’ll explain each of the steps in the query in turn. As a first step, we saved the part of the query that imports the data from CSV in a `const` variable named `csv`. Then we create a list of WOQL operators and save it in another constvariable called `wrangles`, we combine the two parts of the query with a `WOQL.and` operator.

The `wrangles` clause uses 3 WOQL functions to transform the data into the correct form for input. In each case, the function creates a new variable as output — the last argument in each case. The `idgen` function generates IDs for our three document types `Journey`, `Station`, and `Bicycle`. The first argument is the prefix that will be used, the second is a list of variables which combine to give a unique identity for the id. For example, in `Journey` we use 3 fields in the `csv` (`Start_ID`, `Start_Time` and `Bike`) to generate a unique id `Journey_ID`.

Besides generating IDs, we also create new fields with new data types, for example, we use `typecast` to cast `Duration` into `integer` and store it as `Duration_Cast`. We can also use `concat` to contract new text formatted with variables in the fields — for example, to create `Journey_Label`.

We’ll insert them in the graph by adding `triples.` Triples are the atomic data entity in the RDF data model. We add the following to the query:

```js
const inputs = WOQL.and(csv, ...wrangles)
const inserts = WOQL.and(
    WOQL.insert("v:Journey_ID", "Journey")
        .label("v:J_Label")
        .description("v:Journey_Description")
        .property("start_time", "v:ST_Cast")
        .property("end_time", "v:ET_Cast")
        .property("duration", "v:Duration_Cast")
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

This is the clause that actually inserts the data into the structure that we defined on our schema.

* The `insert` function inserts a new node into the database with the id `Journey_ID` and type `Journey` we add properties like `start_time`, `end_time`, `duration`, `start_station`, `end_station` and `label` and put the variables produced above in their correct spots.
* For `Start_Station_URL`, `End_Station_URL` and `Bike_URL`, we assign a `type` and `label` for each of them.

Finally, we have to put all of the above together and create the query that reads the data from the csv, do the data wrangling and add them in the graph as triples. We add this to complete the query:

```
WOQL.and(inputs, inserts);
```

Write some commit message again (e.g. “inserting data”) and click `Run Query`. Remember, this is getting the data in to our graph so you won’t have a query output just yet. 

The little x closes the view pane and the button opens it (both circled in red)

The full query in all it’s glory and in easy to copy format:

```js
const csv = WOQL.get(
    WOQL.as("Start station","v:Start_Station")
        .as("End station", "v:End_Station")
        .as("Start date", "v:Start_Time")
        .as("End date", "v:End_Time")
        .as("Duration", "v:Duration")
        .as("Start station number", "v:Start_ID")
        .as("End station number", "v:End_ID")
        .as("Bike number", "v:Bike")
        .as("Member type", "v:Member_Type")
).remote("https://terminusdb.com/t/data/bike_tutorial.csv")
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
    WOQL.idgen("doc:Bicycle",["v:Bike_Label"],"v:Bike_URL"),    WOQL.concat("v:Start_ID - v:End_ID @ v:Start_Time","v:J_Label"),
    WOQL.concat("Bike v:Bike from v:Start_Station to v:End_Station at v:Start_Time until v:End_Time","v:Journey_Description")
];
//combine inputs
const inputs = WOQL.and(csv, ...wrangles)
//generate data to be inserted
const inserts = WOQL.and(
    WOQL.insert("v:Journey_ID", "Journey")
        .label("v:J_Label")
        .description("v:Journey_Description")
        .property("start_time", "v:ST_Cast")
        .property("end_time", "v:ET_Cast")
        .property("duration", "v:Duration_Cast")
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
//Combine inputs and inserts with when clause
WOQL.and(inputs, inserts);
```

don’t forget to put in the commit message, then click `Run Query`.

- - -

# Query The Data

Back to the query page — again input into the WOQL.js query builder:

```js
WOQL.select("v:Start", "v:Start_Label", "v:End", "v:End_Label").and(
	WOQL.triple("v:Journey", "type", "scm:Journey"),
	WOQL.triple("v:Journey", "start_station", "v:Start"),
	WOQL.opt().triple("v:Start", "label", "v:Start_Label"),
	WOQL.triple("v:Journey", "end_station", "v:End"),
	WOQL.opt().triple("v:End", "label", "v:End_Label"),
	WOQL.triple("v:Journey", "journey_bicycle", "v:Bike")
)
```

Click on `Run Query`and you should see the query returning a table:

![query returns a table](/blog/assets/uploads/results.jpg)

<!--StartFragment-->

Here we used `select` to filter out the variables (those starting with `v:`) that appear in our output. Then we used `and` to link all the conditions we want to include, as you can see there are lot’s of `triples` to be conditioned. The ones with `opt()` means that they are optional — it will be ignored if that data is missing (instead of returning an error — very handy).

The query can be translated as below:

1. select all the `Journeys`
2. and all the `start_stations` of all the `Journeys` (let’s call them `Start`)
3. and, if any, all the `labels` of the `start_stations` (let’s call them `Start_Label`)
4. and all the `end_stations` of all the `Journeys` (let’s call them `End`)
5. and, if any, all the `labels` of the `end_stations` (let’s call them `End_Label`)
6. and all the `journey_bicycles` of all the `Journeys` (let’s call them `Bike`)

We can click the `table` drop down menu in the top right and get a `graph` view of the results:

![result as graph](/blog/assets/uploads/results-graph-view.jpg)

You can go back to ‘DB Home’ to view all of the information about the database, including all your commit messages:

![click on the view button](/blog/assets/uploads/db-home-post.jpg)

Of course, with TerminusHub there is a way to short circuit this tutorial. You can go back to the start, click on ‘Clone’ and copy the bike database in full. You can then immediately run your queries. No need for any interim steps. Life is getting easier!

Terminators first, always!