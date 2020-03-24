---
layout: post
title: "Loading data in turtle RDF format to TerminusDB"
date: 2020-01-30
description:
image: https://miro.medium.com/max/6000/1*gE52jP9sN0JGx1ONoWcdqQ.jpeg
author: Cheuk Ting Ho
tags:
  - Rdf
  - Graph
  - Database
  - Tutorial

---
TerminusDB loves RDF! If you have not noticed already, Terminus uses the W3C’s OWL language to define the schema of its databases and OWL is normally encoded as RDFs.

--------

Loading data in turtle RDF format to TerminusDB is elegant and simple. I assume you have already read our beginner's tutorial — [My First TerminusDB Graph Visualisation](http://blog.terminusdb.com/2020/01/14/my-first-terminusdb-graph-visualisation-bike-share-data/) and have been started using [TerminusDB](https://terminusdb.com/). Now you want to load some data in turtle RDF format instead of the csvs like the tutorial.

Just as a revision, when loading the csvs in the tutorial, we use:

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
```

together with `wrangles` and `inserts` to load the data:

```js
const inputs = WOQL.and(csv, …wrangles);
WOQL.when(inputs, inserts)
```

So now, we want to load turtle RDF, so instead of the csv, we can do:

```js
WOQL.with("graph://temp",
          WOQL.remote("my_url",
                      {"type":"turtle"}),
          WOQL.quad("v:Subject",
                    "v:Predicate",
                    "v:Object",
                    "graph://temp")
)
```

That will read all the turtle data from my_url into a temporary in-memory graph `graph://temp` then we can use `WOQL.when()` to insert the items in the temporary graph to the database.

-------------

Here is an example, we are going to load in all the skills in the Language Skills Collection data at ESCO. We downloaded and unzip the file and put the `lauguage_skills_collection.ttl` in our mounted volume (See [this post](http://blog.terminusdb.com/2020/01/21/loading-your-local-files-in-terminusdb/) about how to do it).

## Create a Schema for the Data

Now create a new database and make a query:

```js
WOQL.when(true, WOQL.doctype("skill").label("Skills").description("Skills extracted from ESCO dataset")
)
```
Now click on the Schema button on the left, we can see that we have created this schema:

![Check the schema](https://miro.medium.com/max/3732/1*GWHcoThbJHtm_KV-pmk67g.png)

## Load data to a temporary in-memory graph for inspection

Now make a new query:

```js
WOQL.limit(50).with("graph://temp",
     WOQL.file("/app/local_files/language_skills_collection.ttl",
               {"type":"turtle"}),
     WOQL.quad("v:Subject",
               "v:Predicate",
               "v:Object",
               "graph://temp")
     )
```

We use `limit(50)` to preview the data in the turtle file:

![Preview the data](https://miro.medium.com/max/3444/1*BWkFZHpdgraSn3lxxYzD-g.png)

You can also make other queries by replacing the `WOQL.quad()`` above with other queries. For example, we want to grab all the class object, properties of those objects and their types:

```js
WOQL.limit(50).with("graph://temp",
     WOQL.file("/app/local_files/language_skills_collection.ttl",
               {"type":"turtle"}),
     WOQL.and(
          WOQL.quad("v:Class_Obj", "type", "v:Class", "graph://temp"),
          WOQL.quad("v:Class_Obj", "v:Property", "v:Prop_Obj", "graph://temp"),
          WOQL.quad("v:Prop_Obj", "type", "v:Prop_Type", "graph://temp")
         )
    )
```

And it will give you the following result:

![Query result](https://miro.medium.com/max/3444/1*07h4YrOBNiu9i4_SuJ-Vsg.png)

Now if we want to have a closer look at all the property `http://www.w3.org/2004/02/skos/core#prefLabel` of `http://data.europa.eu/esco/model#Skill`:

```js
WOQL.with("graph://temp", WOQL.file("/app/local_files/language_skills_collection.ttl", {"type":"turtle"}),
WOQL.and(
     WOQL.quad("v:Skill_obj", "type", "http://data.europa.eu/esco/model#Skill", "graph://temp"),
     WOQL.quad("v:Skill_obj", "http://www.w3.org/2004/02/skos/core#prefLabel", "v:Skills", "graph://temp")
    )
)
```

Which shows us all the skills in the data:

![Closer look of the data](https://miro.medium.com/max/3436/1*mlT5VE4NuAfgq1YNS2c6Xw.png)

## Insert data from in-memory graph to database

To make the insert query, we will use a `WOQL.when()` like we did for the csvs:

```js
const inputs = WOQL.with("graph://temp", WOQL.file("/app/local_files/language_skills_collection.ttl", {"type":"turtle"}),
WOQL.and(
     WOQL.quad("v:Skill_obj", "type", "http://data.europa.eu/esco/model#Skill", "graph://temp"),
     WOQL.quad("v:Skill_obj", "http://www.w3.org/2004/02/skos/core#prefLabel", "v:Skills", "graph://temp")
    )
);
const inserts = WOQL.insert("v:Skill_obj", "skill").label("v:Skills");
WOQL.when(inputs, inserts)
```

If you see the blue box there you know you have your data in the database:

![Blue box means execute success](https://miro.medium.com/max/2744/1*4JDMv8hAl2nEOQ6Ve8c_uw.png)

To be sure, let’s make a simple query to the database:

```js
WOQL.limit(50).star()
```

Voila! We have all the `skills`:

![Query result](https://miro.medium.com/max/3412/1*qP_TvGUomUYdhhiWdm82qw.png)
