---
layout: post
title: "Stuffing all of Human History into a Single Knowledge Graph"
date: 2020-03-01
description:
cover_image: https://miro.medium.com/max/2110/1*2Cm8rFu45ogPIm8C6fsnXg.png
author: Cheuk Ting Ho
tags:
  - Database
  - Dataops
  - Knowledge Graph
  - Open Source
  - Data Pipeline

---
The history of the world sounds amazing, unimaginably massive. Thanks to academics and scholars, we know a lot about the life of our ancestors and thanks to the progress of technology, we now have lots of open data available on the internet. To gather and organise a knowledge graph covering large-scale human social evolution is a huge challenge.

## Seshat: Global History Databank

Named after the Egyptian goddess of wisdom, knowledge, and writing, [Seshat](http://seshatdatabank.info/), the Global History Databank aims to bring together the most current and comprehensive body of knowledge about human history in one place.

Researchers divided the world into a series of Natural Geographic Areas and collected datasets describing all of the specific historical societies or polities that had existed in those areas. By storing the data in a systematic way, different hypotheses about the rise and fall of large-scale societies across the globe and human history can be tested.

[http://seshatdatabank.info/](http://seshatdatabank.info/)

Here are some facts about the data:

* Academic collaboration seeking to create datasets describing every human society since 10,000BCE
* Major publications in 2018–19 in Nature and PNAS on the evolution of social complexity and religion based on a high-quality RDF DB
* Sprawling collection of datasets covering 1000s of variables, mostly human entered and error-prone, with a wide variety of formats and schemata

In 2018, the Seshat researchers have released several public datasets — they can be found here. Now, the challenge is, finding a proper tool that can store the databank in a semantic way that let the researchers query, analyse and collaborate with the data.

## TerminusDB: the Git-like Graph Database

Named after the Roman God of boundaries, TerminusDB is an open-source database that provides structured, semantically meaningful data for rapid delivery of data-driven applications.

With TerminusDB’s unified OWL model, a semantic schema can be created to capture all the complex aspects of the Seshat historical data model. Then we can write queries to ingest data to build an integrated knowledge graph. Based on an immutable append-only MVCC in-memory triple-store implemented in a custom rust storage library directed by a Prolog logic engine, it is efficient, safe and allows git like operations — push, pull, clone, merge and branch. With these advantages, multi-mastered cloud databases can be used to build automated pipelines for low-cost data publishing and distribution.

[https://terminusdb.com/](https://terminusdb.com/)

All these can be achieved easily with WOQL, the Web Object Query Language. It is a powerful language for querying the structure of Terminus Data, it can also be used to construct the schema and load data into the data graph. WOQL is also available in JavaScript client (WOQL.js) and Python client (WOQL.py) which allows programmatic operation of data and smooth integration of analytic and machine learning pipeline and applications.

![A portion of Seshat’s Knowledge Graph showing the evolutionary graph of historical societies](https://miro.medium.com/max/2596/1*Zgpez75_owkbzLoCpCsi0w.png)

We used WOQL to ingest all of the Seshat data into a great big knowledge graph and then generate graphs showing the evolution of hundreds of different properties of historical societies as they changed over time. The above graph shows all of the global historical timelines in Seshat, with each node representing a polity and the edges showing the ‘successor’ polity, the colour of the nodes represents the presence (green) or absence (red) of professional soldiers in each society. Stay tuned as we will soon be publishing a detailed tutorial showing you how you can build your own knowledge graph using TerminusDB and the Seshat dataset.

[Design a Query Language Client for Pythonistas and Data Scientists](/2020/01/20/design-a-query-language-client-for-pythonistas-and-data-scientists/)

If you want to know more, follow our publication to get the news about our talks, tutorials and community events.
