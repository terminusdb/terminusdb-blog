---
layout: post
title: "TerminusDB 1.1 — The Big Babushka"
date: 2020-01-16
description:
image: https://miro.medium.com/max/2820/1*y6pmHcjylW-YGQFbtlXicw.png
author: Luke Feeney
tags:
  - Developer Tools
  - Data
  - Database
  - Python
  - Rust

---
Another very big day for us — our first iteration on [TerminusDB goes out into the wild](https://github.com/terminusdb/terminus-server). We are calling it the **Big Babushka** release as it has some huge changes to the way we manage data and the operations we support — it could almost be a 2.0, but given that we are a relatively young DB, we want to save those release numbers for future exciting developments (I did see another DB on release 0.50.0 — I won’t name names, but come on folks, give us a 1.0!).

## Terminus-store

The major development is our new storage backend, terminus-store. With the introduction of the new store, TerminusDB takes a radical departure from traditional database architectures. Our approach has three parts:

1. We have a graph database with strong schema to retain simplicity and generality
2. The graph is implemented using succinct immutable data structures which enable more sparing use of main memory resources.
3. We adopt a delta encoding approach to updates (‘like git, but for data’) which provides the whole suite of revision control features: branch, merge, squash, rollback, blame, and time-travel facilitating CI/CD approaches on data.

This final point is crucial — with TerminusDB we can now use advanced CI/CD workflows in data operations. The large impact of git on the software development world can now be envisioned in the world of data. This is made possible by synergies between an immutable layered approach and the succinct data structure approach that we’ve used for encoding. The DevOps revolution becomes the DataOps revolution.

Just as with DevOps, DataOps means a focus on collaboration, automation and integration. There is a requirement to bridge what is today a chasm between huge reams of unstructured data on the one hand, and analysts looking for access to simple, clean, structured data on the other. DataOps is that bridge.

TerminusDB is a practical tool for enabling branch, merge, rollback, and the various automated and manual testing regimes which they facilitate on a transactional database management system while providing sophisticated query support

![showing how TerminusDB transections](https://miro.medium.com/max/1194/1*ADSnYco_5vlMbLT7eiT2Rw.png)

The new TerminusDB store is implemented in Rust. It is low level, memory safe, has no run time and is C compatible. We think Rust will be the language of the 2020s and we are delighted to be Rustaceans!

There is a [TerminusDB Technical Paper](https://github.com/terminusdb/terminus-server/blob/dev/docs/whitepaper/terminusdb.pdf) that goes into this in more detail for those interested.

![White Paper](https://miro.medium.com/max/1678/1*S9ho_dXwojark-UnLrRSeQ.png)

## WOQL

The Web Object Query Language — or WOQL — has developed considerably since our first release:

* Extended WOQL to greater functionality for string and arithmetic processing
* Better date handling in WOQL
* Improved type casting in WOQL
* Improvements to query processing speed

The growing maturity of WOQL provides a strong platform to import, clean and query complex data. WOQL remains the best graph query language available today.

WOQL itself is specified as a JSON document format. All transmission of data uses the canonical JSON format, but this is generally annoying to write, hence WOQL.js is the preferred means by which human users can write queries manually. In developing WOQL we wanted to make querying graph data easy to learn, understand, and use, but also build on the power and functionality of other standard data access languages. As we can see from the TerminusDB tutorials, WOQL is a highly composable and flexible graph query language. Graphs are very generically applicable to a much wider range of data than table structured data. Ultimately, the flexibility of graphs with well specified schema, will show their value.

## Terminus — Console

The new Console significantly builds on the simple JavaScript client application that released with 1.0.

In the query interface, you can now switch between WOQL.js and JSON. You can also add new queries to the original query — as inspired by the Jupyter Notebook functionality you know and love.

![TerminusDB console](https://miro.medium.com/max/3474/1*uScpmhNCu0UwUR30Y_RGAA.png)

Once you have run your query, we have a view interface to easily change the view on the output

Throughout the console you now have the ability to toggle between table and graph view. On the query page, on the document page and on the schema page, you can see as a graph. This is super helpful when you are trying to see what’s in a complex schema, or trying to derive insight from a query.

The new Console gives user a quality interface for managing and querying TerminusDB. It ships with TerminusDB.

## Python Client

With the release of the TerminusDB python client, python users can query data and interact with TerminusDB through the API and WOQL.py. We will continue to work to make TerminusDB more accessible for the python community (firstly, by improving our documentation!). TerminusDB is the database for data people, and there are no people that are more data than Pythonistas!

## What’s Next

We are working hard on our collaboration and DataOps features. They should be available by the end of February 2020.

We’ll be pushing out documentation as fast as we can. There is lots of new stuff available, including a couple of tutorials, but it is never enough!

Remember we are open source (GPLv3) now and forever.

(And for anybody wondering, that’s CowDuck, our new mascot)
