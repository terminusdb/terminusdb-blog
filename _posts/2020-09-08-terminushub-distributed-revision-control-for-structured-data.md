---
layout: post
title: "TerminusHub: Distributed Revision Control for Structured Data"
date: 2020-09-08T10:40:07.199Z
description: Let's Get Building
image: /blog/assets/uploads/terminus-cow-up-color.png
author:
  - value: kevin
    label: Kevin Feeney
tags:
  - database
  - data
  - terminus
  - launch
---
Computers are fantastic things because they allow you to leverage much more evidence when making decisions than would otherwise be possible. It is possible to write computer programs which automate the ingestion and analysis of unimaginably large quantities of data.

If the data is well chosen, it is almost always the case that computational analysis reveals new and surprising insights simply because it incorporates more evidence than could possibly be captured by a human brain.

And because the universe is chaotic and there are combinatorial explosions of possibilities all over the place, evidence is always better than intuition when seeking insight.

As anybody who has grappled with computers and large quantities of data will know, it's not as simple as that. The joy of analysis and insight is often crushed beneath a mountain of tedious data sourcing, preparation, management and cleaning tasks - ugly ETL scripts that are a horror to maintain and the horror of trying to extract data with unknown character encodings from undocumented legacy systems - CP-1252 and its friends.

It shouldn't be like this; it doesn't have to be like this. Computers should be able to do most of this for us. It makes no sense that we are still writing the same simple and tedious data validation and transformation programs over and over ad infinitum. There must be a better way.

This is the problem that we set out to solve with TerminusDB. We identified two absolutely indispensable characteristics that were currently sorely lacking in data management tools.

The first one was a rich and universally machine interpretable modelling language. If we want computers to be able to transform data between different representations automatically, they need to be able to describe their data models to one another.

The second major missing requirement is effective revision control. Revision control technologies have been instrumental in turning software production from a craft to an engineering discipline because they make collaboration and coordination between large groups much more fault tolerant - and boy humans produce faults. The need for such capabilities is screamingly obvious when dealing with data - where the existence of multiple versions of the same underlying dataset is almost ubiquitous and with only the most primitive tool support.

In October 2019, we released version 1.0 of Terminus DB - it contained the culmination of 4 years of building out the data modeling capacity that we needed - the W3Cs web ontology language with a closed world interpretation.

We chose this solution firstly because OWL is by far the best thing humanity has yet produced in terms of a rich, machine interpretable data modelling interchange format. It is essentially first order logic with set operations - when it comes to platform interoperability nothing beats mathematics! And adding a closed world interpretation to OWL turns out to be surprisingly easy and semantically unproblematic (closed worlds are contained within open worlds).

In January 2020 with version 1.1. we released the first version of our immutable revision control storage layer - with many of the ideas shamelessly borrowed from git, but expanded significantly because when dealing with data, you need to distinguish between things like schema and instance data and keep them aligned. It turns out to require a significantly more complex structure of internal pointers, but it can be done!

In June 2020, we release version 2.0 - this included the revision control API - push, pull, branch and merge fully integrated with the database, query, and modeling engine. At this stage the database itself was more or less complete in terms of features, but there was one more critical and important point before we could say we had managed to deliver on our vision.

With distributed collaboration technology there is always a bootstrapping problem - it's no use having technology that allows you to collaborate on data in a peer-to-peer decentralized way unless there are other people out there to connect to and collaborate with. To overcome that problem, we needed to deliver an infrastructure that would allow people to get started, to share and find data and collaborators.

Today we release TerminusDB version 3.0 and at the same time we open the doors of [TerminusHub](https://terminusdb.com/hub/). The database is now fully integrated with the hub, allowing all TerminusDB users to share, store, publish and collaborate on databases with other users and do so at the grand price of free. With this release I think we can say that we have a product which delivers on our vision.

The product itself is only a means to an end. We built TerminusDB to take away the pain of building amazing evidence bases for computational analysis. Although we will relentlessly continue to focus on product and remove every pain point that shows up, we now have the type of tool that we wanted. Now we are going to build some truly wonderful data resources. 

Let's get building!