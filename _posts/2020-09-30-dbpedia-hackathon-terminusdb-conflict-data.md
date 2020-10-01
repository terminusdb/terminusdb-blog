---
layout: post
title: "DBpedia Hackathon: TerminusDB & Conflict Data"
date: 2020-09-30T09:25:37.005Z
description: The TerminusDB Team Entry
image: /blog/assets/uploads/3480.1551362519.jpg
author:
  - value: luke
    label: Luke Feeney
tags:
  - database
---
## TerminusDB in the DBpedia Hackathon

Team TerminusDB took part in the **DBpedia Autumn Hackathon 2020** DBpedia is an extract of all of the structured data from Wikipedia. You can clone the full data set from [TerminusHub](https://terminusdb.com/hub/) and run all of the below steps for yourself (for non-technical users - like your correspondent - it is easy to do and feels like a type of magic when the queries emerge with data attached). 

We started with a great working seminar with well-known American archaeologist and anthropologist [Peter Peregrine](https://en.wikipedia.org/wiki/Peter_N._Peregrine). It was an illuminating session, we set out the context of the work and we got started on delivery (first 25 minutes is a discussion of Peter's work and the work of [Seshat: Global History Databank](http://seshatdatabank.info/), the remainder is working with TerminusDB and TerminusHub).

<iframe width="560" height="315" src="https://www.youtube.com/embed/LFhwORjn-Nc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Peter left us with some interesting ideas for how we might combine the the DBpedia data with Seshat in the context of his work on historical conflicts:

```
Here are some ideas for tasks to give to hackathon teams:

1) Using the Social Resilience cases identify episodes of famine and/or epidemic disease in the 50-year period following 535CE, with estimates of deaths, where possible.

2) Using the Social Resilience cases identify military conflicts in the period 500-600CE, with estimates of the number of combatants and the number of deaths, where possible.

Bonus: link references to their MARC records

As examples for the workshop, you might use the Seshat Latium case and illustrate pulling information and images from Perseus and shapefiles from Pleiadies.
```

## Into the Server

With the context set, we moved the action into the TerminusDB [Discord server](https://discord.gg/QFVUJEY). We used a [public Mural board ](https://app.mural.co/t/asymmetric6612/m/asymmetric6612/1600859397643/1cad3d5ff4cdb2809b41bb148aaf808353512157)to brainstorm and plot our course.

![](/blog/assets/uploads/mural.jpg "Planning to attach the Hackathon")

We knew that we were looking for conflicts between 500CE and 600 CE in the DBpedia data set and wrote this simple query to find and extract that information:

```
limit(10).triple("v:X", "type", "scm:MilitaryConflict")
  .triple("v:X", "scm:date", "v:Date")
  .re("(....)-..-..", "v:Date", ["v:All", "v:Year"])
  .cast("v:Year", "xsd:integer", "v:Year_Integer")
  .greater("v:Year_Integer", 500)
  .greater(600, "v:Year_Integer")
```

That query gave us this result:

![](/blog/assets/uploads/first-result.png)

As the object of the exercise is to enrich the Seshat conflict data between 500 and 600CE, we were able to quickly exclude the [Battle of the Eclipse](https://en.wikipedia.org/wiki/Battle_of_the_Eclipse) as it was fought between the Medes and Lydians in the early 6th century BCE. This is an issue when working with data like that taken from Wikipedia - it is inputted by non-professionals and there are errors. 

Here is a short video of the 500-600BCE query and our results

<iframe width="560" height="315" src="https://www.youtube.com/embed/m5pVvzN_oBU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

With the remaining four conflicts from DBpedia, we broke into teams and worked on each battle - first we checked the Seshat data (available to clone on TerminusHub) to see if the polities in Seshat matched the belligerents in the battles

This query returns the list of all the Seshat polities and their start and end date

```
triple("v:X", "type", "scm:Polity")
.triple("v:X", "scm:end", "v:End")
.triple("v:X", "scm:start", "v:Start")
.triple("v:X", "label", "v:Name")
```

Here is a video of the query running:

<iframe width="560" height="315" src="https://www.youtube.com/embed/9sqRwCiv-lI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

That actually provides too much information from Seshat and we found it easier to use a query that scans for patterns in the Seshat polity names and alternative names

```
triple("v:X", "type", "scm:Polity").triple("v:X", "scm:end", "v:End").triple("v:X", "scm:start", "v:Start").triple("v:X", "label", "v:Name")
.triple("v:X", "scm:alternative_names", "v:YYY").triple("v:YYY", "scm:string", "v:AName")
.or(
  re("Rom", "v:Name", ["v:Match"]),
  re("Rom", "v:AName", ["v:Match"])  
)
```

In the above case looking for the string `Rom` - you can change that to `Byz` for the Byzantines or `Sas` for the Sassanids. 

## Battle of Vézeronce

Of the remaining battles we need to find out if the relevant belligerents are polities in the Seshat databank and add the Battles if they are. For this write up, we'll use the Battle of Vézeronce and our exploration there (thou we added all the remaining battles too). 

The Battle of Vézeronce was fought by the Franks and the Burgundians in 524CE. 

![](/blog/assets/uploads/both-sides.jpg "Structured Extract from Wikipedia")

We first looked for the Franks using the scanning query and get this result:

![](/blog/assets/uploads/franks.jpg)

We can see that the Franks in that period are the Early Merovingian variety (Clovis, the first King of the Franks, has died and his sons are fighting over his former lands). The associated doc is:

`doc:frmerve`

This is the search for the Franks:

<iframe width="560" height="315" src="https://www.youtube.com/embed/tHNcilxJURk" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

On the other side of the Battle of Vézeronce is the Burgundians - those Scandinavian/East Germanic people who ended up in South Central France. 

Burgundy turns out to not be represented in the Seshat databank:

<iframe width="560" height="315" src="https://www.youtube.com/embed/6NWmvEX-as0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This is the view script that colors the timeline according the presence and absence of soldiers

```
view.height(800);
view.width(1200);
view.edges(["v:Predecessor", "v:Polity"], ["v:Polity", "v:Successor"])
view.node("v:Predecessor").text("v:Plab").icon({label: true})
view.node("v:Successor").text("v:Slab").icon({label: true})
view.node("v:Polity").text("v:Lab").icon({label: true})
view.node("v:Polity", "v:Presence").in("scm:absent").color([255,0,0])
view.node("v:Polity", "v:Presence").in("scm:present").color([0,255,0])
view.node("v:Polity", "v:Presence").in("scm:unknown").color([150,150,150])
```

Now we have established where we are in relation to the data in DBpedia and Seshat, we can now move to enriching the Seshat conflict data with the conflict data from DBpedia. 

Lets take a quick look at the evolution of professional soldiers in the polities in Seshat. It certainly links across to the battles and gives us more context on the type of political entities we are talking about. 

This query will give a timeline of polities and which ones had professional soldiers

```
WOQL.and(
        WOQL.triple("v:Polity", "type", "scm:Polity"),
        WOQL.triple("v:Polity", "successor", "v:Successor"),
        WOQL.triple("v:Polity", "predecessor", "v:Predecessor"),
        WOQL.triple("v:Polity", "label", "v:Lab"),
        WOQL.triple("v:Successor", "label", "v:Slab"),
        WOQL.triple("v:Predecessor", "label", "v:Plab"),
        WOQL.opt().and(
            WOQL.triple("v:Polity", "professional_soldiers", "v:Variable"),
               WOQL.triple("v:Variable", "epistemicState", "v:Presence")
        )
     )
```

Here is a video of the query running in the console:

<iframe width="560" height="315" src="https://www.youtube.com/embed/b2HKX69JI58" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This the full graph of the changing polities so you can appreciate the complex picture of constant change and upheaval:

![](/blog/assets/uploads/graph-of-developments.jpg)

To return to the Battle of Vézeronce, we use this query to insert the battle with our belligerent polity `doc:frmerve`

```
add_triple("doc:Battle_of_Vézeronce", "type", "scm:Battle")
.add_triple("doc:Battle_of_Vézeronce", "label", "doc:Battle of Vézeronce")
.add_triple("doc:Battle_of_Vézeronce", "scm:end", literal("524", "xdd:integerRange"))
.add_triple("doc:Battle_of_Vézeronce", "scm:start",  literal("524", "xdd:integerRange"))
.add_triple("doc:Battle_of_Vézeronce", "scm:belligerent", "doc:frmerve")
```

This enrichment is made to the conflict branch of the Seshat Databank which can then be kept as a separate area for study. Perhaps later we can merge the conflict branch into the main Seshat data. 

## Focus

We focused on the second of Peter Peregrine's questions:  

```
Using the Social Resilience cases identify military conflicts in the period 500-600CE, with estimates of the number of combatants and the number of deaths, where possible.
```

We were able to establish the conflicts in the period and enrich the data with that information. Unfortunately there was no estimates of the numbers of deaths in the DBpedia data. This will have to be extracted from other sources. 

## Generalized Script and Jupyter Notebooks

We used the TerminusDB Jupyter Notebook integration for exploration and visualization of the data. All our work can be seen in our GitHub repository:

<https://github.com/terminusdb-labs/dbpedia-hackathon-autumn-2020>

We were able to generalize a cross-database query that allows us, and other researchers, to take the full list of all polities from Seshat and then combine with all the battle from DBpedia. This script check names and dates to make sure there is a match and then carries out an auto insert. 

The query is available on GitHub:

<https://github.com/terminusdb-labs/dbpedia-hackathon-autumn-2020/blob/master/DBpedia_to_Seshat.ipynb>

This is the output visualized as a graph. The battles cluster around certain polities (Ottoman Empires I, II & III stand out in particular).

<iframe src="https://terminusdb-labs.github.io/dbpedia-hackathon-autumn-2020/all_polities_and_conflicts.html" width="100%" height="600px"></iframe>

## Historical Context

To contextualize the hackathon effort, we are looking at the[ Late Antique Little Ice Age ](https://en.wikipedia.org/wiki/Late_Antique_Little_Ice_Age)period that lasted for about 100 years after the [extreme events of 535-536](https://en.wikipedia.org/wiki/Extreme_weather_events_of_535%E2%80%93536) - think 2020 and multiply it by 10 and throw in an ice age - so we can study social resilience. That's important because it can help us to understand how societies fare when faced with extreme events.

In the examples we used, the Sassanids were no more by the end of the Little Ice Age. They along with the Eastern Roman or Byzantine Empire (another one that popped up in our battles) were so weakened by plague and internal and external conflicts in this period that they were unable to resist the onslaught of the Arabs. In the Battle of Qadisiyyah, the Muslim army defeated the Sassanids and Persia never recovered. This also led to the massive decline of the Zoroastrian religion. We can see this on the graph visualization - the Empire cuts some time after the Battle of Callinicum that we documented. 

This is the resulting Late Antique Ice Age graph for you to examine:

<iframe src="https://terminusdb-labs.github.io/dbpedia-hackathon-autumn-2020/ice_age.html" width="100%" height="600px"></iframe>

You can see the East Romans and the Sassanids and their battles during the period, including of course the Battle of Callinicum. We have enriched the data to give us a fuller picture.

On the other end of our polities, the Franks increase their strength through the Little Ice Age and emerge with an Empire under Charles 'The Hammer' Martel at the turn of the 8th Century.  

Documenting and enriching this data allows for deeper understanding of the drivers of resilience and will hopefully allow us to better understand how societies can prepare for cataclysmic change.