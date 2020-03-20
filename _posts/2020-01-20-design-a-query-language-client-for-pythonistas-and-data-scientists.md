---
layout: post
title: "Design a Query Language Client for Pythonistas and Data Scientists"
date: 2020-01-20
description:
image: /assets/images/cowduck_sitting_logo.png
author: Cheuk Ting Ho
tags:
  - Database
  - Open Source
  - Data Science
  - Query Language
  - Python

---
Having an effective yet user-friendly query language is one of the most important aspects of successful database technology. Here at TerminusDB, we aim to provide a good experience to our users whose main language is Python. Enter WOQLpy — a Query Language Client for Pythonistas and Data Scientists.

## What is WOQLpy?

WOQLpy comes with the Terminus Python Client which you can install with pip:

```
pip install terminus-client-python
```

Example usage:

```python
import woqlclient.woqlClient as woql
from woqlclient import WOQLQuery

server_url = "http://localhost:6363"
key = "root"
dbId = "pybike"
client = woql.WOQLClient()
client.connect(server_url, key)
client.createDatabase(dbId, "Bicycle Graph")
station = WOQLQuery().doctype("Station").label("Bike Station")
journey = WOQLQuery().doctype("Journey")
journey = journey.label("Journey")
journey = journey.property("start_station", "Station").label("Start Station")
journey = journey.property("end_station", "Station").label("End Station")
schema = WOQLQuery().when(True).woql_and(station, journey)
schema.execute(client)
```

The complete script can be found in GitHub.

As you see, the python client can execute a query created with WOQLpy. It is because WOQLpy lets you build WOQL — the original query language in JSON-LD format with methods in WOQLQuery object and send it to TerminusDB through the Python client. Without WOQLpy, queries need to be written in JSON-LD, for example, the same query above will be:

```json
{
  "when": [
    {
      "true": []
    },
    {
      "and": [
        {
          "add_quad": [
            "scm:Station",
            "rdf:type",
            "owl:Class",
            "db:schema"
          ]
        },
        {
          "add_quad": [
            "scm:Station",
            "rdfs:subClassOf",
            "tcs:Document",
            "db:schema"
          ]
        },
        {
          "add_quad": [
            "scm:Station",
            "rdfs:label",
            {
              "@value": "Bike Station",
              "@language": "en"
            },
            "db:schema"
          ]
        },
        {
          "add_quad": [
            "scm:end_station",
            "rdf:type",
            "owl:ObjectProperty",
            "db:schema"
          ]
        },
        {
          "add_quad": [
            "scm:end_station",
            "rdfs:range",
            "scm:Station",
            "db:schema"
          ]
        },
        {
          "add_quad": [
            "scm:end_station",
            "rdfs:domain",
            "scm:Journey",
            "db:schema"
          ]
        },
        {
          "and": [
            {
              "add_quad": [
                "scm:start_station",
                "rdf:type",
                "owl:ObjectProperty",
                "db:schema"
              ]
            },
            {
              "add_quad": [
                "scm:start_station",
                "rdfs:range",
                "scm:Station",
                "db:schema"
              ]
            },
            {
              "add_quad": [
                "scm:start_station",
                "rdfs:domain",
                "scm:Journey",
                "db:schema"
              ]
            },
            {
              "and": [
                {
                  "add_quad": [
                    "scm:Journey",
                    "rdf:type",
                    "owl:Class",
                    "db:schema"
                  ]
                },
                {
                  "add_quad": [
                    "scm:Journey",
                    "rdfs:subClassOf",
                    "tcs:Document",
                    "db:schema"
                  ]
                },
                {
                  "add_quad": [
                    "scm:Journey",
                    "rdfs:label",
                    {
                      "@value": "Journey",
                      "@language": "en"
                    },
                    "db:schema"
                  ]
                }
              ]
            },
            {
              "add_quad": [
                "scm:start_station",
                "rdfs:label",
                {
                  "@value": "Start Station",
                  "@language": "en"
                },
                "db:schema"
              ]
            }
          ]
        },
        {
          "add_quad": [
            "scm:end_station",
            "rdfs:label",
            {
              "@value": "End Station",
              "@language": "en"
            },
            "db:schema"
          ]
        }
      ]
    }
  ]
}
```

## Challenges in design

It’s nice to have WOQLpy, but there are some challenges in adopting WOQL in Python. For example, you may notice that there is a method in the example called `woql_and`, in WOQL, there are operations that are the same as the Python keywords, to name a few: and, or, not, as, from. In this case, we added the `woql_` prefix to make it valid in Python.

Another challenge is, as many of the methods we allow provide for more that one way of input, for example, with `.label` we allow users to input just a string “Journey” or a dictionary {“@value”: “Journey”, “@language”: “en”}, we have to do lots of checking in handling different user inputs. But it is totally worth it as we believe it will give a better usage.

## Make it better for Data Scientist

We are still at the beginning of our journey to make TerminusDB super useful for data scientists. One of the features that we have in mind to implement is to make our [cool graph visualization](https://medium.com/terminusdb/my-first-terminusdb-graph-visualisation-bike-share-data-39c59a1ab86a?source=friends_link&sk=2f877df5dcb2f00b9e4e85d5088f015e) embedded in Jupyter notebook. Right now it is possible to be embedded to websites thanks to our Javascript client. We should be able to do that same to Jupyter notebook for Python client.

Also, we think it would be nice to have a direct io with pandas data frame instead of passing it with csvs.

-----------------------

TerminusDB is open-source, we would like to invite you to try it out and give us feedback and ideas. Please feel free to join the slack channel and chat with the team: [http://bit.ly/terminusdb-slack](http://bit.ly/terminusdb-slack)
