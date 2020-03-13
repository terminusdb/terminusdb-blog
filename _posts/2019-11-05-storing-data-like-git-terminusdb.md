---
layout: post
title: "Storing data like git — TerminusDB"
date: 2019-11-05
description:
image: https://miro.medium.com/max/8000/1*23WsqANJv5IS4ar-LGU2sw.jpeg
author: Cheuk Ting Ho
tags:
  - Git
  - Graph Database
  - Database
  - New Techonology
  - AI Tools

---
TLast week I have been to a meetup where a team of software developers from Dublin talks about their newest invention — [TerminusDB](https://terminusdb.com/), a graph database that stores data like git. My reaction was like, what? Imagine all the cool things you can do with git: time travel, branching and forking etc, you can do them with your data.

What’s more, TerminusDB is a graph database, meaning all data are stored in nodes and edges, makes it easy to extract relations between your data without all the hideous joins in relational SQL database.

In this article, let’s talk about the following:

1. What is a graph
2. How git works
3. How TerminusDB works

## What is a graph?

In mathematics, a graph is a structure amounting to a set of objects in which some pairs of the objects are in some sense “related”. The objects correspond to mathematical abstractions called nodes and each of the related pairs of vertices is called an edge [[1](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics))].

The most common type of graphs is directed graphs and undirected graphs. The difference is, the edges of a directed graph have direction from a node to another but the edges of an undirected graph do not — it just linked two nodes together.

![example of graphs](https://miro.medium.com/max/848/0*N7sgzupLg_9yXq_n.gif)

A lot of our data can be stored in a graph, and the usage of graph databases are more than what you think: knowledge graph in Google and NASA, recommendation engines, fraud detection in financial services, machine learning using graphs in deep learning and AI.

![recommendation via knowledge graph](https://miro.medium.com/max/1200/0*iOO7eoc9T2h1wt2u.png)

## How git works

For those who have not used git/ not familiar with git, let’s quickly recap how git works.

![explaining how git works](https://miro.medium.com/max/2026/0*R9RZrgyWQFMfSNae.png)

After you make changes in the repo, you will have to add your changes in the staging area, then you commit it. Git will store the new content and keep different commit as a series of events.

![get commit tree](https://miro.medium.com/max/1600/0*pooMsU1ggBkhzBjK.png)

Fun fact here, counter-intuitively, git does not store diff. Instead, it makes files into blobs and stores repo as a tree of pointers pointing to the blobs. It also stores the commits in sequences so it stores the history of the repo.

## How TerminusDB works

Here we see how similar TerminusDB is storing data in a chain of commits, layers in this case, just like git. However, it stores the diff instead (can see it having advantage in having a huge amount of data). For each layer, data, as a triplet consists of the edge and the nodes that it links, is stored either on the +ve plane or -ve plane (except the initial layer as everything was created).+ve means new data is added and -ve means that data is deleted.

![how TerminusDB works](https://miro.medium.com/max/1920/1*5Tww22yWeosH4T8UdwXq8w.gif)

When you make a query, you search from the most recent layer (the HEAD layer) back, if it is found in the +ve plane, it exists in the database; if it is found in the -ve plane, it existed at some point in time but was deleted so it can be concluded that does not exist anymore in the database. If it is not found on either, it goes one commit back and does the same thing until hitting the initial layer which finding the data or not can conclude whether it exists in the database or not.

![how TerminudDB stores data](https://miro.medium.com/max/1920/1*sRDGALZFqc19jRhtyHl0Bw.gif)

Making changes to the data in the database is like making a new commit in git. Internally, a layer builder would build the +ve and -ve plane (except the initial layer of cause) and then commit it to the series of layers, moving the HEAD forward. This makes time travel as simple as moving the HEAD just like git. Also, imagine you can make a new branch and create more layers and merge it back to the master branch.
There’s so much potential in this genius design. I am already thinking about how this can give lot’s of advantage in processing data. I have talked to the team and they are super nice. As the product is quite new, they are happy to answer questions or hear feedback from you. Here are their contact details:

Github: [https://github.com/terminusdb/terminus-store](https://github.com/terminusdb/terminus-store)

TerminusDB: [https://terminusdb.com](https://terminusdb.com)

Community forum: [https://community.terminusdb.com/](https://community.terminusdb.com/)
