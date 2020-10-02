---
layout: post
title: "Write up of our Webinar - TerminusDB Internals"
date: 2020-04-17
description:
image: /assets/images/terminusdb-webinar-termunisdb-internals.png
author: Cheuk Ting Ho
tags:
  - Dataops
  - Database
  - Webinar

---
As enthusiastic Terminators, we all want to know how TerminusDB works. We want to know how with Prolog and Rust it works like magic: provide functions like git, allows rollback, branch and merge. Last week I asked our BDFL Gavin Mendel-Gleason -- creator of TerminusDB, technical and no so technical questions about TemrinusDB internals.

You can access to all our webinars as podcasts here: <https://podcast.terminusdb.com/> including last week's interview with Gavin, newest and previous episodes.

Here I highlighted some of the questions and answers.

---

## Why did you decide to build TerminusDB? Building a fully featured DB is hard and takes ages?!?!

Good question, and sometimes ask myself if it made sense.

We were working on Seshat Global History Databank, it was an ambitious project cause it consists of information about all historical societies, including a huge range of different factors about the polities that needed to be stored. My co-founder Kevin Feeney and I were trying to find the information architecture that would allow us to retain this very large amount and very diverse data and allow it to be changed over time. It's a very multi-disciplinary project, lots of different people, from archaeologist to mathematical modellers, would be involved. The data structure may need to be changed and we need to store rich data and have quality control, curate data and that it would allow all kinds of complexity. Including information disagreements, confidence in the data, whether it was derived or from direct observation. We tested a bunch of different databases to see if it would fit, but eventually started from scratch.

## Why Prolog? Prolog is not very popular choice in developing software.

It is a strange choice. Initially, we started in Java with Linked Data libraries. As the development goes, I keep feeling that I was implementing a Prolog interpreter to check data, so why not just use a Prolog. Inference checking is core to Prolog and even the slower implementation in Prolog turns out to be very fast in this case and it is well-designed to handle this job. If you are doing first-order logic, datalog type queries, the best way is to leverage Prolog. Prolog can do all the things that SQL can do and can add some complexity and perform recursive queries as well.

## What's the biggest advantage of TerminusDB? What makes it stands out?

Yeah, it is quite different from other databases. I would say it the holistic approach to data management. Because we have used a graph to represent things that allow for flexibility and you can use a large range of models. It uses a subset of OWL which provides a very rich modelling language - It allows you to model with inheritance. It's something that relational databases and management systems are lacking. It becomes problematic because they cannot derive a property form relational tables. Because we have a strong and flexible schema we can use our graph database as a document store - each document is described by the schema, so you can pull out from and update a JSON document in the database. In fact, it is all stored as a graph internally so it could be searched as a graph and give us the flexibility that is not commonly found in other databases. And because of the immutable storage system we also have the capacity to go back in time, query on former databases - you can time travel. That is another big advantage of immutable storage approach.

---

Visit <https://community.terminusdb.com/> and [follow us on Twitter @TerminusDB](https://twitter.com/TerminusDB) for the most updated news and webinar events.
